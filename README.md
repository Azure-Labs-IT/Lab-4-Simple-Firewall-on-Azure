# Lab-4-Simple-Basic-Firewall-on-Azure
This projects consists of guidelines on how to configure a simple basic firewall on Azure Virtual Machine(s)

<h1>Azure simple firewall configuration</h1>

<h2>Description</h2>
How to configure/setup a simple firewall for an Azure Virtual machine(s) on a subnet. 
<br />


<h2>Utilities Used</h2>

- <b>Azure</b> 
- <b>Pre-installed WireShark</b> <br/>
      (See Azure Lab 3: <a href="https://github.com/Azure-Labs-IT/Lab-3-Azure-Observe-Network-Traffic/blob/main/README.md#lab-3-azure"> WireShark installation and usage</a>)
- <b>Powershell</b> 

<h2>Environments Used </h2>

- <b>Windows 11</b> (25H2)

<h2>Prerequisites </h2>

- <b>An Azure Account/Subscription (paid or free)</b>
- <b>Pre-Created Resource Group</b> <br/>
    (See Lab 1: <a href="https://github.com/Azure-Labs-IT/Lab-1-Azure/blob/main/README.md"> How to create Resource Group</a>)
- <b>Pre-Created Virtual Machines in the same resource group and subnet. One Linux Virtual Machine and One Windows Virtual Machine</b> <br/>
    (See Lab 1: <a href="https://github.com/Azure-Labs-IT/Lab-1-Azure/blob/main/README.md"> How to create a Virtual Machine(s)</a>)

<h2>Labs walk-through:</h2>

<p align="center">
1. On your Windows Virtual Machine open WireShark and Powershell. Filter the traffic on Wireshark to ICMP traffic only then in powershell run the perpetual ping command ( ping __IP_ADDRESS____-t) <br/>
<img src="" height="80%" width="80%" alt="Run perpetual ping"/>
<br />
<br />
</p>

<p align="center">
2. On the Azure portal: <br/> 
Since we're sending traffic to the Linux machine we need to see it's network traffic rules or better known as Firewall. So go to the pre-created Linux Virtual machine and click on it to view it's specifications and machine info like network settings 
    <br/>
<br/>
<img src="" height="80%" width="80%" alt="Click on Linux VM"/>
<br />
<br />
</p>

<p align="center">
3. A new screen will pop up showing the virtual machine info. Under the Networking dropdown go to Network Settings, you should see some of the Network rules that were already set by Azure or you when setting up the Virtual Machine. From the Network Settings screen click on the Network Security Group assigned to the Linux virtual machine:  <br/>
<img src="" height="80%" width="80%" alt="Go to Network Security Group"/>
<br />
<br />
</p>


<p align="center">
4. Now you are at the Firewall of the virtual machine and from here you can see all the set security parameters for incoming and outgoing traffic for this Virtual Machine.<br/>
<img src="" alt="See Firewall"/>
<br />
<br />
</p>

<p align="center">
5.Under Settings click on Inbound Secuirty Rules, from here you can set new or adjust existing parameters for traffic being sent to this Virtual Machine:  <br/>
<img src="" height="80%" width="80%" alt="Click on inbound for adjusting inbound traffic"/>
<br />
<br />
</p>

<p align="center">
6. Click on Add this will pop up a new screen where you can input details on the specifications of your new security rule and add that new secrurity rule to the inbound security rule list you saw before:  <br/>
<img src="" alt="Click on Add"/>
<br />
<br />
</p>

<p align="center">
  7.Each Paramater has it's own purpose and use case:
    <ul>
      <li>
        <b>Source:</b> This states the origin of the network traffic. In this "Any" means the rule applies to traffic from any source IP address or range. Other options could include specific IP addresses, IP ranges, or service tags.  
      </li>
      <li>
        <b>Source Port Ranges:</b> This states the source port(s) basically the exact port number on the source machine where the traffic is coming from and incoming traffic from this port will be affected by the rule/parameter you create. For example if you wanted to block all incoming traffic from the port number 25 (SMTP) of the source, you would add the port number in Source and under action click deny.
      </li>
      <li>
        <b>Service:</b> This is basically the quick shorthand of Source port ranges and is handy if you want to quickly populate the Source port ranges without typing them out. This Allows selection of common services (like HTTP, SSH) which automatically populate the port ranges and protocol fields. "Custom" allows manual specification of these fields.
      </li>
      <li>
        <b>Destination port ranges:</b> This defines the specific ports on the destination machine(Linux machine) the traffic is trying to reach and will be affected by this rule. The * means all destination ports will be affected by this rule. 
      </li>
      <li>
        <b>Protocol:</b> Determines the type of network protocol the rule applies to. Options include:
        <ol>
          <li>Any: Applies to all protocols.</li>
          <li>TCP: Transmission Control Protocol.</li>
          <li>UDP: User Datagram Protocol.</li>
          <li>ICMPv4/ICMPv6: Internet Control Message Protocol for IPv4/IPv6, often used for diagnostics like ping which is what we're using ICMPv4 for.</li> 
        </ol>
      </li>
      <li>
        <b>Action:</b> This decides what happens to traffic that matches the criteria:
        <ol>
          <li>Allow: Permits the matching traffic to pass through the firewall. </li>
          <li>Deny: Blocks or discards the matching traffic.</li> 
        </ol>
      </li>
      <li>
         <b>Priority:</b> A number that determines the order in which rules are evaluated. Lower numbers have higher priority. Rules are processed in numerical order, and the first rule that matches             the traffic is applied. A priority of 290 is relatively high, meaning this rule will be checked before rules with higher numbers (e.g., 300, 400). 
      </li>    
      <li>
        <b>Name:</b> A unique identifier for the rule (e.g., "DenyAnyInbound").
      </li>  
      <li>
       <b>Description:</b> An optional field for adding context or notes about the rule's purpose. 
      </li>
    </ul>
   <p align="center">
      Set the following parameters from their default and Click Add to add a new rule. This rule will just block all incoming traffic and since ICMP does not have a port number you can leave           the Destination port as is:  <br/> 
        <img src="" height="80%" width="80%" alt="Set parameters/Criteria and add Rule"/> <br/>  
    </p>  
</p>

<p align="center">
8. After adding the new rule. Go to Powershell you should see a request timed out error start populating the terminal .This means that now all the ping requests have been blocked by the configured firewall. On Wireshark you should only see request packets as well:<br/>
<img src="" width="80%" alt="Firewall blocking ICMP traffic"/>
<br />
<br />
</p>

<p align="center">
9. To disable the new rule you can just delete the Rule from Azure, but you first have to select the new rule first and then you can delete it but that's if you're using the delete button on the tab:  <br/>
<img src="" width="80%" alt="Disable and delete"/>
<br />
<br />
</p>

<p align="center">
10.  It will take a little while to delete but once it is done you will receive response from the Virtual Machine again:  <br/>
<img src="" width="80%" alt="Delete Done"/>
<br />
<br />
</p>

<p align="center">
11. As you can see now the ping requests are getting a reply and are no longer blocked. Rule removed!:  <br/>
<img src="" width="80%" alt="Rule lifted"/>
<br />
<br />
</p>


<p align="center">
<b> And you're done ,you have now successfully configured a simple basic firewall for a virtual machine. Don't forget to disable/stop your virtual machines on Azure once done(remember you pay as you use): </b>  <br/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
