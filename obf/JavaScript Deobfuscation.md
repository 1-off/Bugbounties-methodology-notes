## The following topics will be discussed:
```
    Locating JavaScript code
    Intro to Code Obfuscation
    How to Deobfuscate JavaScript code
    How to decode encoded messages
    Basic Code Analysis
    Sending basic HTTP requests
```

First step, find the secret.js and deobfuscate this code with https://deobfuscate.io/, http://www.jsnice.org/, to obfuscate use this https://obfuscator.io/
```javascript
eval(function (p, a, c, k, e, d) { e = function (c) { return c.toString(36) }; if (!''.replace(/^/, String)) { while (c--) { d[c.toString(a)] = k[c] || c.toString(a) } k = [function (e) { return d[e] }]; e = function () { return '\\w+' }; c = 1 }; while (c--) { if (k[c]) { p = p.replace(new RegExp('\\b' + e(c) + '\\b', 'g'), k[c]) } } return p }('g 4(){0 5="6{7!}";0 1=8 a();0 2="/9.c";1.d("e",2,f);1.b(3)}', 17, 17, 'var|xhr|url|null|generateSerial|flag|HTB|1_4m_7h3_53r14l_g3n3r470r|new|serial|XMLHttpRequest|send|php|open|POST|true|function'.split('|'), 0, {}))
```

Once done this passage you will remove eval and rename the function flag (very important)
```javascript
function flag(p, a, c, k, e, d) { e = function (c) { return c.toString(36) }; if (!''.replace(/^/, String)) { while (c--) { d[c.toString(a)] = k[c] || c.toString(a) } k = [function (e) { return d[e] }]; e = function () { return '\\w+' }; c = 1 }; while (c--) { if (k[c]) { p = p.replace(new RegExp('\\b' + e(c) + '\\b', 'g'), k[c]) } } return p }
```
You will receive a string N2gxNV8xNV9hX3MzY3IzN19tMzU1NGcz which you need to decode from base64 at https://www.base64decode.org/. 
Once you have the 7h15_15_a_s3cr37_m3554g3 you will need to send it as a data.
```javascript
function generateSerial(){var flag="HTB{1_4m_7h3_53r14l_g3n3r470r!}";var xhr=new XMLHttpRequest();var url="/serial.php"; let urlx= xhr.open("POST",url,true);xhr.send("7h15_15_a_s3cr37_m3554g3"); xhr.onload = function(){console.log(this.responseText);}}
```
When you send it you will read the flag and submit it. 

This is a quite interesting challenge that will teach you a lot, none the less always give a check to the source code in case you miss anything. 
