# view_ticket.php

>[!info] Notes
> Part of the attack chain in [[Attack Storyboarding]]. Reached from [[dashboard.php]]; see also [[create_ticket.php]] and [[admin.php]].

## Features
> [!note]
> 1. A chatbox system where the user can chat with the agent.
> 2. User can still input attachment. ==🔴LFI==

## Attack Surface
> [!danger]
> 1. ==🔴LFI== is possible in view_ticket.php, but it's not recommended.
>     - Upload the malicious file/binary into the attachment and click on view attachment.

## Misconfigurations
> [!warning]
> *(not yet documented)*
