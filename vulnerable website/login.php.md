# login.php

>[!info] Notes
> Part of the attack chain in [[Attack Storyboarding]]

## Functions
> [!note]
> 1. Text box/card where the user is able to enter their credentials.
> 2. Once logged in, it'll bring them to [[dashboard.php|dashboard page]].

## Attack Surface
> [!danger]
> 1. Use ==🟡dirbuster/gobuster== to enumerate through the web directory which is publicly exposed.
>     - `config.php` and `ldap_config.php` exposed on purpose (potential attack surface).
>     - ==🔴Path traversal== can be conducted here. `/etc/apache2/.conf` has been configured to allow path traversal.
> 2. The initial foothold to the web server would be through LDAP (protocol). There is a test account left in the server on purpose with a weak password, crackable with ==🟡Hydra== (offline password cracking).
