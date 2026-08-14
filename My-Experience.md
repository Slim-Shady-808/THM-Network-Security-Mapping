### My Experience
When I first started this TryHackMe Networking Module with NetSecTap, I only had a very conceptual and general knowledge of networks, from my two years of experience doing part-time IT work. As such, besides knowing that IP addresses were assigned to each machine within a network, most of the information and exercises from the module were new concepts to me. 
(Screenshot)

Since everything was new to me, I decided to use a learning technique I use at school, which involves taking notes for each module onto a google doc, which I would then use as reference for a Claude context window, in the event I ever wanted to learn more information about a topic or concept. 

The first part of the TryHackMe Network module is Passive Reconnasaince, which I learned was about gathering information about a target without direct interaction, using publicly available sources. I learned the difference between passive and active recon, and was quickly able to answer the module questions, which asked me to distinguish between active and passive recon examples. 
(Screenshot)

Then, I was introduced to the whois protocol, which is a TCP query, running on Port 43, useful for finding domain names and registration details. The module also mentioned curl, querying RDAP domains, which are beginning to phase out Whois. Running the VM attackbox, I was able to easily find the registry information for the TryHackMe domain, using "whois tryhackme.com", answering the questions seen in the screenshot. While curl was an available tool, it wasn't requried to find the information. 
(Screenshot)

The next part of the Passive reconnasiance module covered nslookup and dig (which is the modern prefered option) protocols, revealing IP addresses, mail servers, and txt records. Since the TryHackMe module had examples of what to do, I was able to answer the given question of finding the flag within the TXT records of thmlabs.com with "dig @1.1.1.1 thmlabs.com TXT".
(Screenshot)

Moving past the command line tools the TryHackMe module introduced me to two web browser passive recon tools, DNSDumpster and Shodan.io. Using DNSDumpster was simple, as I just had to search up the tryhackme domain and look through the results to find the answer to the given question. On the other hand, when I was using Shodan.io, i was at first confused because I thought I had to search up a specific domain to find the answer to the given question (Which country has the most publicly accessible apache servers?). However, I quickly found I could just search the topic of interest (apache and ngix) to find the information about the servers of interst, and to answer the question. 
(Screenshots)

The next section of the Network module is focused on Active Reconnaisance, which involves direct interaction with a target system to gain information about that system. 
First, I learned about how web browsers could be used for active recon, as it is one of the least suspicious tools to use. TCP and UDP were both ports that I was somewhat familiar with, as my work experience had taught me basics about browser connections. Something that I was unfamiliar with using were developer tools and the given browser extensions. Fortunately, as I had some previous experience coding with html, I was able to quickly figure out the GUI and find the total number of "questions" within the example website. 
(Screenshot)

Moving onto the command line tools, I was pretty excited to see ping explained in the module, as I had to previously use ping while working with workstations and domains. With the knowledge I already had, I quickly answer the first three questions, which asked how to set the data size of the ICMP echo packet, what the size of the ICMP header was, and if MS Windows Firewall blocks ping by default. Then, for the final question, it was as simple as starting the given lab machine and attackbox and inputing the given ping command to find the answer to the question. 

However, since I was not as familiar with traceroute, telnet, and netcat, I had to take time to learn what they are and how they work. Additionally, using google, I was able to find more summative descriptions and definitions of the three protocols. Yet, once I read about the protocols, and learned the command lines, they were fairly simple to use. Using traceroute was self explanatory, and answering the questions was simple enough, with the new knowledge that it maps each "hop" to the target host from my machine. 
(Screenshots) 

Answering the telnet questions was simple as well. Since I already was given the IP address and the port, I was able to find the name and the version of the running server. The netcat question was also simple to answer, as I was given the -p flag needed for specifying ports, allowing me to find the server version on port 21. I didn't quite understand what the differnce between the two was, so with a quick online search, I found that telnet works on the application layer of the OSI model while netcat works on the transport layer (and allows for UDP).

The Nmap Live Host section was completely novel to me. I had some idea of what the transport layer is but had no clue what ARP scans were, TCP SYN/ACK scans, or reverse DNS lookup. Fortunately, I had some background knowledge of subnetworks and network segments. Learning that ARP sent queries to hosts within a subnet, I found the ARP GUI easy to use and was able to answer the questions with ease.
(Screenshot)

Learning about and experimenting with Nmap ARP scans and TCP/UDP (mascan) scans was enjoyable. Getting through this part of the module was as simple as reading through the lessons, learning about the different flags and options, and using them in the attack box to find the answers to some of the given questions. I didn't quite understand what reverse-dns lookup was, so I googled the term and was able to learn that reverse-dns was simply for finding domain names based on an IP address, instead of finding an IP address based on domain names.
(screenshot)
(screenshot)

The basic and advanced port scans may have been my favorite part of the module (aside from learning about Hydra), as it was interesting to learn about the advantages and disadvantages of each option was, and it was satisfying to experiment with them in the attack box. 
The basic port scan module broke down the idea of open, closed, filtered, and unfiltered ports. It was at this point I began to have to use the internet to answer some of the questions in the module, such as "which service uses UDP port 53 by default?" as I had no clue what the answers were, and they were not listed in the lesson. However, I did learn that open ports are what penntesters look out for when testing systems, because they introduce a potential access point/vulnerability for attackers to manipulate and access. 
This information gave me more context as to why TCP Connect, TCP SYN, and UDP scans were used, as well as their different flags. 
(Screenshot)

The goal is to see if there are any open ports using these scans, which attackers can use to their advantage when finding vulnerabilities in a system. It was interesting to learn the difference between TCP and UDP, as TCP works based on connections and handshakes, while UDP works through connectionless packet sending, whether the reciever is open or not. To find the answers to the given questions for this section of the model, I was able to use the lesson examples as reference for command lines to find the necessary information. 
(Screenshot)

It was interesting to learn how powerful advanced port scans are with the amount of information they can uncover, while minimizing potential detection from a target. I found that the TCP FIN, XMAS, and Null scans basically are common scans with certain pretedermined flags set, that allow pentesters to have different vectors to find open ports. The TCP Maimon scan is not as useful for finding open ports with modern networks, but it still can be useful when testing unconfigured networks. The module also presented TCP ACK and Window scans as useful for learning what ports are blocked by a firewall. Finally, TCP custom scans allow the pentester to set specific flags. 
(Screeshot)
(Screenshot)

The next part of the advanced port scans introduced the idea of spoofing MAC and IP addresses, fragmenting packets with wireshark, and using zombie/idle scans to hide the pentester's identity. With these type of scans, an attacker can reduce the chances of being detected and caught, which would grant them more destructive potential. The questions for most of the advanced port scan section were answerable by reading the lesson and inputting the corresponding command lines to find certain information. 
(Screenshot)
(Screenshot)
(Screenshot)

The post-port scans add to a pentesters toolbox, by allowing them to detect service versions, find OS versions, script nmap commands, and save the output of the commands using normal, grep, and xml. By detecting service versions and OS versions, pentesters can use online vulnerability databases to see what threat vectors may be prominent on their system. Nmap scripts add an incredible amount of tools, from finding backdoors, to launching attacks on a target. Answering the questions for detecting service/os versions was a similar process answering questions to previous modules, as I looked through example commands as reference to write commands I needed for answering the questions. The Nmap scripting and output response sections were more complicated, and it took me longer to figure out the commands I needed to answer the given questions, but using the lesson as reference, I was able to figure it out.
(Screenshot)
(Screenshot)
(Screenshot)
(Screenshot)

The final part of the learning module was about ports and servers. 
