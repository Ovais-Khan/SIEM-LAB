<h1>Azure Sentinel - SIEM Lab</h1>


<h2>Description</h2>

Step into the future of cybersecurity with our Azure-powered home lab, a dynamic testing ground that leverages Microsoft Azure to create a simulated vulnerable machine for rigorous testing. This innovative setup integrates Azure Security Center for real-time threat detection, Azure Log Analytics for comprehensive log collection (with a focus on capturing failed RDP logins), and Azure Monitor for performance insights. By connecting Azure Log Analytics with Power BI, our home lab offers visually compelling dashboards that map failed RDP login attempts onto a world map, providing a unique and insightful global threat perspective. This cutting-edge environment not only enables realistic testing scenarios but also enhances your ability to identify and respond to potential threats, offering a holistic approach to cybersecurity training and defense in the comfort of your home.<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 


<h2>Environments Used </h2>

- <b>Azure Virtulization</b>
- <b>ipgeolocation.io</b>

<h2>Program Walk-Through:</h2>

<p align="center">
Create A free Azure Subscription with $200 worth of Free Credits: <br/>
<img src="https://i.imgur.com/T8QLQWp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Create a "Honey Pot" Virtual Machine. Make sure to configure the NIC Security Group to allow inbound traffic, and once booted disable Windows Firewall to allow our machine to be as discoverable as possible:  <br/>
<img src="https://i.imgur.com/OGvzFyq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next, we will create a Log Analytics Workspace. What this will do is allow us to ingest logs from the virtual machine via Windows Event Logs. We will also create our own custom logs that will map the geo data of the failed RDP login attempts: <br/>
<img src="https://i.imgur.com/JUfix8C.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next, we will enable the ability to gather the logs from the Virtual Machine into the Log Analytics Workspace. To do so navigate to Microsoft Defender for Cloud > Environment Settings > choose your workspace and enable the following settings as shown. Also, click on the Data collection tab on the left and enable All Events:  <br/>
<img src="https://i.imgur.com/OCjXbcj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next, We will connect our Virtual Machine to the workspace. Navigate to the following tab and click connect:  <br/>
<img src="https://i.imgur.com/IcBxCs2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Add Microsoft Sentinel to your log analytics workspace, navigate to the shown tabs to do so:  <br/>
<img src="https://i.imgur.com/PQlMQHm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
As a recap of what we've done so far, this is the general idea of what our setup looks like. This diagram will help you visualize what we've configured.:  <br/>
<img src="https://i.imgur.com/sE42adj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
To test if your HoneyPot VM is discoverable, ping the IP address from your own device. If Windows Firewall is configured to be off you should get successful ICMP echos:  <br/>
<img src="https://i.imgur.com/AnvgHm5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next, You will use the PowerShell script that is provided copy it into a new script in PowerShell ISE, and save the file to your desktop. IMPORTANT * You will have to replace the API Key with your own Key. Simply go to ipgeolocation.io sign up for an account and use your own Key. I've highlighted the area where you will submit your own Key:  <br/>
<img src="https://i.imgur.com/BJHZZOg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next, hit run to see the magic. As soon as I hit run in a couple of minutes someone from Moscow was trying to brute force my machine. They tried the using the username:administrator. To see where these failed login attempts are being cached, navigate to your C:\ProgramData folder and there will be a file created there called failed_rdp. The first 11 entries is sample data that we will use to train the Log Analytics Algorithm, more explanations coming.  <br/>
<img src="https://i.imgur.com/MNU7el3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next, We will go back to our Azure portal and navigate to our Log Analytics Workspace > Tables > Create> New Custom Log ( MMA-Based) We will then take the sample data form the VM and copy it over and save it in a text editor on our own machine and save it as a file. Next, you'll want to upload that file as a sample log:  <br/>
<img src="https://i.imgur.com/ne9SBV9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
If you hit next you will be prompted to choose a path, enter this path that is located on your virtual machine where all the failed RDP attempts are stored:  <br/>
<img src="https://i.imgur.com/Aj46aR5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Now you need to train the algo to properly parse out the fields designated to longitude and latitude columns.:  <br/>
<img src="https://i.imgur.com/sVDSb6W.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next, Let's add a new Workbook on Sentinel that will allow us to see our data in a more appealing interactive method:  <br/>
<img src="https://i.imgur.com/IcBxCs2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
END RESULT !!! Let your vulnerable VM run for some time, as more and more people try to brute force it after some time your map should start to populate. You can now add touches to your map and customize it however you would want to:  <br/>
<img src="https://i.imgur.com/ICj1OFb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br /><!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
