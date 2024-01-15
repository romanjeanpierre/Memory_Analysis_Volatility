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
<br/>
<br/>
<p>In an incident response engagement, we can use these utilities to capture the image of Malware running on a system.
<br/>
<br/>
<h3>Collecting Disk Image using FTK Imager</h3>
<br/>
<p>Open FTK Imager: Go to File > Create Disk Image > Select Source from Physical Drive</p>
<br/>
<img src="https://imgur.com/liZWHGq.png" height="80%" width="80%" alt="FTK Imager Disk Image Creation">
<br/>
<br/>
<p>Select a Drive input</p>
<br/>
<br/>
<img src="https://imgur.com/Zxb4QuC.png" height="80%" width="80%" alt="Drive Input Selection">
<br/>
<br/>
<p>Create Image - Select the output destination for the file. Click Add and change the format type to a .E01 file. This filetype is used by analysis tools, such as the enterprise-grade forensics triage software EnCase.</p>
<br/>
<img src="https://imgur.com/MVLtw1f.png" height="80%" width="80%" alt="FTK Imager Disk Image Output">
<br/>
<br/>
<p>Select Output location and filename:</p>
<br/>
<img src="https://imgur.com/7o9Ijn0.png" height="80%" width="80%" alt="Output Location and Filename">
<br/>
<img src="https://imgur.com/eywXn2y.png" height="80%" width="80%" alt="Output Location and Filename 2">
<br/>
<p>Once the disk image is complete, FTK Imager will provide us with hash values for integrity purposes.</p>
<!-- Add additional content as needed -->

</body>
</html>
<p>So we can ensure that the disk image or any copies are the exact same as when it was acquired. This allows us to prove or disprove claims of data corruption or tampering.</p>
<br/>
<img src="https://imgur.com/yXysp7b.png" height="80%" width="80%" alt="FTK Imager Disk Image Hash Values">
<br/>
<br/>
<h3>Live Acquisition - Remote KAPE</h3>
<br/>
<p>Copy and Paste the KAPE Application from Host to Guest workstation via a RDP connection</p>
<br/>
<img src="https://imgur.com/8ImXICP.png" height="80%" width="80%" alt="FTK Imager Disk Image Creation">
<br/>
<br/>
<p>Open KAPE on Guest workstation, Config target options, source, destination and items then > Execute </p>
<br/>
<br/>
<img src="https://imgur.com/3DPZY6Q.png" height="80%" width="80%" alt="Drive Input Selection">
<br/>
<br/>
<p>Exfiltrate Kape output from guest workstation to host workstation and analyze results.</p>
<br/>
<img src="https://imgur.com/Irq3N53.png" height="80%" width="80%" alt="FTK Imager Disk Image Output">
<br/>
<br/>
<!-- Add additional sections or content as needed -->

<h2>Conclusion</h2>
<p>In conclusion, the use of industry-standard tools such as FTK Imager, Sysinternals, and KAPE facilitates efficient and effective digital data acquisition for forensic purposes. The adherence to critical data integrity methodologies, including the Order of Volatility and Chain of Custody, ensures the reliability and admissibility of the acquired evidence in legal proceedings.</p>

<p>This project walkthrough provides a comprehensive guide to collecting memory dumps and disk images, crucial steps in investigating and responding to incidents involving potentially compromised workstations.</p>

