

# Enumeration 
## emails
- https://hunter.io/
## Subdomains enumeration
![](/media/subdomain_enum.png)
#### main links
- https://search.censys.io/
- certspotter
- whoxy.com
- subdomainizer(find keys)
- subscraper
- shodan.io
- https://sslmate.com/help/reference/ct_search_api_v1
- crt.sh

#### asn enumeration
ASNs are assigned in blocks by Internet Assigned Numbers Authority (IANA) to regional Internet registries (RIRs). The appropriate RIR then assigns ASNs to entities within its designated area from the block assigned by IANA. 1
- https://youtu.be/p4JgIu1mceI?t=969
- hurricane electronic internet service bgp.he.net
- metabigor
```bash
echo 'name' | metabigor net --org -v
```
```bash
amas intel -asn <ASN number>
```
```
crt.sh/?q=.%25<what are you looking for as subdomain>.%25.<domain>.<top level domain>
crt.sh/?q=.%25api.%25.yahoo.com
crt.sh/?q=.%25.%25.%25.%25.yahoo.com
```
```bash
curl -s https://crt.sh/?q=%.$1 | sed 's/<\/\?[^>]+>//g' | grep $1
```
- sublist3r
  ```bash
  sublist3r -d 'irobot.com'
  ```
- httprobe
  - https://github.com/tomnomnom/httprobe
```bash
cat recon/example/domains.txt | httprobe --prefer-https
certspotter $TARGET | httprobe --prefer-https
```
#### Spiders
- hackrawler
- gospider
- 
#### subdomain brutforcing
- https://portswigger.net/burp/documentation/desktop/tutorials/enumerating-subdomains
------------


## Investigate the technology etc. 
- https://builtwith.com/irobot.com
- securityheaders.com
- nikto (warning don't use it if you need to be stealth)
  ```bash
  nikto -h https://irobot.com
  ```

## Other
- https://vulners.com

## Videos:
Sorted By importance
  - [Web App Testing: Episode 1 - Enumeration](https://www.youtube.com/watch?v=ZBi8Qa9m5c0&t=5566s)
    - very important video about scoping 
  - [The Bug Hunter's Methodology v4.0 - Recon Edition by @jhaddix #NahamCon2020!](https://www.youtube.com/watch?v=p4JgIu1mceI)
    - many different tools, spiders, asn enumeration
  - [Live Bug Bounty Recon Session on Yahoo (Part 1 - 7/14/2019)](https://www.youtube.com/watch?v=MIujSpuDtFY)
    - a lot of bash scripting and macros

