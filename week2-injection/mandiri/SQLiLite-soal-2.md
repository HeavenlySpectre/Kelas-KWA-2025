# SQLiLite - PicoCTF

## Challenge Description

* **Category**: SQL Injection, Web Exploitation
* **Description**: Can you login to this website?
* **Link Resource**: `https://play.picoctf.org/practice/challenge/304?page=1&search=sql%20lite`

## Solution

### Step 1 : Try to "Bruteforce"

* We can try to input anything at the given login field by the system
* When entering something into the username and password, actually it exposed the sql queries used by the system

[bruteforce](../../assets/sqlilite1.png)

### Step 2 : Accurate payload

* We can use this payload `'--` at the username and just giving any password
* We can actually login to the page but cannot see the flag
* By inspecting the source code, we can finally see the given flag

[sqliliteflag](../../assets/sqlilite2.png)

## Result

We are able to log in to the website and acquiring the flag.

## Explanation

The vulnerability exploited here is a **Login Bypass using Inline Comment SQL Injection**. Because the application directly embeds user input into the SQL query without sanitization, injecting `'--` into the username field prematurely closes the string and comments out the rest of the query, including the password check. This forces the query to return a valid result as long as the supplied username exists or matches the query structure, allowing authentication without a real password. The exposure of raw SQL queries in error messages further confirmed the injection point and made exploitation easier. Ultimately, this lack of input validation and error handling allowed us to bypass login and retrieve the flag from the source code.
