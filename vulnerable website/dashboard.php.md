# dashboard.php

>[!info] Notes
> Part of the attack chain in [[Attack Storyboarding]]. Reached after logging in via [[login.php]].

## Functions
> [!note]
> 1. Features a table which shows the current **outstanding** tickets.
> 2. A search bar to search through their tickets without any user validation/prepared statement.
> 3. The user is able to click on the ticket_id to view the current ticket in [[view_ticket.php]].

## Attack Surface
> [!danger]
> 1. Search bar is vulnerable to ==🔴SQLi attack== (backend search).
> 2. Enter the basic `1 OR 1=1` into the URL parameter or the search box.
> 3. Vulnerable to any type of SQLi attack, including UNION-based attack.
