The goal of Bug Hunt is to discover bugs and vulnerablities in a target app/website.
(Tools )Ferox Buster , Burp Proxy , Intruder , Repeater.

#### Types of Bugs :
###### Information Disclosure:
    Allowing the outsiders to view some part of information in app/website like source code which must not be allowed. They can misuse it. It falls under crytpgraphic failure category in OWASP Top 10 (2nd Most Common Security Threat).


    ./feroxbuster.exe -u https://0ad2008d03482faa80b949f7005e0027.web-security-academy.net/ -w common.txt 
     (Command to run feroxbuster for any url to identify the hidden paths)

Burp Suite is the Penetration Testing Software. Burp Proxy helps in proxy functionality to intercept requests before they go to the web application. In a result we able to manipulate inputs that we cannot be seen or manipulated using the URL/normal Input Boxes that we see on the website.

anyurl.com/robots.txt (Will get you the list of route paths that are not supposed to be loaded by google,yahoo,bing etc.., you may get any bug files that are not supposed to be present in it!)

https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-via-backup-files 
(A great platform to practise identify Information Disclosure bugs) Access the Lab ! [We can able to view the hardcoded files via any backup files which we can get by simply appending /robots.txt in any URL]

https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-in-version-control-history 
(A great platform to practise identify Information Disclosure bugs) Access the Lab ! [We can able to view past changes in an app with Git so that we may get any sensitive information that've been deleted!]

https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-in-error-messages 
(A great platform to practise identify Information Disclosure bugs) Access the Lab ! 

https://raw.githubusercontent.com/danielmiessler/SecLists/395c945627196b6172e6e83a014c964f805e8d48/Discovery/Web-Content/common.txt (Common paths that may contain sensitive data!)

https://github.com/epi052/feroxbuster (Feroxbuster is a tool used to identify any sensitive content that may present in given paths in text file that contains Common Paths)

###### Broken Access Control:
    Allowing the unauthorized user to view certain information which broke the trust between people and app. It's a most common threat in OWASP. It has many sub categories such as Path Traversal Vulnerabilities,CSRF,IDOR and much more. Broken Access Control allows you to access/modify data beyond limits or permissions. Access/Modify info belongs to another user when we logged in. Access user info without logging in.

https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter
(A great platform to practise identify Broken Access Control bugs) Access the Lab ! [With this you can able to access Admin page and features when you're logged in as a normal user. This can be done by changing the Request Cookies section in Burp Suite by setting "Admin=true" in it. If you want it to occur automatically every time. You can change the proxy settings in  "Match and replace" section by the typing the name you want to match in "Match" box and the word you want to replace in "Replace" box. For example "Admin=false" in "Match" and "Admin=true" in "Replace". After every request you sent, it will set the cookie which you put in settings. Make sure you remove that when you're done so that it won't interfere in any tests in future!]

https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-unpredictable-user-ids
(A great platform to practise identify Broken Access Control bugs) Access the Lab ! [By creating two users, one user can able to view other user by replacing the id in request parameter.]

IDOR (Insecure Direct Object Reference) is a special case of BCA. It happens when app directly accesses user data without verification based on user input where we will be able to directly retrieve data that does not belong to us. When it is possible, it falls under IDOR Category.

https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references
(A great platform to practise identify Broken Access Control bugs) Access the Lab ! [This helps you understand IDOR where we can able to download that doesn't belongs to us but belongs to others. We can view that by intercepting in Burp Proxy and analyzing the request method and manipulating the path in it.]


Burp Repeater is the great functionality in Burp Suite where we send the request into Repeater and exact same is reflected in Repeater where we can send that request to get the response instantly without actually have to be post in Web Application and also able to manipulate the input by changing the user data.
https://portswigger.net/web-security/access-control/lab-user-role-can-be-modified-in-user-profile 
(A great platform to practise identify Broken Access Control bugs) Access the Lab ! [This helps you change the role of user with the help of Burp Repeator]]


HTTP Trace is also another coolest feature in Burp Suite where one can change the request method into TRACE to receive response from server with some additional headers like if we are trying to access anyurl.com/admin it will send /GET /admin route to the server where we can able to edit it as /TRACE /admin. This will download the response from server with additional headers. 

https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-authentication-bypass 
(A great platform to practise identify Broken Access Control bugs) Access the Lab ! [This will help you to access admin page when we add the modified information from header into the Match and replace tools in Proxy Setings in the Replace box. In this example I got "X-Custom-IP-Authorization: (my-ip)" where I changed the info in Replace Box as "X-Custom-IP-Authorization:  127.0.0.1" (localhost). The server will assume that the request is coming from local and it might think have access for admin.]

![[Pasted image 20230902233643.png]]

###### Injection Vulnerabilities:
    These are very dangerous. It allows to gain full control which can be easy for hackers to attack target app.



# Bug Hunt vs Pen Test

Bug Hunt has flexible way to select the target and discover bugs. We can work anytime we like.  But in a Pen Test, the target approaches us and asks  "I want you to test the security of my app". We don't have flexibilty to select the target and we should be work in alloted time limit. We have to exploit the bugs that able to hack the target in a Pen Testing.



