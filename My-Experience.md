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

