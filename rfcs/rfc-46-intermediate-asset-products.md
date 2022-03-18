### Summary:
This feature proposes adding the ability for Asset Builders to output products as intermediates to be processed by other Asset Builders

### What is the relevance of this feature?
Asset Processor does not offer a real mechanism for chaining builders together to form an asset processing pipeline.  Currently, all files in the Asset Processor are processed in a single step: A source file is fed as input to a builder which produces a product file in the cache.  If another builder wants to consume this, the only way to fake chaining builders is to declare a job dependency on the other builder and have some source file to use as an additional input, since products cannot be an input.  Other alternatives involve creating monolithic builders that do everything in 1 step or building custom extension frameworks specific to the builder (such as how the scene builder works).  Each of these workarounds has their own drawbacks: Job dependencies require a source file, which is not feasible when the number of products is unknown/dynamic and can lead to wasteful reprocessing, monolithic builders are inflexible (they work exactly 1 way and cannot be easily reconfigured or parts reused), and builders should not have to re-invent the wheel every time to design their own extension framework.

#### Use Cases
* More fine grained dependencies to reduce rebuilding -  This solution will allow builders to be broken into smaller builders which can work in stages to allow more selective rebuilding.
* Procedurally generated source files - Users may wish to procedurally generate typical engine source assets, such as prefab files, which is not possible with the current pipeline.
* Dual authoring paths - Builders may wish to offer users the option of authoring a file by hand or using a node editor to generate a graph.  Instead of having to manually export the graph into a source format, this solution can be used to create a builder which automatically converts a node graph file into a source format consumed by another builder which creates the final output.
* Outputting common files to a central location - The shader pipeline produces debug symbol files which need to be included in order to be used.  By using the output override the shader builder can place all the symbol files in a single directory, making it much easier to include all the debug symbols.

### Feature design description:

This proposal will introduce a concept of intermediate assets to allow builder chaining.  A builder can flag an output product as either an intermediate product, a product asset, or both when it outputs the product.  When the AP sees these flags, it copies the product the appropriate locations: the cache and/or a special "intermediate asset" folder.  The contents of the intermediate assets folder don't need to be checked into source control since it will be regenerated just like any other product would.  The intermediate assets folder will be set as a watch folder, meaning the file scanner & file monitor will pick up files as if they are normal sources.  All the standard functionality of AP will continue to work as-is, treating the product as every other source file: it will be assigned a unique UUID (in the case this results in a collision with an existing product, the job will fail with an error), be monitored and fed into builders according to their file patterns.  Intermediate jobs will need to be tagged as platform-agnostic, see the technical design section for explanation.  A new always-enabled platform will be added which can be used to indicate a platform-agnostic intermediate job.

