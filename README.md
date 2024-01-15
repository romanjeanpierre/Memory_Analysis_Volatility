<h1>Memory Dump Analysis with Volatility</h1>

<h2>Brief Description</h2>
<p>The objective of this project is to conduct memory forensics using the industry standard tool - Volatility. Memory forensics is a crucial aspect of digital forensics, aiding in the analysis of volatile system data to uncover potential security incidents, malware infections, and other suspicious activities.</p>

<h2>Project Walk-Through</h2>
<br/>
To start a memory analysis with Volatility, identifying the type of memory image is a mandatory step for the suggested profile to be used with that image. 
<br/>
<br/>
Open Terminal > Change Directory to where Volatility application resides > Input file path of memory image and retrieve image info
<br/>
<br/>
Command => <code>python vol.py -f /home/ubuntu/Desktop/Volatility\ Exercise/memdump1.mem image info</code>
<br/>
<br/>
<img src="https://imgur.com/C3Qo3mE.png" height="80%" width="80%" alt="Project Overview Image">
<h3> Results:</h3>
<oL>
<li>Suggested Profile: Win7SP1x64</li> 
<li>KDGB address value: 0xf80002bfa0a0L</li>
</oL>
<br/>
<h3>Find the list of processes</h3>
<br/>
<p> Command => <code>python vol.py -f /home/ubuntu/Desktop/Volatility\ Exercise/memdump1.mem --profile=Win7SP1x64 pslist</code></p>
<br/>
<img src="https://imgur.com/T07ndUW.png" height="80%" width="80%" alt="FTK Imager Memory Capture">

<h3>Display processes in Parent and Child Representation</h3>
<br/>
Use the Parent-child relationship to detect unusual processes.
<br/>
<p> Command => <code>python vol.py -f /home/ubuntu/Desktop/Volatility\ Exercise/memdump1.mem --profile=Win7SP1x64 pstree </code></p>
<br/>
<img src="https://imgur.com/td5uSbc.png" height="80%" width="80%">

<img src="https://imgur.com/UZnmFcg.png" height="80%" width="80%">
<br/>
svchost.exe process creates a cmd.exe child process, where the ping command is used, because we can see the ping.exe process being this will be considered unusual activity. 
<br/>
<br/>
<h3> Filtering processes with sum count </h3>
<p>Finding how many processes with the name "svchost.exe" were running in the system</p>
Command => <code> python vol.py -f /home/ubuntu/Desktop/Volatility\ Exercise/memdump1.mem --profile=Win7SP1x64 pslist | grep “svchost.exe” | wc -l </code>
<br/>
<br/>
<img src="https://imgur.com/ZOcBykp.png" height="80%" width="80%" alt="PID Results Output">

<h3> Finding command lines with associated PIDs </h3>
Command => <code> python vol.py -f /home/ubuntu/Desktop/Volatility\ Exercise/memdump1.mem --profile=Win7SP1x64 cmdline -p 2352 </code>
<br/>
<br/>
<img src="https://imgur.com/LsRVX6r.png" height="80%" width="80%" alt="ProcDump Memory Dump">
<h3>Filtering Malicious Network Connections</h3>
A machine suspected to be infected by some type of malware. We need to identify the harmful IP related to the malware.
<br/>
Command => <code>python vol.py -f /home/ubuntu/Desktop/Volatility\ Exercise/memdump2.mem --profile=Win7SP1x64 netscan</code>
<br/>
<br/>
<img src="https://imgur.com/cVl6nkP.png" height="80%" width="80%" alt="FTK Imager Disk Image Creation">
<br/>
WINWORD.EXE (Microsoft Word) was communicating to the Foreign IP 65[.]111[.]166[.]58 on port 80 (HTTP). Based on knowledge of phishing attacks, this is very likely to be a malicious Word document macro that is downloading malicious software from the mentioned IP address over HTTP.
<br/>
<br/>
<h3> Dump specific process and calculate MD5 Hash</h3>
Filter process ID 2940 information and output to a directory path
<br/>
Command => <code>python vol.py -f /home/ubuntu/Desktop/Volatility\ Exercise/memdump2.mem --profile=Win7SP1x64 procdump -p 2940 -D /home/ubuntu/Desktop</code>
<br/>
<img src="https://imgur.com/kXUvhpb.png" height="80%" width="80%" alt="Drive Input Selection">
<br/>
Calculate MD5 Hash
<img src="https://imgur.com/tINj1d6.png" height="80%" width="80%">
<br/>
<br/>

<h2>Conclusion</h2>
<p>In conclusion, the use of industry-standard tools such as FTK Imager, Sysinternals, and KAPE facilitates efficient and effective digital data acquisition for forensic purposes. The adherence to critical data integrity methodologies, including the Order of Volatility and Chain of Custody, ensures the reliability and admissibility of the acquired evidence in legal proceedings.</p>

<p>This project walkthrough provides a comprehensive guide to collecting memory dumps and disk images, crucial steps in investigating and responding to incidents involving potentially compromised workstations.</p>

