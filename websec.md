
***
title: websec
date: 2020-05-03 15:28:55+01:00
author: Gaxpert
***


# websec
## Recon
### Get javascript files
We can use several tools like `gau` which searches on Openthread, Wayback Machine and Common Crawl
`~/go/bin/gau example.com -h | grep "\.js" | uniq | sort`

After we can check which javascript files still exist since we are searching in archive sites

