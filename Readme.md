<p align="center">
<img src="https://github.com/user-attachments/assets/43c82b45-3072-4afd-9991-88e49046b513"/>
</p>

<h1>Building DNS</h1>
In this lab, we will explore and experiment with the Domain Name System (DNS) to gain a deeper understanding of how it operates and supports network communication. In this exercise, the goal is to enhance our knowledge of DNS functionality and its  role in translating domain names into IP addresses.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- DNS

<h2>Operating Systems Used </h2>

- Windows 10 Pro (22H2)-Client 1
- Windows Server- DC-1(Domain Controller)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Virtual Network inside Azure Resource Group
- Powershell

<h2>Actions and Observations</h2>

<p>
<img src="https://github.com/user-attachments/assets/70f2f67d-7876-4788-9802-a6119c9efd16" height="50%" width="50%"/>
</p>

<p>
Create a Resource Group, as seen in the lab [Creating Virtual Machines within Azure](https://github.com/gustygreen/Azure). Create and add a Virtual Network. Create and add the Domain Controller. Create and add the Client.<br />
<p>

  
<h2>Lab Steps</h2>

<h3>Search for mainframe DNS record</h3>
<p>
<img src="https://github.com/user-attachments/assets/be2615d4-63a6-4126-8077-4635db42f4c7" height="50%" width="50%"/>
</p>
<p>
First, start the VMs and RDP into Client1. Open PowerShell as an administrator and attempt to ping "mainframe." This will fail because "mainframe" is not found in the local cache, the hosts file, or the DNS server.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/c3a8a121-09d4-4696-b5dc-f917ad089e7e" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, run the command ipconfig /displaydns (this shows the local dns cache). You'll notice there is no entry for "mainframe" in the DNS cache.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/e7cc7c21-e53d-4157-87f6-a6ded4fb27a4" height="50%" width="50%"/>
</p>
<p>
After that, run the command nslookup mainframe. Once again, you'll find that no results are returned.
</p>
<br />


<h3>Create a DNS record for mainframe</h3>
<p>
<img src="https://github.com/user-attachments/assets/23963f7c-a68e-4fdf-a1b6-0986a15d7727" height="50%" width="50%"/>
</p>
<p>
Next, we'll create a DNS A-record on DC-1 for "mainframe" and point it to DC-1's private IP address. On the DC-1 VM, search for Administrative Tools and open DNS. 
</p>

<p>
<img src="https://github.com/user-attachments/assets/e35a64c7-67a1-44b4-9191-9fd5cd24afe5" height="50%" width="50%"/> 
</p>
<p>
Navigate to DC-1(1) > Forward Lookup Zones(2) > mydomain.com(3), then right-click inside mydomain.com and select New Host (A or AAAA). 
</p>

<p>
<img src="https://github.com/user-attachments/assets/b49453b3-cf3a-4141-b9e3-ad1f2b8663f3" height="50%" width="50%"/> 
</p>
<p>
In the Name field, enter mainframe, and in the IP Address field, input DC-1's private IP address. Select Add Host. 
</p>

<p>
<img src="https://github.com/user-attachments/assets/c1274cf2-c49a-43ab-8c0f-80d55b36d17a" height="50%" width="50%"/> 
</p>

You'll receive a prompt that the record was created as well as it will be displayed under mydomain.com host list.
<br />

<p>
<img src="https://github.com/user-attachments/assets/51eaa20a-df1c-45b5-b8cd-a5f6d6852387" height="50%" width="50%"/>
</p>
<p>
Now, return to Client1 and ping "mainframe" in PowerShell again. This time, the ping will succeed because we created a DNS A-record on DC-1 pointing to its private IP address.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/03aca951-964c-47ad-8b43-2353cd91b9ff" height="50%" width="50%"/>
  
<img src="https://github.com/user-attachments/assets/0c45cfb5-2921-499b-9869-ab919d26e548" height="50%" width="50%"/>
</p>
<p>
Now, go back to DC-1 and update the DNS A-record for "mainframe" to point to the IP address 8.8.8.8. After making this change, return to the Client1 VM and ping "mainframe" again. You'll notice it still pings the old IP address.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/425d77ee-02c2-4416-bdd9-4a7e10818ef1" height="50%" width="50%"/>
</p>
<p>
Use the command ipconfig /displaydns on Client1 and locate the entry for "mainframe." You'll see that the A-record still points to 10.0.0.4, providing a more detailed view of the cached DNS entry.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/5509478b-d0d9-4164-8d88-c97f3654efea" height="50%" width="50%"/>
</p>
<p>
To ensure that the new IP address from the updated A-record is reflected, use the command ipconfig /flushdns to clear the DNS cache. This will clear the local DNS cache.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/f96d2d16-a6bf-4d18-bcd5-ee1f67b0a712" height="50%" width="50%"/>
</p>
<p>
Now, attempt to ping "mainframe" one more time from Client1. You'll notice that it successfully pings the new IP address 8.8.8.8, reflecting the updated A-record.
</p>
<br />

<h3>Create a CNAME record for forage</h3>
<p>
<img src="https://github.com/user-attachments/assets/b1eae823-a861-4cc3-8f8c-7ea11d3a43d3" height="50%" width="50%"/>

<img src="https://github.com/user-attachments/assets/a5786493-0063-4839-8172-5afbd2dd1b24" height="50%" width="50%"/>
</p>
<p>
Next, go back to the DC-1 VM and create a CNAME record that points the host "forage" to "www.google.com". To do this, navigate to the DNS Manager, right-click mydomain.com under Forward Lookup Zones, and select New Alias (CNAME). In the Alias name field, enter forage, and in the Fully Qualified Domain Name (FQDN) field, enter www.google.com. Select OK. This will now be listed in the DNS server host file.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/2c8a71f3-caf2-4993-90d5-1226b4e68ec1" height="50%" width="50%"/>
</p>
<p>
Go back to the Client1 VM and ping "forage." You should observe that the ping is successfully resolved to www.google.com, as the CNAME record you created points "forage" to that address.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/319ef8b6-6fb9-4232-8644-a090230fab79" height="50%" width="50%"/>
</p>
<p>
Lastly, run the command "nslookup forage" on Client1. The results should show that "forage" resolves to www.google.com, indicating the CNAME record is working as expected.
</p>
<br />

This concludes how to setup and use DNS on a Domain Controller.
