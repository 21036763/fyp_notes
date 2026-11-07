>[!info] Introduction
>served under /var/www/html/dashboard.php

>This page serves as the landing page after the user login. dashboard.php. 

1. The user can either view their open/closed tickets within this page.
2. Search for their tickets.

> [!danger] Attack Vectors

#IDOR
- The user can either search for their own ticket within the 'search tickets' box or enumerate the ticket_id to view tickets not created by them. 
- The user can also use the URL parameter {id} to enumerate the tickets within the ticket database. 
  ```url
  http://localhost.com/query_ticket={id}
  ```

#SQLi 
- The SQL statement was not using the proper mitigations like prepared statements or checking for user inputs. The SQL statement only checks if the user runs a simple `1 OR 1=1'-- ` in the "ticket search" function.
- Refer to [[Attack Storyboarding#Vulnerability 1 SQLi Attack]] for full attack chain.

>[!warning] Misconfigurations

#IDOR 
- RewriteRule was misconfigured to allow IDOR with the following code: 
  `RewriteRule ^files/(.*)`
- Insecure querying of SQL statement that does not check the ticket created_by and user requesting
  ```sql
  $sql = "SELECT t.id, t.title, t.category, t.status, t.created_at,
  u.last_login
	FROM tickets t
	JOIN users u ON u.id = t.created_by
	WHERE t.id = {$search}"
  ```

#SQLi 
- Insecure querying of SQL statement that does not check user inputs. 
>[!success] Mitigations

#IDOR 
- Implement strict access control within the SQLI statement to introduce a IF logic to only show the ticket if the created_by correspond with the requesting user.
#SQLi 
- Replace `query()` function with `htmlspecialchars()` or `prepare()` statements. 
- Instead of using `query()`, the web server should adopt `htmlspecialchars()` or `prepare()` in the SQL statement as these functions checks and sanitize user inputs instead of trusting and accepting whatever the user inputs into the querying field. 