### Technical design description:
##### The UUID Issue
Intermediate jobs need to be run as platform-agnostic jobs because every intermediate needs to be assigned a unique UUID, as the Asset Processor will treat intermediates as sources and requires that all sources have a unique UUID (otherwise there's no way to know which file is being referred to).  Platform-agnostic jobs in this context means jobs which run on every enabled platform, as opposed to a platform-specific job which is only run on one platform.  More specifically, we need to avoid having IntermediateFileA which only produces jobs for the PC platform and IntermediateFileB which only produces jobs for the IOS platform.  While it may be possible to allow partitioning jobs by platform while still having unique UUIDs, we need a single ProcessJob call to create all the outputs.  The reasoning is as follows: downstream builders actually have the opposite requirement, which is that all the platform variants of an intermediate need to have the same UUID.  The current design of the engine is that Assets are referenced by UUID:SubId (aka AssetId).  When the engine tries to load an asset for a specific platform, it expects the AssetId to be the same across all platforms and uses the Asset Catalog  to look up the actual file.  If the UUID was different for each platform, it would break this system.  In order to get around this, builders that do need to output platform-specific outputs will need to A) create all their outputs in a single ProcessJob call so that they can B) output an additional extra "manifest" file which references all the platform variants that were output.  Downstream builders will then depend on the manifest file instead of the individual files.  This means there is a single file with a single UUID as the input, which can then be split into platform specific products as usual.  When another product file is referencing a product which was created from intermediates, the UUID it references will ultimately be that of the manifest file.

`AssetBuilderSDK::JobProduct` will be updated to add 2 new fields (this includes updating the serialization and behavior context):

- `m_outputFlags` - (default ProductAsset) allows builders to indicate what type of output is being produced.  This is a bit flag type enum, so the options can be combined
  -  IntermediateAsset - indicates a product should be copied to the intermediate product folder.
  - ProductAsset - indicates the builder wants the product to be copied to the product folder.
- `m_outputPathOverride` - (default empty) a scanfolder relative, folder only path to allow builders to override the default pathing, which is useful when a builder outputs a shared set of assets to be consumed by a later builder (see use case 4 above). For now this is not compatible with the ProductAsset flag as this would change existing assumptions that input rel path = output rel path for products and may break some existing code. If a use-case for this comes up in the future, we can address it then.

AssetBuilderSDK will need to have a "Common" platform added to it.  If a builder is outputting intermediate files, then in CreateJobs the builder would output only a single job with the Platform set to Common.  `PlatformConfiguration::InitializeFromConfigFiles` will add the "Common" platform as an always-enabled platform.  `PlatformConfiguration::PopulatePlatformsForScanFolder` will need to filter out the "Common" platform to prevent it from being included in the list of platforms a builder would normally loop over when creating jobs.  This work will only cover supporting the Common platform for intermediate assets, adding support for Common Product Assets is out of scope (an error check will be added to AssetProcessed_Impl to catch this).

The asset database Products table will be updated to add a `Flags` INTEGER column to store the above flag and any future flags we might add.

`AssetProcessor::JobDetails` will be updated to add a new `m_intermediateFolder` path which will be set by `AssetProcessorManager::AnalyzeJob`.  `m_destinationPath` can't be used instead because there might be a mix of product assets and intermediate assets; we also don't know at this point if the builder will report a product or intermediate asset.

`AssetProcessorManager::AnalyzeJob` will be updated to check the flags above and confirm the intermediate/products exist as expected (rather than assuming a product always exists)

`RCJob::CopyCompiledAssets` will be updated to check the ProcessJobResponse for the above flags and copy each product to the intermediate/product folders as appropriate, using the same relative pathing as would occur in the cache by default, unless otherwise specified by `m_intermediateOutputPath`.  An error will be output if the ProductOutput flag is specified with `m_outputPathOverride`.

`AssetProcessorManager::AssetProcessed_Impl` will be updated to verify the outputs of a job don't conflict with (have the same relative path as) an existing file, if so the job is failed and an error is shown.

Asset Processor Batch will need to be modified to enable the File Monitor.  Normally it doesn't run in APB since it isn't needed but it will be needed now to correctly process a chain that contains intermediate assets

Example:
![Example 1 (6)](https://user-images.githubusercontent.com/80125227/156651643-d51b5700-1e1f-4d58-a5ce-983e4492381e.png)

The Intermediate Assets folder will be a subfolder of the Cache, since the files inside are generated and should be treated like the other cache files and we avoid users having to update their `.gitignore` settings (the files shouldn't be checked in since they're generated).  A scanfolder configuration will be hardcoded in PlatformConfiguration.  The order of the scanfolder will have to be set to be lower than the projectroot scanfolder, since the AP computes the scanfolder for a file by looping the order-sorted list and finding the first (scanfolder)+(relative path) that is valid and exists.  Since the projectroot order is 0, and changing it might affect users that have created custom scanfolders, we'll use -1 for the intermediate asset scanfolder.

The Asset Scanner and AssetProcessorManager will need to be updated to more specifically check if a file is in the cache and not in the intermediate assets folder, to avoid ignoring the files in the intermediate assets folder.

It is possible that an intermediate asset might be output which has the same relative path as an existing non-intermediate asset.  Since the UUID is currently based on the relative path from the scanfolder, this could result in a duplicate UUID, which would mean one of the files would silently override the other.  As this would be confusing and likely unintended behavior, if an intermediate product is output with a duplicate relative path, the job will be failed and an error output explaining what happened.

To avoid loops, especially with copy builders where the input and output are the same type, AssetProcessorManager::ProcessBuilders will be updated to recursively search up the source ? product ? job chain to see if the same builder (stored on the job) it is about to run already exists in the chain, if so it will fail the job with an error about the processing loop.

Since the intermediate file copy happens at the same time as the product copy, there's no risk of work being lost if Asset Processor is terminated midway through, because the current job will not be marked as complete until after the copy (meaning a failure beforehand will cause it to re-run) and after the copy, the hash/modtime of the intermediate source will change, which the AP will pick up when it starts up again.

`AssetProcessorManager::DeleteProducts` will be updated to check for intermediate outputs and delete them if found.  This will automatically cause a recursive loop to clean up the entire chain: the deletion of the copied products will trigger a source delete notification for the intermediate product, which will delete its products, etc

`AssetProcessorManager::CheckDeletedSourceFile` will be updated to check if the source is in the intermediate assets folder, if so, it will check for a matching product in the Products table.  If one is found (and the file exists), then we assume someone manually deleted the file and we need to regenerate the product, so we'll look up the source of the found product and queue it for reprocessing.  If no entry is found, we assume it has been deleted and we delete the products of the intermediate.

### What are the advantages of the feature?
This feature will allow for builders to be broken up into smaller, chained builders, which in turn allows less rebuilding for data that hasn't changed and could allow moving slower work out of the CreateJobs phase and into the ProcessJob phase.  Sources for existing builders can also be procedurally generated without having to add support to the builder.

### What are the disadvantages of the feature?
There may be performance degradation due to increased job count.

### How will this be implemented or integrated into the O3DE environment?
This work will mainly involve making changes to the AssetBuilderSDK and Asset Processor.

### Are there any alternatives to this feature?
- Job Dependencies on Products offers a solution for cases dealing with a dependency on a product, but still requires the user to have a source input.  For cases where users want to chain builders together so the input for a builder is only the output from another builder, then the solution offered in this proposal is required.
- Create jobs from product assets - While this would enable our customers to create interesting chains of assets, it's not required for the immediate needs, and the problems to solve to support this are much larger in scope than the solution presented in this document. This is also not a one way door, we can always build this later.
- Update builders to function like the Scene Builder, where the intermediate information is stored in memory while processing, and the builder's output are just the end products. This solution basically means building a second asset pipeline inside of a builder, and it's not something that scales well. It's also generally disliked for the scene builder pipeline.
- Performance improvements in the asset processor, and the relevant builder jobs - The core problem being solved by Intermediate Asset Products is performance, the time it takes to process assets in response to making a change is very long. There have been several performance improvements done to asset processing recently, but at this point it will take a lot more effort to reduce asset processing time with traditional performance improvements, whereas Intermediate Asset Products provide builders a way to better manage processing time in response to making changes to assets.

### How will users learn this feature?
Asset Builder documentation will need to be updated to explain the feature.

### Are there any open questions?
- Should the initial work include adding read-only support in our tools?
  - Without this, users can make changes to intermediate files and end up losing their work unexpectedly