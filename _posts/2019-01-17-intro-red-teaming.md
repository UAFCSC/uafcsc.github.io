---
layout: post
title: Intro to Red Teaming
featured-img: redteaming
author: Chris Bailey
categories: [Lectures]
---

Hey everyone,

I hope you enjoyed your break. We're going to get this semester started with something I know a lot of people have been interested in learning: basic red teaming. I'll be covering Metasploit, credential theft, hash cracking, and potentially some password brute-forcing as well. This is the kind of stuff we're hesitant to teach early in the semester, but at this point I feel like I can trust you all with it. Aside from being generally fun and interesting, I consider it critical knowledge to be an effective blue team member -- if you don't know what the enemy is working with, it's a lot harder to defend against.

CCDC regional qualifiers are coming very soon. Next week we'll be going over incident response, and the week after will be our second practice competition, after which we'll be announcing team selections. We'll be sending more information about that over the next few weeks.

We hope to see you there.

Chris Bailey, CSC President

## Reference Materials
* [Metasploit source code](https://github.com/rapid7/metasploit-framework) (it's fairly well organized and understandable)
* [Offensive Security Metasploit guide](https://www.offensive-security.com/metasploit-unleashed/)
* [Nmap documentation](https://nmap.org/book/man.html)

## Lab Rules
* Don't change any passwords or anything else that is likely to prevent other members from being able to get in
* Only the 10.250.102.0/24 network is in scope, not counting .1 (the router)
* You will be awarded points for every compromised user, every compromised machine, and every flag (note that there aren't as many as a normal CTF-type lab, and they're not in flag format. Credentials are your main goal.)
* Don't spam Crackpipe with tasks. My GPU server isn't that powerful, and you're sharing it with everyone.

## Crackpipe
Commands can be placed in #random or private messaged to the bot. Command format is `crackpipe hashtype hash`. Supported hash types at the moment are `ntlm` and `unix6`.
