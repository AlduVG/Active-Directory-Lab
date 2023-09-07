# Active Directory Lab

Welcome! In this project, I'll guide you through the process of setting up a Windows Active Directory lab using Virtual Box.

**Prerequisites:**

Windows Server ISO (2016 version was used in this project).

Windows ISO (Version 10 was used for this project).

Any virtualization tool (Oracle VM Virtual Box was used in this project).

## Lab diagram.

![Diagram](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/bbac80a5-e27d-43b5-aec2-ee8504a8fa43)

## Setting up Virtual Box and Windows install.

This Active Directory project on Windows will be using VirtualBox. For this lab, we will create two virtual machines in VirtualBox. The specific hardware configuration may vary depending on your needs, but in this example, we will use **2 GB of RAM** for both virtual machines, one for the Domain Controller, and the other for the Windows Host. We will also allocate **60 GB of disk space** for each of them.

The Domain Controller will use 2 NIC cards. One will be configured as NAT in VirtualBox and will be responsible for routing internal network traffic to the internet. It will use the DHCP from our home router. The other NIC of the DC will be used for the internal network.

The Windows host will only have one NIC, and it will also be configured in the VirtualBox internal network.

## Lab setup.

### Windows Server setup

![1 1 Instalando Windows](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/288c5cf5-3ebf-4838-a445-cc27e2d71523)

![1 2 Windows10](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/3cfd3965-713a-4dfe-b3e8-bf6dbe32b337)

Once the virtual machine is installed, we can proceed to change the name of the local server. Select "Computer name," click "Change," and assign a name to the server.

![1 3 Win panel](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/627405a0-a7c2-4777-ae14-37d2a03aedc9)

![1 4 ServerNameChange](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/c261fa3b-4d19-406f-956b-5a6f0bd9fb75)

![1 5DCname](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/e0fadbcd-a5c5-437c-b7d7-7952a1380cc5)

Before restarting, it's a good idea to change the server's time zone. 

![1 6TimeZone](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/9811ca22-d9a2-4415-be84-87152a44e8bd)

![1 7TimeZoneChange](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/d5160f35-1c23-4842-90bf-ad83eb51acbb)

Restart the server to apply the name change.

![1 15DCNameChange](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/905b2913-4d32-43a4-8a51-4c45e8a3f5e9)

#### Promoting to Domain Controller.

Once the server has been restarted, you can verify that the name has indeed changed. Select "Manage" and then "Roles and Features." Click "Next," and then select the "Role-based installation" option. Choose your server and click "Next." The only role we will add is "Active Directory Domain Controller."

![1 17-DCUpdate](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/775568e3-d363-44a4-993c-2dc7eac83d1c)

![1 18-DCUpdate2](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/ea4b5334-ae1c-4e37-a408-3e8ed8feeb21)

![1 19-AD DS](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/fffba5ca-00ac-4b18-aa43-14e42d8f0a90)

Once installed, we will promote our server to a Domain Controller as shown in the image. Select the option to "Add a new forest," and the domain name will be the one of your choice. In this case, I will use ".local" for simplicity and add a password for recovery purposes if needed.

![1 20 Promoting to domain controller](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/856ad434-600e-415d-b6b9-17cc8df0bb53)

![1 21 domain name](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/d251c248-6197-48a7-a7d7-94578916f6ab)

From here, click "Next" until you reach the "Install" option. This process may take up to approximately 10 minutes. Return to "Manage," select "Server Manager Properties," and then check the box for "Do not start Server Manager automatically at logon." This concludes the setup of our Windows Server.

#### Creating an admin account

Instead of using the default administrator account, we will create our dedicated administrator account. Go to Start > Windows Administrative Tools > Active Directory Users and Computers.

![1 24 Admin creation](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/04dd9000-3b60-4e60-8c3a-052474d80268)

Once inside, select our domain, right-click > New > Organizational Unit. After creating the OU, we can proceed to create a user. 

![1 24 1 Admin creation](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/f0acf14f-9fb9-482f-aeca-f183ab16acd4)

