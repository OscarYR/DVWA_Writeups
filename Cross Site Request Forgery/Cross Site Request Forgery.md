# Cross Site Request Forgery

Cross-Site Request Forgery (CSRF) is a type of attack where an attacker tricks a user into performing unwanted actions on a web application where they are authenticated. It takes advantage of the trust a web application has in the user's browser. 

In simpler terms, CSRF exploits the fact that the browser automatically includes authentication information (like cookies or session data) in requests sent to a web server. If an attacker can convince a user to perform a request on a website they are already authenticated on (e.g., through a link or form submission), the request may be processed with the user's privileges, even though the user didn't intend to perform that action.

For instance, when a website like online banking is under the logged in status by victims, attackers can craft a malicious link or script that embedded on a website such as in an image that says “Click here to get special promotion” (e.g, <img src=`http://bank.com/transfer?amount=1000&to=attacker_account`/>). Once being clicked, the script will then be executed and transferred the money to attackers, and victim won’t even realize that. This is because CSRF utilize the “logged in” status, making the bank believes that the request is made by trusted user, thus allowing the transfer occurred.

