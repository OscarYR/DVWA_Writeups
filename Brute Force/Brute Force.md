# Brute Force
## Low Difficulty
### What is Brute Force?

Brute force attack is a hacking method that uses trial and error to crack passwords, login credentials, or encryption keys. It can be automated using password cracking tools such as hydra and John the Ripper that comes with different wordlists and customizable options, making the cracking process faster and easier. In this case, since we already know the credential is ‘admin’ and ‘password’ as they are the one that we used for login, we will pretend that we don’t know the credential and try to brute force it.

<img src="./Screenshots/Screenshot1.png" width=80% height=80%>

### Exploit Process

To start, we will have to setup the burp suite and intercept the request. Burp Suite is a popular security testing tool used for web application vulnerability scanning. It provides several features for testing and analyzing web apps such as Intruder, Repeater, Scanner, and more.

<img src="./Screenshots/Screenshot2.png" width=80% height=80%>

Intruder helps to automates attacks like brute force or fuzzing to find vulnerabilities, such as weak login credentials. We will go to the Intruder tab, then select cluster bomb attack, which is for specifying multiple payload positions. Since we need the username and password, the former will be payload position 1, and the latter will be payload position 2.

<
