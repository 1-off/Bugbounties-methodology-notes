# BURB ALL THE THINGS
Both Burp and ZAP provide additional features other than the default web proxy, which are essential for web application penetration testing. Two of the most important extra features are web fuzzers and web scanners. The built-in web fuzzers are powerful tools that act as web fuzzing, enumeration, and brute-forcing tools. This may also act as an alternative for many of the CLI-based fuzzers we use, like ffuf, dirbuster, gobuster, wfuzz, among others.

## Methodology 

### 1- Intruder 
Burp's web fuzzer is called Burp Intruder
#### Target
locate our request, then right-click on the request and select Send to Intruder
#### Positions
The second tab, 'Positions', is where we place the payload position pointer, which is the point where words from our wordlist will be placed and iterated over. We will be demonstrating how to fuzz web directories, which is similar to what's done by tools like ffuf or gobuster.
```To check whether a web directory exists, our fuzzing should be in 'GET /DIRECTORY/', such that existing pages would return 200 OK, otherwise we'd get 404 NOT FOUND. So, we will need to select DIRECTORY as the payload position, by either wrapping it with § or by selecting the word DIRECTORY and clicking on the the Add § button```
#### Payload
'Payloads', we get to choose and customize our payloads/wordlists.









## Shortcuts 
- CTRL+I send it to Intruder
