>[!info] Introduction 
>served under /var/www/html/create_ticket.php

>create_ticket.php is a POST form which allows the user to create tickets which would then be posted into the database. 

>[!danger] Attack Vectors

#Cross-Site-Scripting can be exploited within create_ticket.php input fields. The input fields does not sanitize for user inputs, allowing the user to type malicious payload/scripts into the fields.

>[!warning] Misconfigurations
1. The web server was not designed with `htmlspecialchars()` or any prepared statements which escapes and sanitize user inputs. 

>[!success] Mitigation
- Edit the code to use `htmlspecialchars()` instead of trusting the user's input.