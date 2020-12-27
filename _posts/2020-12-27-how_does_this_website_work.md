---
layout: post
title:  "How does this website work, basic network utlilities"
date:   2020-12-27 20:05:43 -0500
categories: Web
---


So I've been meaning to start a personal blog for awhile, I do a lot of little side projects for fun and I think it would be good for me to start documenting them.

One thing I like to do is dig into something until I understand it. This website is a Github Pages site, I figured I would dive into what that means, who owns it, what server it's on etc.

This is also an excuse to dig into basic network tools, I don't have much professional experience with the internet at a technical level, so I should learn a lot.

## Ping

Ping is one of the most basic web utilities there is, you just give a `ping <ip.address.or.host.name.you.want.to.ping>` from your terminal, and the utility will attempt to "ping" the ip address or host name that you specify. This is the goto utiltiy to just see if something is online, it works just as well over the internet as it does for local networks (as long as the local network is using IP). When I develop more complex robotic systems, this is something I use all the time when debugging, to ask "is the device I'm trying to talk to even reachable?".

How does it work? the ping utility uses something called the "Internet Control Message Protocol" ICMP, which is something baked into the internet protocol suite. This protocol operates separately from TCP/UDP, in that it is not using those protocols to send/receive data. Also unlike those other two protocols, it is not associated with a port number.

Anyway, this protocol is used extensively on the internet for things like reporting error messages or requesting information. One such use is to request an "echo" that is, just send this data back where you got it from. That is precisely what the ping utility does, it sends out one of these ICMP echo packets to the address/host you specify, and prints to the screen if the packet was sent back, and how long it took. Let's take a look what happens when I ping this site.

```
sgillen@smbp-9 ~> ping sgillen.github.io                                                                                                                                                             (base) 
PING sgillen.github.io (185.199.111.153): 56 data bytes
64 bytes from 185.199.111.153: icmp_seq=0 ttl=56 time=23.907 ms
64 bytes from 185.199.111.153: icmp_seq=1 ttl=56 time=17.528 ms
64 bytes from 185.199.111.153: icmp_seq=2 ttl=56 time=14.413 ms
64 bytes from 185.199.111.153: icmp_seq=3 ttl=56 time=14.483 ms
^C
--- sgillen.github.io ping statistics ---
4 packets transmitted, 4 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 14.413/17.583/23.907/3.862 ms
```

Great, so we can see that sgillen.github.io was resolved to some address (185.199.111.153), and that we are able to receive ping packets from that address reliably. 

15ms does seem like a pretty short time to receive packets, let's figure out where in the world the server hosting this site is located.


## Whois

To do this we can use whois, another "basic" network utility, it is also, according to Wikipedia, the name for the protocol used by the whois utility. Essentially it is a utility / protocol that allows us to lookup information about a specific domain / IP address. Whois servers are hosted mostly by internet registries[https://en.wikipedia.org/wiki/Domain_name_registry] and registrars[https://en.wikipedia.org/wiki/Domain_name_registrar]. We can query these servers for information about which person or organization has registered a particular domain or ip. Let's see what happens when we search for the IP we found associated with our website above.


```
sgillen@smbp-9 ~> whois 185.199.111.153
% IANA WHOIS server
% for more information on IANA, visit http://www.iana.org
% This query returned 1 object

refer:        whois.ripe.net

inetnum:      185.0.0.0 - 185.255.255.255
organisation: RIPE NCC
status:       ALLOCATED

whois:        whois.ripe.net

changed:      2011-02
source:       IANA

# whois.ripe.net

inetnum:        185.199.108.0 - 185.199.111.255
netname:        US-GITHUB-20170413
country:        US
org:            ORG-GI58-RIPE
admin-c:        GA9828-RIPE
tech-c:         NO1444-RIPE
status:         ALLOCATED PA
mnt-by:         RIPE-NCC-HM-MNT
mnt-by:         us-github-1-mnt
created:        2017-04-13T15:36:35Z
last-modified:  2018-12-14T10:48:39Z
source:         RIPE

...


```

There was more information but I cut it off for brevity, importantly I can see that the status is "ALLOCATED PA", which I am taking to mean that the address has been allocated to a server which resides in the state of Pennsylvania. Given than I am currently near Washington DC, this tracks with the ping I was seeing before. Cool!

## Traceroute

I was also curious about the route my packets where taking to this server. To do this we use a utility called traceroute. Traceroute is actually pretty interesting and I wanted to talk about how it's implemented. Traceroute leverages something called TTL values. TTL stands for Time To Live, it is a parameter of an IP packet which determines how many "hops" the packet can go through before being discarded. Every time a router forwards a packet, it decrements the TTL value by one, if a router decrements the value to zero, the packet is discarded, and an ICMP packet is sent to the original sender to inform them of the timeout. So what traceroute does is send out packets to our server with different TTL values. So for example a packet with TTL of 1 is sent out to the target server. It is sent from our computer to our gateway, the gateway decrements the TTL by one, finds that the new result is zero, and therefore sends back to our computer an ICMP packet. Traceroute then logs the IP of the gateway as the first stop in our route, and sends out a packet with TTL of two. (I'm sure I'm oversimplifying a little, for efficiency probably all the packets are sent out at once, but the idea is the same). This allows us to build up the entire route from our computer to the server, pretty cool right?

Anyway, let's see what the route to our website is:


```
sgillen@smbp-9 ~> traceroute -P ICMP sgillen.github.io
traceroute: Warning: sgillen.github.io has multiple addresses; using 185.199.111.153
traceroute to sgillen.github.io (185.199.111.153), 64 hops max, 72 byte packets
 1  10.0.0.1 (10.0.0.1)  5.490 ms  3.909 ms  4.101 ms
 2  96.120.107.93 (96.120.107.93)  13.300 ms  12.576 ms  11.493 ms
 3  24.124.177.121 (24.124.177.121)  10.558 ms  13.021 ms  11.801 ms
 4  68.86.205.173 (68.86.205.173)  17.781 ms  13.238 ms  13.949 ms
 5  96.110.235.17 (96.110.235.17)  13.988 ms  27.053 ms  13.218 ms
 6  be-31441-cs04.ashburn.va.ibone.comcast.net (96.110.40.29)  16.858 ms  15.424 ms  17.537 ms
 7  be-2411-pe11.ashburn.va.ibone.comcast.net (96.110.32.134)  16.405 ms  16.251 ms  18.185 ms
 8  50.248.118.58 (50.248.118.58)  14.743 ms  15.134 ms  14.980 ms
 9  185.199.111.153 (185.199.111.153)  15.272 ms  15.585 ms  20.803 ms
```

Cool, but maybe not terribly interesting, maybe we can run whois on all of these lookups? I did this with a tiny amount of unix sorecery: `traceroute -P ICMP sgillen.github.io | awk '{print $3}' | tr -d '()' | xargs whois > whois_log.txt` Which takes the output from before, runs it through awk to extract just the ip, runs that through tr to remove the parens, and then feeds the result one line at a time to whois, whos output is all redirected to a text file. 

I'll save you the output file, it's rather long. The long and short of it is that every router along the way is owned by Comcast, not terribly exciting but maybe that's interesting in it's own way. 

## Conclusions

Anyway, hope you enjoyed this dive into basic net utilities, next I'll probably dig more into the server the website is running on itself.
