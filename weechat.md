---
title: weechat
date: 2020-04-06 05:51:36+01:00
author: Gaxpert
---

# weechat

# setup

Generate key
`openssl ecparam -genkey -name prime256v1 -out ~/.weechat/ecdsa.pem`

Get the public key
`openssl ec -noout -text -conv_form compressed -in ~/.weechat/ecdsa.pem | grep '^pub:' -A 3 | tail -n 3 | tr -d ' \n:' | xxd -r -p | base64`

Add tor
`/proxy add tor socks5 127.0.0.1 9050`

Add server (freenode)
`/server add freenode-tor ajnvpgl6prmkb7yktvue6im5wiedlz2w32uhcwaamdiecdrfpwwgnlqd.onion`

Set tor proxy
`/set irc.server.freenode-tor.proxy "tor"`

Set SASL auth
```
/set irc.server.freenode-tor.sasl_mechanism ecdsa-nist256p-challenge
/set irc.server.freenode-tor.sasl_username "your_nickname"
/set irc.server.freenode-tor.sasl_key "%h/ecdsa.pem"
```

Connect (must have tor install and started)
`/connect freenode-tor`
