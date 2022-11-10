# GNS3 IOS POC.

This is a POC to get ansible working with network gear.

## GNS3 Brief overview 
The IOU L3 image is loaded into GNS3 with an interface configured with an IP address for SSH access. I had issues using the telnet session so make sure you set it up properly.

Also connect IOU L3 image to a "Cloud" that's in the network you want it to connect to. I Connected Eth0/0 from IOU1 to the cloud.

![Image](https://i.imgur.com/GHTClTM.png)

## Setting up the router. 
Get SSH setup on the box with an IP address and a local username (cisco with pw of cisco in this case)

```
IOU1#conf t

IOU1#(config)#ip domain-name mydomain.net.local

IOU1#(config)#crypto key generate rsa
The name for the keys will be: IOU1.mydomain.net.local
Choose the size of the key modulus in the range of 360 to 4096 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 2048
% Generating 2048 bit RSA keys, keys will be non-exportable...
[OK] (elapsed time was 4 seconds)


IOU1(config)#interface Ethernet0/0
IOU1(config-if)#ip address 192.168.1.50 255.255.255.0
IOU1(config-if)#exit

IOU1#(config)#username cisco secret cisco

IOU1#(config)#ip ssh version 2

IOU1(config)#line vty 0 4
IOU1(config-line)#transport input ssh
IOU1(config-line)#login local
IOU1(config-line)#end

IOU1#wr
Building configuration...
[OK]
```

## Ansible Dependencies
These were my conditions.
1. Running Python 3.10.8
2. `pip install --user ansible ansible-pylibssh`
3. `ansible-galaxy collection install cisco.ios`

## Running the playbook:
1. Ensure the inventory has the proper information you setup
2. `ansible-playbook configure_acl.yml`

## Ending Notes
I Hope this helped. This was mainly a POC for me to get going. This'll help me do better things. I'm using the modules from [Ansible's Network Module page](https://docs.ansible.com/ansible/2.9/modules/list_of_network_modules.html) as well as their [Network Getting Started Guide](https://docs.ansible.com/ansible/latest/network/getting_started/index.html) to work through. 