# ZAP Fuzzer
Very powerful for fuzzing various web end-points.

## Fuzz Location
The Fuzz Location is very similar to Intruder Payload Position, where our payloads will be placed.

## Payloads
The attack payloads in ZAP's Fuzzer are similar in concept to Intruder's Payloads, though they are not as advanced as Intruder's.
We can click on the Add button to add our payloads and select from 8 different payload types. The following are some of them:
```
    File: This allows us to select a payload wordlist from a file.
    File Fuzzers: This allows us to select wordlists from built-in databases of wordlists.
    Numberzz: Generates sequences of numbers with custom increments.
```
## Processors
We may also want to perform some processing on each word in our payload wordlist. 
The following are some of the payload processors we can use:
```
    Base64 Decode/Encode
    MD5 Hash
    Postfix String
    Prefix String
    SHA-1/256/512 Hash
    URL Decode/Encode
    Script
```
## Options
Finally, we can set a few options for our fuzzers, similar to what we did with Burp Intruder. For example, we can set the Concurrent threads per scan to 20.

## Identify succesful results
- Size Resp. Body which may indicate that we got a different page if its size was different than other responses
- RTT for attacks like time-based SQL injections, which are detected by a time delay in the server response.

## steps Parrot:
- menu 
  - search owasp-zap 
- quick scan -> input the target
- bottom bar -> Fuzzer -> new fuzzer -> HTTP -> select the website from dropdown menu
- click edit to edit the request and select the word you want to fuzz and click add. After add you can select the payload and use the option processor. Once it is how you want click start. 
