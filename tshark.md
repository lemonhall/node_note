
ip.addr==10.0.0.1  && ip.addr==10.0.0.2 [sets a conversation filter between the two defined IP addresses]

http or dns [sets a filter to display all http and dns]

tcp.port==4000 [sets a filter for any TCP packet with 4000 as a source or dest port]

tcp.flags.reset==1 [displays all TCP resets]

http.request [displays all HTTP GET requests]

tcp contains traffic [displays all TCP packets that contain the word ‘traffic’. Excellent when searching on a specific string or user ID]

!(arp or icmp or dns) [masks out arp, icmp, dns, or whatever other protocols may be background noise. Allowing you to focus on the traffic of interest]

udp contains 33:27:58 [sets a filter for the HEX values of 0x33 0x27 0x58 at any offset]

tcp.analysis.retransmission [displays all retransmissions in the trace. Helps when tracking down slow application performance and packet loss]

So there are a few of my favorite Wireshark filters (which does not include the Follow TCP Stream filter). How about you? What are your most commonly used filters?

