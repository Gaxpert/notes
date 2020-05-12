
***
title: tools_websec
date: 2020-05-01 15:27:17+01:00
author: Gaxpert
***


# tools_websec
## Wordlists
(secLists)[https://github.com/danielmiessler/SecLists]
(wfuzz)[https://github.com/xmendez/wfuzz/tree/master/wordlist]
(fuzzdb)[https://github.com/fuzzdb-project/fuzzdb]
(dirb)[https://github.com/v0re/dirb/tree/master/wordlists]
## Domains
### assetfinder
Find domains and subdomains potentially related to a given domain. [assetfinder](https://github.com/tomnomnom/assetfinder)
#### Install
`go get -u github.com/tomnomnom/assetfinder`
#### Usage
`assetfinder [--subs-only] <domain>`
### httprobe
Take a list of domains and probe for working http and https servers. (httprobe)[https://github.com/tomnomnom/httprobe]
#### Install
`go get -u github.com/tomnomnom/httprobe`
#### Usage
`cat domains | httprobe`
### Get all urls (gau)
Fetches all urls from AlienVault's Open Threat Exchange, the Wayback Machine, and Common Crawl for any given domain. Inspired by Tomnomnom's waybackurls.
[gau](https://github.com/lc/gau)
#### Install
`GO111MODULE=on go get -u -v github.com/lc/gau`
```sh
printf example.com | gau
cat domains.txt | gau
gau example.com
```

## Path Finders
### ffuf
A fast web fuzzer written in Go.[ffuf](https://github.com/ffuf/ffuf)
#### Install
`go get github.com/ffuf/ffuf`
#### Usage


### gobuster
[gobuster](https://github.com/OJ/gobuster)
####

## Urls
### hakcheckurl
Checks url and reports status code
[hakcheckurl](https://github.com/hakluke/hakcheckurl)
#### Install
`go get github.com/hakluke/hakcheckurl`

## Javascript
### Deobfuscate javascript
[JSillery](https://github.com/mindedsecurity/JStillery)
[js-beautify](https://github.com/beautify-web/js-beautify)
[illuminatejs](https://github.com/geeksonsecurity/illuminatejs)
[jsnice](http://www.jsnice.org/)

## XSS
### Xss payloads
[payloads](https://github.com/pgaijin66/XSS-Payloads)
