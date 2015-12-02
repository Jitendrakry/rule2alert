# Rule2Alert Example Usage Page #

This page will describe how one might use Rule2Alert to generate a rule.

# Dependencies #

  * Scapy - http://www.secdev.org/projects/scapy/
  * libpcap
  * libpcre
  * python (Written using 2.6)

# Examples #

Help Menu

```
#python r2a.py -h
Usage: python r2a.py [-vt] -f rule_file -c snort_config -w pcap

Options:
  -h, --help     show this help message and exit
  -f RULE_FILE   Read in snort rule file
  -c SNORT_CONF  Read in snort configuration file
  -w PCAP        Name of pcap file
  -v             Verbose hex output of raw alert
  -t             Test rule against current snort configuration
  -m HOMENET     Set $HOME_NET IP Address
  -e EXTNET      Set $EXTERNAL_NET IP Address
  -s MANUALSID   Manual SID Selection
  -n MANUALNUM   Number of times to alert SID
```

Ok, lets say you have a file called **test.rule** which has the following Snort rule:

```
alert tcp any 1024: -> $HOME_NET $HTTP_PORTS (msg:"ET WEB_SERVER Tilde in URI, potential .asp source disclosure vulnerability"; \
flow:established,to_server; content:"GET "; \
depth:4; nocase; uricontent:".asp~"; nocase; classtype:web-application-attack; \
reference:url,seclists.org/fulldisclosure/2009/Sep/0321.html; \
reference:url,doc.emergingthreats.net/2009952; \
reference:url,www.emergingthreats.net/cgi-bin/cvsweb.cgi/sigs/WEB_SERVER/WEB_SERVER_Tilde_Disclosure; sid:2009952; rev:7;)
```

**You no longer need to specify a snort.conf, as long as you specify the home\_net and ext\_net with the "-m" and "-e" switches.**

```
#python r2a.py -f test.rule -m 192.168.1.1 -e 1.1.1.1 -w test.pcap
Building Rule: 2009952
Loaded 1 rules succesfully!
Writing packets to pcap...
Finished writing packets
```

Now we can test with snort, as long as Snort has a configured snort.conf with that rule located in.  In my setup, I just have snort.conf loaded with emerging-all.rules.

```
#snort -c /etc/snort/snort.conf -q -A console -k none -r test.pcap
06/11-15:38:03.670923  [**] [1:2009952:7] ET WEB_SERVER Tilde in URI, potential .asp source disclosure vulnerability [**] [Classification: Web Application Attack] [Priority: 1] {TCP} 1.1.1.1:1024 -> 192.168.1.1:80
```

And we have alerted!