# Create A Gem

These workflows center around testing the basic functionality of the Project Manager gem creation tools.

## Common Issues

*   Dialogs do not open or progress as expected
*   Created gems do not immediately refresh in gem catalog list

## Workflows

| Workflow                               | Steps                                                                                                                                                                                                          | Expectations                                                                                                                                                                                                                                                                               |
|----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Launch Create a Gem experience         | 1.  Enter the Gem Catalog<br>2.  Select the hamburger menu in the top right corner<br>3.  Select "Create a New Gem"                                                                                            | Dialog changes to Create a New Gem step 1 "Gem Setup"                                                                                                                                                                                                                                      |
| Step 1 - Gem Setup Options             | 1.  Launch the Create a Gem experience<br>2.  Observe the options in Step 1                                                                                                                                    | Dialog displays options for:<br><br>*   Prebuilt Gem<br>*   GemRepo<br>*   Asset Gem<br>*   Default Gem<br>*   C++ Tool Gem<br>*   Python Tool Gem<br>*   Choose existing template                                                                                                         |
| Step 2 - Gem Details                   | 1.  Launch the Create a Gem experience<br>2.  Select a gem type/template<br>3.  Left click Next                                                                                                                | All fields can be edited<br>    *   Gem System Name<br>   *   Gem Display Name<br>    *   Gem Summary<br>    *   Requirements<br>    *   License<br>    *   License URL<br>    *   User-defined Gem Tags<br>    *   Gem Location<br>    *   Gem Icon Path<br>    *   Documentation URL<br> |
| Step 2 - Required Fields not satisfied | 1.  Launch the Create a Gem experience<br>2.  Select a gem type/template<br>3.  Left click Next<br>4.  On step 2 page, click next without entering information into some of the required fields (marked by \*) | *   User is unable to progress without required fields and a visual indicator suggests what is missing<br>   Required Fields:<br>    *   Gem Display Name<br>    *   Gem System Name<br>    *   License<br>   *   Gem Location<br>                                                         |
| Step 3 - Creator Details               | 1.  Launch the Create a Gem experience<br>2.  Select a Gem Type/Template<br>3.  Left Click Next<br>4.  Enter Details for Gem<br>5.  Left Click Next                                                            | *   User is unable to progress without required fields and a visual indicator suggests what is missing<br>   Required Fields:<br>    *   Creator Name<br>                                                                                                                                  |
| Gem name matches existing gem          | 1.  Create two gems with the same name, but different values for description, etc.                                                                                                                             | *   No issues in using both gems together                                                                                                                                                                                                                                                  |
| Created Gem displays in Gem Catalog    | 1.  All steps completed (1-3) for creating a new gem<br>2.  Go to Gem Catalog if not already there after creating                                                                                              | *   Gem can be found in the gem catalog and enabled/disabled                                                                                                                                                                                                                               |