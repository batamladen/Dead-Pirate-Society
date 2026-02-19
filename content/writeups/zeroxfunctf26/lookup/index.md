---
title: "Lookup"
author: "Skye"
layout: "single"
github: "https://pointerpointer.com/"
---


## Walkthrough

This event takes place on [ctf.0xfun.org](http://ctf.0xfun.org/), but you can easily find it by searching

---

---

At first I tried looking up stuff online which was pointless.

Then I used commands on my terminal

```bash
whois 0xfun.org ->  provide details to a registrar.   
dig ctf.0xfun.org TXT -> a DNS lookup tool
```

```bash
┌──(kali㉿kali)-[~]
└─$ whois 0xfun.org                    
Domain Name: 0xfun.org
Registry Domain ID: REDACTED
Registrar WHOIS Server: http://whois.porkbun.com
Registrar URL: https://porkbun.com
Updated Date: 2025-09-14T14:06:36Z
Creation Date: 2024-07-31T14:05:46Z
Registry Expiry Date: 2026-07-31T14:05:46Z
Registrar: Porkbun LLC
Registrar IANA ID: 1861
Registrar Abuse Contact Email: abuse@porkbun.com
Registrar Abuse Contact Phone: +1.8557675286
Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Name Server: cesar.ns.cloudflare.com
Name Server: dell.ns.cloudflare.com
DNSSEC: unsigned
URL of the ICANN Whois Inaccuracy Complaint Form: https://icann.org/wicf/
>>> Last update of WHOIS database: 2026-02-14T06:46:44Z <<<

For more information on Whois status codes, please visit https://icann.org/epp

Terms of Use: Access to Public Interest Registry WHOIS information is provided to assist persons in determining the contents of a domain name registration record in the Public Interest Registry registry database. The data in this record is provided by Public Interest Registry for informational purposes only, and Public Interest Registry does not guarantee its accuracy. This service is intended only for query-based access. You agree that you will use this data only for lawful purposes and that, under no circumstances will you use this data to (a) allow, enable, or otherwise support the transmission by e-mail, telephone, or facsimile of mass unsolicited, commercial advertising or solicitations to entities other than the data recipient's own existing customers; or (b) enable high volume, automated, electronic processes that send queries or data to the systems of Registry Operator, a Registrar, or Identity Digital except as reasonably necessary to register domain names or modify existing registrations. All rights reserved. Public Interest Registry reserves the right to modify these terms at any time. By submitting this query, you agree to abide by this policy.  The Registrar of Record identified in this output may have an RDDS service that can be queried for additional information on how to contact the Registrant, Admin, or Tech contact of the queried domain name.
                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ dig ctf.0xfun.org TXT

; <<>> DiG 9.20.11-4+b1-Debian <<>> ctf.0xfun.org TXT
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 36515
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;ctf.0xfun.org.                 IN      TXT

;; ANSWER SECTION:
ctf.0xfun.org.          300     IN      TXT     "0xfun{4ny_1nfo_th4ts_pub1cly_4cc3ss1bl3_1s_0S1NT}"
ctf.0xfun.org.          300     IN      TXT     "v=spf1 include:mailgun.org ~all"

;; Query time: 16 msec
;; SERVER: 24.200.243.189#53(24.200.243.189) (UDP)
;; WHEN: Sat Feb 14 01:47:02 EST 2026
;; MSG SIZE  rcvd: 148
```

Flag: 
0xfun{4ny_1nfo_th4ts_pub1cly_4cc3ss1bl3_1s_0S1NT}