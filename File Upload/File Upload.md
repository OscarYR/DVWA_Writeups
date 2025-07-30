# File Upload

A file upload vulnerability occurs when an application improperly handles files uploaded by users, allowing attackers to upload malicious files such as web shells, scripts, or executables that can be executed on the server or used to exploit the system. This can lead to remote code execution (RCE), data leakage, defacement, or server takeover.

Uploaded files represent a significant risk to web applications. The first step in many attacks is to get some code to the system to be attacked. Then the attacker only needs to find a way to get the code executed. Using a file upload helps the attacker accomplish the first step.
<br><br>

---

## Low Difficulty

### File Creation and Upload

Low difficulty code will not check the contents of the file being uploaded in any way. It relies only on trust. Based on this, we can create a simple PHP file with script that can execute command. After that, we upload the PHP file and access to the directory through URL.

<img src="./Screenshots/Screenshot1.png" width=80% height=80%>

<img src="./Screenshots/Screenshot2.png" width=80% height=80%><br><br>

### Exploit

After accessing the directory, we are given a blank page. But don’t forget earlier that we have specified the cmd command:

<img src="./Screenshots/Screenshot3.png" width=80% height=80%>

<img src="./Screenshots/Screenshot4.png" width=80% height=80%><br><br>

We can use different command to enumerate the server by just inserting to the URL:

> Command 1

<img src="./Screenshots/Screenshot5.png" width=80% height=80%>

<img src="./Screenshots/Screenshot6.png" width=80% height=80%><br><br>

> Command 2

<img src="./Screenshots/Screenshot7.png" width=80% height=80%>

<img src="./Screenshots/Screenshot8.png" width=80% height=80%><br><br>

### Simplify using Burp Suite's Repeater

We can even send the intercepted request to Repeater using Burp Suite to simplify the process of executing different command:

<img src="./Screenshots/Screenshot9.png" width=80% height=80%>

<img src="./Screenshots/Screenshot10.png" width=80% height=80%><br><br>

### Bonus #1 (Weevely)

Weevely is a web shell tool designed for post-exploitation on compromised web servers. It allows user to upload a small, stealthy PHP file (called an agent) to a vulnerable server. Once uploaded, this file acts as a backdoor, and the user can connect to it using the Weevely client to control the server remotely:

<img src="./Screenshots/Screenshot11.png" width=80% height=80%><br><br>

We can create a PHP file using the `generate` command and provide a password. The password will then be used to access the remote connection:

<img src="./Screenshots/Screenshot12.png" width=80% height=80%><br><br>

Upload the generated PHP file and access it using URL:

<img src="./Screenshots/Screenshot13.png" width=80% height=80%>

<img src="./Screenshots/Screenshot14.png" width=80% height=80%><br><br>

After that, we head back to Weevely and connect to the web shell using the password we created just now:

<img src="./Screenshots/Screenshot15.png" width=80% height=80%>

<img src="./Screenshots/Screenshot16.png" width=80% height=80%><br><br>

We now can enumerate through the system and execute many actions using Weevely:

<img src="./Screenshots/Screenshot17.png" width=80% height=80%>

<img src="./Screenshots/Screenshot18.png" width=80% height=80%>

> Tips: Use `:help` to see more about the commands can beused in Weevely.

<br><br>

### Bonus #2 (Msfvenom)

We can also use Msfvenom, a popular payload generator for reverse shell to penetrate the DVWA in Low difficulty:

<img src="./Screenshots/Screenshot19.png" width=80% height=80%><br><br>

Here we create a raw PHP reverse tcp file and upload to the server:

<img src="./Screenshots/Screenshot20.png" width=80% height=80%>

<img src="./Screenshots/Screenshot21.png" width=80% height=80%>

<img src="./Screenshots/Screenshot22.png" width=80% height=80%><br><br>

Next, we launch Msfconsole and set up the reverse tcp listener. Go back to the browser and access the file:

<img src="./Screenshots/Screenshot23.png" width=80% height=80%>

<img src="./Screenshots/Screenshot24.png" width=80% height=80%><br><br>

Now, we have an interpreter shell opened and able to enumerate the system using different commands:

<img src="./Screenshots/Screenshot25.png" width=80% height=80%>

<img src="./Screenshots/Screenshot26.png" width=80% height=80%><br><br>

In short, if file upload vulnerability presents, not only the attacker can access sensitive information of the web server or system but also use malicious script to further exploit the system.
<br><br>

---

## Medium Difficulty

In Medium difficulty, the code will check the reported file type from the client when its being uploaded. That is why our created PHP file can’t be uploaded since it only accepts JPEG or PNG files:

<img src="./Screenshots/Screenshot27.png" width=80% height=80%>

<img src="./Screenshots/Screenshot28.png" width=80% height=80%><br><br>

### Analyzes Request

Let’s intercept the traffic. We can see that the content type is `application/x-PHP`, which used as an indicator for deciding if the upload can be accepted:

