# Forbidden Paths - PicoCTF
## Challenge Description

* **Category**: SQL Injection, Web Exploitation
* **Description**: Can you get the flag?
We know that the website files live in `/usr/share/nginx/html/` and the flag is at `/flag.txt` but the website is filtering absolute file paths. Can you get past the filter to read the flag?
* **Link Resource**: `https://play.picoctf.org/practice/challenge/270?page=1&search=forbidden%20path`

## Solution

### Step 1 : Read the Description

* Based on the challenge description, we can assume that we are at this directory when we open the link
/usr/share/nginx/html/`

[engine](../../assets/forbiddenpath1.png)

### Step 2 : Execute

* We can get to the root directory by executing this command

```
../../../../flag.txt
```

* We can see a "flag" in the output shown

[hintssti1](../../assets/forbiddenpath2.png)

## Result

We are able to acquire the flag.

## Explanation

The vulnerability exploited here is a classic **Path Traversal (Directory Traversal)** attack. The application attempted to restrict access to files by filtering absolute paths, but it failed to prevent relative path sequences (`../`). This bypassed the intended restrictions and allowed disclosure of sensitive files from the server.
