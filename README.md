# poc-maas

## Software Required
* Virtual Box
* Vagrant
* Putty

## Knowdedge Required
Please have a look at the "The node lifecycle"<br/>
https://maas.io/how-it-works

## Provision MaaS Virtual Box Guest OS 
* Go to vagrant/maas
* Run: 0-install-plugin.bat
* Run: 1-up.bat <br/> 
  It will bring up a guest OS and install Ansible.
* SSH to 192.168.10.2 with your Putty.

## Install MaaS
* cd /vagrant/ansible
* Run: install.sh <br />
  It will configure ansible and install MaaS server.
* vi ~/.ssh/id_rsa.pub
* Copy the content of the public key. You will need it later.    
* Run: sudo maas createadmin <br />  
  Input admin account, password, and email address to create the admin account. 

## Initiate MaaS
* Visit: http://192.168.10.2:5240/MAAS/
* Login with your admin account.
* Initiate MaaS <br/> 
  You will be asked to provide your ssh public key. <br/> 
  Paste it. <br/> 
  After initialization, you will see the main page of MaaS.
* Navigate to: Subnets > fabric-1<br/>
  The subnet should be 192.168.10.0/24
* In the block "VLANs on this fabric", enter the "untagged" VLAN
* Take Action > Provide DHCP
* Change the Gateway IP to: 192.168.10.2 which is the IP of this MaaS server.
* Navigate to: Nodes <br/>
  Then you can create some "virtual" bare metal virtual box guest machine. <br/>
  Once they are network booted, they will be discovered here. 

## Create and Power on Managed Nodes Virtual Box Guest Machine
* Use your virtual box
* Create some "virtual" bare metal virtual box guest machines.
* Configure them network boot.
* Configure them with the same HostOnly adapter of MaaS server.
* Power them on.
* These bare metals will boot with a temp OS system, connecting to MaaS server, and register themselves to MaaS.<br/>
  Then, they will power off themselves.

## Commissioning
* Back to the web site of MaaS server. http://192.168.10.2:5240/MAAS/
* Navigate to: Nodes<br/> 
  You should see some "New" bare metals discovered.
* Change their hostname
* Change their power type: "Manual"
* Select these bare metals, then, Take Action > Commission
* Back to your virtual box.
* Power on these bare metals and they will start commissioning. <br/>
  After commissioning, they will power off themselves.
  
## Deploying
* Back to the web site of MaaS server. http://192.168.10.2:5240/MAAS/
* Navigate to: Nodes<br/> 
  You should see the status of these bare metals changed to "Ready". And more hardware information are discovered.
* If you want to assign static IP address to nodes, do it at this stage.
* Select these bare metals, then, Take Action > Deploy
* Back to your virtual box.
* Power on these bare metals and they will start installing OS.