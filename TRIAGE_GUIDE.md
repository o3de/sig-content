# O3DE Content SIG - Issue Triage Guide

## Table of Contents

[Overview](#overview)<br/>
[Issue Triaging](#issuetriaging)<br/>
[Process](#process)<br/>
[Triage Links](#links)<br/>
[Triage](#triage)<br/>
[Issue Workflow](#workflow)<br/>
[Labels for Controbitors](#labels)<br/>
[Stale Issues](#staleissues)<br/>
[Sig Assigned But No Action](#noaction)<br/>
[No Activity for 90 days](#noactivity)<br/>

<br/>

---




## Overview <a name="overview"></a>

This guide covers triaging issues for SIG-Content. Maintainers are encouraged to use and update this guide to ensure

any contributor to SIG-Content understands how issues are handled.




<br/>


##  Issue Triaging <a name="issuetriaging"></a>

Triaging is the process of ensuring a smooth intake of issues into the SIG-Content backlog. The goal is to make sure issues are both relevant to SIG-Content

and contain sufficient information for the community to take action.




The goals of this process are to ensure:

* Issue reorted are appropriate for SIG-Content

* Issues have clear information as to the nature of the problem or request

* Issue status are regularly maintained and updated until they are resolved

* Identify issues, feature requests and whether they fall under the SIG's charter





<br/>

## Process <a name="process"></a>

SIG-Content runs triage several times a week, please see the [calendar](https://lists.o3de.org/g/o3de-calendar/calendar?calstart=2022-01-20) 

Anyone is welcome to attend. Triage will be led by SIG chair/co-chair or maintainer.




Triaging aims to:

* Ensure issues in backlog are in a ready state for the community to take action upon

* Ensure load is balanced across SIG maintainers and participants

* Involve the SIG-Content community so all can participate




If time permits, on the day of triage and before the meeting, create a new thread in SIG-Content and add triage links below.

* Set the thread to automatically archive after 24 hours


<br/>

## Triage Links <a name="links"></a>

[SIG-Content Repo](https://github.com/o3de/sig-content)<br/>
[Filter for Issues](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fcontent+label%3Akind%2Fbug+label%3Aneeds-triage)<br/>
[Filter for Feature Requests](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fcontent+-label%3Akind%2Fbug+label%3Aneeds-triage)<br/>
[Filter for Pull Requests](https://github.com/o3de/o3de/pulls?q=is%3Apr+is%3Aopen+draft%3Afalse+label%3Asig%2Fcontent+review%3Arequired)<br/>

<br/>

## Triage <a name="triage"></a>

1. Disconnect from any VPN as it sometimes prevents Discord from working correctly

2. Join the SIG-Content discord voice channel

3. Start with the [Issues](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fcontent+label%3Akind%2Fbug+label%3Aneeds-triage)

4. Process all new issues, these should have labels `needs-triage` and `sig/content`

   4.1. Announce issue number and title to those in Discord voice channel so others can follow along

5. Ensure issue falls within the SIG-Content charter. If the issue falls outside of SIG-Content, remove the `sig/content` label and __comment on the issue__. If the correct SIG is known assign it to that SIG, otherwise, add the `needs-sig` label so the general O3DE issue triage meeting can triage and find the appropriate owners.

6. Review the issue and comments and see if it can be accepted

7. Review the technical implications. If a large change, issue should become an RFC, ask requestor to bring issue back as RFC or to make a feature request, if that would be more appropriate.

8. Assign a reviewer, if required, to handle follow-up comments, to reproduce the issue or ask questions.

9. If issue is rejected, assign commenter to reject issue. 

10. If issue is accepted, remove `needs-triage` label, set priority for issue and add `triage/accepted` label.


<br/>

If time permits:

* Review any open [blocker](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fcontent+label%3Apriority%2Fblocker) and [critical](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fcontent+label%3Apriority%2Fcritical) issues in the main repository:

  * Ensure priority is still valid

  * Assign any required reviwers or ask for updates

* Review any issue open for more than [90 days](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fcontent+sort%3Acreated-asc) and see if its still valid.

* Review any open `needs-triage` and `needs-sig` issues that may be for SIG-Network


<br/>

## Issue Workflow <a name="workflow"></a>

If you are assigned an issue to validate, work with requestor to get enough information to validate the issue.




If it can be reproduced then:

* Add comment and add `triage/accepted` labels

* Define priority with SIG Chair(s)

* Ensure issue is not a duplicate




If issue cannot be reproduced then:

* Comment on the issue and ask the requester for more information to aid reproduction, add `triage/needs-information`

* Or close the issue if both parties agree that this is not an issue/not reproducible.




If the issue is not clear or needs more information

* Comment on the issue and add the `triage/needs-information` label to show that the requestor needs to provide more information.


<br/>

## Labels for Contributors <a name="labels"></a>

Consider adding `good-first-issue` for new contributors for the SIG.




Consider adding `help-wanted` for issues that do not have immediate resourcing, and external contributors can likely contribute to.




<br/>


## Stale issues <a name="staleissues"></a>




SIG will periodically audit for stale items. If during triage, you encounter stale issues, use the guidance below to see if issue should be closed.


<br/>

## Sig Assigned But No Action <a name="noaction"></a>

If an issue with the SIG-Content label has had no updates for a while (14 days), follow up with the SIG, either through the Discord chat channel, triage or monthly meeting. Consider attending a SIG-Content meeting to raise the issue for discussion.


<br/>

## No Activity for 90 days <a name="noactivity"></a>

An issue can be removed if it has been abandoned by the requestor. Issues are considered abandoned if there has been no active for 90 days, especially if issue has had `triage/needs-information` label with no follow up from requestor.



<br/>
<br/>

_Part of this guide was informed by the [Kubernetes Triage Guide](https://github.com/kubernetes/community/blob/master/contributors/guide/issue-triage.md)_