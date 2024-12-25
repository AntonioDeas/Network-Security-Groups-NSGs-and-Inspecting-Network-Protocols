<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this walkthrough, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create Sample File Shares with Permissions
- Test Access to File Shares as a Normal User
- Create an “ACCOUNTANTS” Security Group and Assign Permissions
- Test Access to the “Accounting” Folder
- Add User to the “ACCOUNTANTS” Group and Retest Access


<h2>Create Sample File Shares with Permissions</h2>

<p>
  <img width="500" alt="created folders" src="https://github.com/user-attachments/assets/459d8b81-c258-4fa5-95ff-8fb41413ab29" />
  <img width="500" alt="read access folder share" src="https://github.com/user-attachments/assets/e563652b-2345-4ddd-bd96-70d3270c2f79" />
  <img width="500" alt="read access folder share complete" src="https://github.com/user-attachments/assets/31459ebf-af05-458b-bbea-a237ffcdc4a2" />
<img width="500" alt="write access folder share" src="https://github.com/user-attachments/assets/3c55c894-5a68-431f-8caf-4f9cc0b56a00" />
<img width="500" alt="write access folder share completed" src="https://github.com/user-attachments/assets/124e488c-5d1e-4f52-acc8-1c3f64963a0e" />
<img width="500" alt="no access folder share" src="https://github.com/user-attachments/assets/395118ea-6960-4e86-a1d6-9a310a541979" />
<img width="500" alt="no access folder share complete" src="https://github.com/user-attachments/assets/2b982677-bd84-428f-900e-0f9edf2e550c" />


</p>
<p>
On DC-1, logged in as the domain admin (mydomain.com\jane_admin), I navigate to the C:\ drive and create four folders named “read-access,” “write-access,” “no-access,” and “accounting.” I then assign permissions as follows:

<ul><li>“read-access”: Shared with the “Domain Users” group, granting “Read” permissions.</li>
<li>“write-access”: Shared with the “Domain Users” group, granting “Read/Write” permissions.</li>
<li>“no-access”: Shared with the “Domain Admins” group, granting “Read/Write” permissions.</li>
<li>The “accounting” folder is left unconfigured at this stage.</li></ul>
</p>
<br />


<h2>Test Access to File Shares as a Normal User</h2>

<p>
<img width="500" alt="all folders page on client-1" src="https://github.com/user-attachments/assets/6538f1ee-e774-4e60-80fe-c08845b7fcd6" />
<img width="500" alt="read folder access (denied)" src="https://github.com/user-attachments/assets/8eb44c19-7ca3-4b14-a107-fbd59503cbb9" />
<img width="500" alt="write folder access" src="https://github.com/user-attachments/assets/fe00f98e-f530-424d-9dde-470741ea7dfe" />
<img width="500" alt="no access folder access (denied)" src="https://github.com/user-attachments/assets/4d1b364e-6ac4-423c-8093-10a038934536" />


</p>
<p>
I log into Client-1 as a normal user (mydomain<nux.nub>) and attempt to access the shared folders by navigating to \\dc-1 using the run command. I test which folders can be accessed and where files can be created, verifying that permissions are enforced as configured. Normal users can only access “read-access” and “write-access,” with creation rights limited to “write-access,” while “no-access” is restricted.</p>
<br />

<h2>Create an “ACCOUNTANTS” Security Group and Assign Permissions</h2>


<p>
<img width="500" alt="added group folder in mydomain com" src="https://github.com/user-attachments/assets/438f3602-f017-40e7-a87e-4fe166ca0c9b" />
<img width="500" alt="added ACCOUNTING group in groups folder" src="https://github.com/user-attachments/assets/f9b53d6b-8991-4fb1-a373-302ac1f5bb1a" />
<img width="500" alt="accounts folder group share" src="https://github.com/user-attachments/assets/0e75ce62-6e3e-4f01-88af-778f68bc82b4" />
<img width="500" alt="accounts folder group share complete" src="https://github.com/user-attachments/assets/9dcd0302-cfaa-4fc5-a7bc-27c04decc96b" />

  
</p>
<p>
On DC-1, I use Active Directory to create a new security group called “ACCOUNTANTS.” I then return to the “accounting” folder and set it to share with the “ACCOUNTANTS” group, granting “Read/Write” permissions.

</p>
<br />

<h2>Test Access to the “Accounting” Folder</h2>


<p>
<img width="769" alt="accounts folder access (denied)" src="https://github.com/user-attachments/assets/4ef8c08b-2eaa-4c04-8c8c-e5394a23d292" />
</p>
<p>
I log back into Client-1 as <nux.nub> and attempt to access the “accounting” folder. Access is denied as expected, since the user is not yet a member of the “ACCOUNTANTS” group.

</p>
<br />

<h2>Add User to the “ACCOUNTANTS” Group and Retest Access</h2>


<p>
<img width="700" alt="adding user to access accounts folder" src="https://github.com/user-attachments/assets/b268dab8-c215-429f-8310-9c50b6216ca4" />
  <img width="700" alt="logging in user client-1" src="https://github.com/user-attachments/assets/3b1398f2-7e6d-402a-8489-2519387e2be3" />
  <img width="700" alt="account folder acces granted" src="https://github.com/user-attachments/assets/8ccbd4b0-d7ef-46ce-929e-76114ff03a2e" />


</p>
<p>
I log out of Client-1 as <someuser> and return to DC-1. In Active Directory, I add <nux.nub> to the "ACCOUNTANTS" security group. After updating the group membership, I sign back into Client-1 as <someuser> and attempt to access the "accounting" folder again. This time, access is granted, confirming the permissions and group membership are functioning correctly.

This process demonstrates how to configure file shares with specific permissions, test access based on group assignments, and dynamically update user access through Active Directory.</p>
<br />
