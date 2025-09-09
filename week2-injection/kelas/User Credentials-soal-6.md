# User Credentials - OWASP JUICE SHOP

## Challenge Description

* **Category**: Injection
* **Difficulty**: ⭐⭐⭐⭐
* **Description**: Retrieve a list of all user credentials via SQL Injection.
* **Link Resource**: `http://localhost:3000/#/score-board?categories=Injection`

## Solution

### Step 1: Find the DB Schema

* We need to find the tables named like `Users` or `user` from the DB Schema

![finduser](../../assets/User%20credentials1.png)

* We also know the complete information of the `Users` table

```
username
email
password
role
lastloginIP
profileimage
totpSecret
isActive
createdAt
updatedAt
deletedAt
```

### Step 2: Payload for User Credentials

* We can use the payload below to know about the user credentials

```
non-existentvalue'))%20union%20all%20select%201,email,username,4,password,6,7,8,9%20from%20Users%20--%20
```

* The payload is a **Union-based SQL Injection**, specifically crafted to extract sensitive data (like usernames, emails, and passwords) from the Users table.

## Result

We are able to retrieve a list of all user credentials

![Result](../../assets/User%20credentials2.png)

## Explanation

The vulnerability exploited in this challenge is a **Union-based SQL Injection** on the search functionality. The input field was not sanitized, allowing us to close the original query and inject our own `UNION SELECT` statement. By aligning the number of selected columns with the original query, we successfully retrieved sensitive fields (`email`, `username`, and `password`) from the `Users` table. This works because the application directly displayed query results without filtering or validation. As a result, we were able to exfiltrate the complete list of user credentials, demonstrating a critical breach of confidentiality due to improper input handling and lack of parameterized queries.