<img src="./Screenshots/Screenshot29.png" width=80% height=80%><br><br>

If we just simply change the file extension, it won’t work as it is based on the `content-type` condition:

<img src="./Screenshots/Screenshot30.png" width=80% height=80%>

<img src="./Screenshots/Screenshot31.png" width=80% height=80%><br><br>

### Solution

If we change the `content-type` to `JPEG` or `PNG`, it will then be accepted by the web server:

<img src="./Screenshots/Screenshot32.png" width=80% height=80%>

<img src="./Screenshots/Screenshot33.png" width=80% height=80%><br><br>

Forward the modified request, and we are able to established a reverse shell connection in Medium difficulty:

<img src="./Screenshots/Screenshot34.png" width=80% height=80%><br><br>

---

## High Difficulty

In high difficulty, once the file has been received from the client, the server will try to resize any image that was included in the request. it means the server isn't just checking file extension or magic bytes. Instead, it likely uses a library (e.g., GD, ImageMagick, or getimagesize() in PHP) to actually process or resize the image and fail if the image isn't real.

### Hex Editing

To bypass this, we can hex edit our PHP meterpreter file to include PNG file signatures, so the server will process it as a valid image:

<img src="./Screenshots/Screenshot35.png" width=80% height=80%>

> Ref. https://en.wikipedia.org/wiki/List_of_file_signatures

<br><br>

First, we do an eight bytes space on top of the line for the PHP file, so we can fill the hex values later:

<img src="./Screenshots/Screenshot36.png" width=80% height=80%><br><br>

Then, we use the tool `Hexeditor` to edit the hex value, replace the top bytes of spaces (they are a bunch of 20s’) with the PNG signature value and save it:

<img src="./Screenshots/Screenshot37.png" width=80% height=80%>

<img src="./Screenshots/Screenshot38.png" width=80% height=80%><br><br>

Next, we copy the modified PHP file and make it a new one with `.jpg` extension so it will be processed by the server:

<img src="./Screenshots/Screenshot39.png" width=80% height=80%><br><br>

We are now able to upload the file:

<img src="./Screenshots/Screenshot40.png" width=80% height=80%><br><br>

The last thing we need to do is using another vulnerability which is `File Inclusion` to access the uploaded file:

<img src="./Screenshots/Screenshot41.png" width=80% height=80%><br><br>

A reverse shell connection has been successfully established:

<img src="./Screenshots/Screenshot42.png" width=80% height=80%><br><br>

### Bonus #1 (Exiftool)

"What if we just use a real `JPEG` or `PNG` file then insert the PHP reverse shell payload into the image?" you might ask. Yes, we can do that by using `Exiftool`.

ExifTool is a powerful tool to read, write, and edit metadata in a wide range of file types especially images (like `JPG`, `PNG`, `TIFF`), but also videos, PDFs, etc. We can use exiftool to insert PHP payloads and evade the file upload filters. 

First, we can simply get any image. Then, use `Exiftool` to embed a PHP reverse tcp script into the image:

<img src="./Screenshots/Screenshot43.png" width=80% height=80%>

<img src="./Screenshots/Screenshot44.png" width=80% height=80%>

> Ref. https://spencerdodd.github.io/2017/03/05/dvwa_file_upload/

<br><br>

Now we are able to upload the `JPEG` file with embedded script:

<img src="./Screenshots/Screenshot45.png" width=80% height=80%><br><br>

Remember to setup the listener, then use the same way to access the uploaded file (File Inclusion vulnerability, with low difficulty). Now we have an active meterpreter session opened:

<img src="./Screenshots/Screenshot46.png" width=80% height=80%>

<img src="./Screenshots/Screenshot47.png" width=80% height=80%><br><br>

---

## Conclusion

This challenge shows how improperly validated file uploads can lead to severe consequences such as **remote code execution**, **reverse shells**, and **server compromise**.

At **Low difficulty**, we uploaded malicious PHP scripts directly and executed them for command injection and shell access.

At **Medium difficulty**, we bypassed MIME-type checking by manipulating the `Content-Type` header during upload, demonstrating that trusting client-side file types is insecure.

At **High difficulty**, the challenge introduced image processing checks to detect fake images. We successfully bypassed this by:
- Injecting **PNG magic bytes** via hex editing
- Embedding payloads in image metadata using **ExifTool**
- Leveraging **File Inclusion** to trigger execution

These techniques are commonly used by attackers in real-world web attacks and highlight the need for **multi-layered file upload defenses**.
<br><br>

### Skills Applied:

- Web shell creation using PHP (manual, Weevely, Msfvenom)
- Reverse shell setup with `nc` and `msfconsole`
- Burp Suite request manipulation
- MIME-type header bypass techniques
- Basic binary manipulation using **hex editors**
- Exploiting image libraries' validation assumptions
- Metadata injection via **ExifTool**
- Combining **File Upload + File Inclusion** vulnerabilities