After creating the user, we will now assign the administrator role. Right-click on the created user > Properties > Member Of > Add > Domain Admins.

![1 24 2 passwordsetp](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/c8c0d8da-3080-4686-a1f2-37c75913f352)

![1 24 3 passwordsetp](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/0c86cbac-0363-43fe-9917-1657c6554977)

If we log out and log in with our created user, we will see that we are indeed administrators now.

Next, let's add "Roles and Features" again. This time, we will simply add "Remote Access" > "Next" > Select "Routing" > "Add Features" > "Next" > "Install."

#### IP addressing and NICs

Based on the diagram, we have two NICs. 

![1 8 IP address](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/44c4b590-9fef-42f9-8101-28eb4224d173)

![1 9IP change](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/a88a48a3-ce76-455b-b8a5-6a0a33d4dccf)

![1 13 IPv4Settings](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/eaa20ac7-8c9d-48c6-9cfc-12edc8d31ecd)

The first NIC will be used to route all domain traffic to the internet. We don't need to configure anything for this NIC because it automatically obtains an IP from our home router through DHCP. The other NIC will be used for the VirtualBox internal network and must be manually configured. To identify which NIC is which, you can simply check the details of the IPs assigned to these NICs.

![1 13 a-IPv4-DHCP-NIC](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/7fa28525-1093-4a17-a24d-f4b35919ab5b)

The one with a Class A IP can be assumed to be the NATed NIC selected in VirtualBox. The other one has an IP from the APIPA range (169.254.0.0/16). 

![1 13 b-IPv4-APIPA-NIC](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/61cdded5-e933-4f04-a168-472b83e868eb)

Deselect IPv6 since it's not needed. We won't assign a default gateway as the domain controller will serve as the gateway. Also, changing the interfaces name is a good idea. When installing Active Directory, the DNS service is installed by default. Therefore, the server will use itself as DNS, utilizing the loopback address.

![1 13 b-IPv4-APIPA-NIC](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/bcf60ec8-614e-4b98-940f-fc301e388103)

I'll be renaming the NICs so we can identify them from each other.

![1 13 c-renamingNICS](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/787eef5c-a453-429c-ad12-84c846f89de7)

#### DHCP setup
We will follow the previous steps to install DHCP. Navigate to "Roles and Features" and select "DHCP Server." Proceed to install it. Now, go to "Tools" and select DHCP.

![1 31 DHCP](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/568dde52-04bc-4a5d-bf72-7486be508155)

Right-click on IPv4 and choose "New Scope." Click "Next."

![1 31 1 DHCP](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/fd288a6b-0c3a-40e3-8ca5-23f56ba5a4e0)

For the name, I will use the range of IPs I'll be using. In this case, it will be 172.16.0.100-200.

![1 31 2 DHCP](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/71a53b01-ee49-4459-9bb6-aa6a89548a61)
![1 31 3 DHCP](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/2854ce9c-7d61-421d-adbc-53fe2daff4ca)


We don't need to add an exclusion, so click "Next."

![1 31 4 DHCP](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/08bd6df8-972e-4048-9801-dc0ccd8021c7)

Select the time of lease. This is at user discretion.

![1 31 5 DHCP](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/a9ec58ea-00aa-4d4a-99c0-ede3be7faabf)

Select the option "Configure this scope now" and add the IP of the DC.

![1 31 6 DHCP](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/dc456522-8341-4186-b6c6-937091c17e02)
![1 31 7 DHCP](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/92c22a65-12f3-419d-8e4b-ef8adf607366)
![1 31 8 DHCP](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/3d5b7300-5071-4442-91ff-6a684b9d0557)

We won't be using WINS, so click "Next." Activate the scope and click "Next" to finish. Select our DC in the DHCP menu, right-click, and choose "Authorize." Then right-click again and select "Refresh." You will now see green arrows indicating that our DHCP scope is active.

![1 31 9 DHCP](https://github.com/AlduVG/Active-Directory-Lab/assets/131760637/d54de29d-3586-45ec-a8a6-796e6bd725f5)

