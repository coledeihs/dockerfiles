Windows utils
=============

The latest release for this is at https://github.com/cohesivenet/dockerfiles/releases/download/windows-util-ssh-20150327/windows-util-ssh.tar.gz

This is mostly the same as the windows-util container, so take a look at the installation instructions for that https://github.com/cohesivenet/dockerfiles/tree/master/windows-util

Using SSH
---------

Add a firewall rule such as:

    PREROUTING_CUST -i eth0 -p tcp -s 0.0.0.0/0 --dport 2222 -j DNAT --to 198.51.100.2:22

Then SSH to port 2222 on the VNS3 IP and log in as user 'vns3' with password 'vns3'. Be carefull to only open up the cloud firewall to your own IP.

Tunneling
---------

You can tunnel through the SSH session to connect to VMs running in private subnets on the other side of the VNS3
