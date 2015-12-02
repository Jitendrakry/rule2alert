Rule2Alert parses snort rules and generates packets on the fly that would alert the IDS.  It can either write the packets to a pcap or send the packets directly to the IDS.

Rule2Alert utilizes <a href='http://www.secdev.org/projects/scapy/'>Scapy</a> to craft each individual packet.  Based on the snort rule, R2A can also craft the TCP 3-Way Handshake to imitate a full TCP connection between the server and the client.

Rule2Alert is still in alpha status, and is not ready for a full release.  It can only handle simple rules as of now, but we plan to have a better working version soon!

We want to know what you would use this tool for!  Please post your comments to the [R2AUsage](R2AUsage.md) wiki page.  Thanks!

There is now an example page located here on how to use Rule2Alert: [R2AExamples](R2AExamples.md)

Updates to the project will most likely be posted here first:

http://www.malforge.com