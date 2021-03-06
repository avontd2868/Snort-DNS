Snort-DNS
=========

Snort rules to detect local malware, phishing, and adult content by inspecting DNS responses from OpenDNS

I use OpenDNS Family Shield as my domain name service. It's an effective way of minimizing one popular avenue of infection and phishing attacks. DNS is bit like a phone book: it translates human-friendly website names into computer-friendly network addresses. OpenDNS takes advantage of this to provide a layer of protection: for most websites, it will tell my computer the real network address, but for domains known to host malware, phishing attacks, or adult content, OpenDNS instead gives the network address of a page with a warning message.

This project takes advantage of this fact. The local.rules file contains a set of Snort rules that identify DNS responses (packets from udp port 53 destined for a device on the local network), then inspects the payload. If the payload includes one of OpenDNS' blocked content landing pages, the rule will fire an alert.

For more details on how this works, and suggested improvements, see http://dnlongen.blogspot.com/2015/01/detecting-malware-through-dns-queries.html

1. The alert tells me the IP address of the offending computer or device, but not the domain name that was requested. I have Snort configured to store each packet that triggered an alert, and can use tcpdump to analyse the packets - but that's a bit of a pain. Do any readers know of a way to include payload fields from a DNS packet in the alert message?
 
2. I've identified 4 specific "warning page" DNS responses, but OpenDNS owns far more addresses that they may use for other conditions now or in the future. At a minimum, OpenDNS owns the ranges 67.215.64.0/19 and 204.194.232.0/21 -- all told, about 10,000 addresses. Snort supports matching IP ranges in CIDR notation for the source and destination, but my approach currently does a binary match in the payload. Do any readers have an example of a Snort rule that parses DNS packets into their component fields?
