>[!info] Introduction
>served under /var/www/html/login.php

> A normal HTTP-POST form used for logins.

>[!danger] Attack Vector

#SQLi
The user can issue a textbook example of a `1 OR 1=1'--` SQLi attack on the username field and it'll fetch the first user in the database, allowing them to login to the web page. 

SQL statement: 
```sql
"SELECT * FROM users WHERE username = '" . $username . "' AND password = '" . $password . "'"
```
The querying logic does not sanitize and checks for user input because of the lack of `prepare()`. 
What gets executed on the web server: 
`$result = $pdo->query($sql)`

So when the user enters `1' OR 1=1'-- `, anything after the username gets commented out and the statement becomes:
```sql
SELECT * FROM users WHERE username = "1' OR 1=1
```

>[!warning] Misconfigurations

- Adopting prepared statement with `prepare()` instead of using `query()`. 

>[!success] Mitigations

 - `query()` does not sanitize for user inputs and trust whatever the user inputs into the fields.
- `prepare()` acts like a prepared statement where it sanitize user inputs and blocks any incoming SQLi attack.