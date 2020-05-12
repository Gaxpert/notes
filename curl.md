
***
title: curl
date: 2020-04-29 20:33:22+01:00
author: Gaxpert
***


# curl
## Get only status code
`curl -s -o /dev/null -w "%{http_code}" http://www.example.org/`
