

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
