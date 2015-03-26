Windows utils
=============

The latest release for this is at https://github.com/cohesivenet/dockerfiles/releases/download/windows-util-20150326/windows-util.tar.gz

To deploy this container:

First log into VNS3 management console.

Uploading the image
-------------------

Click on Container > Images then press Upload Image

Name the image, provide a description and insert the Image file url:
https://github.com/cohesivenet/dockerfiles/releases/download/windows-util-20150326/windows-util.tar.gz

![alt text](https://cohesive.net/dnld/Docker_Windows_Util/upload_image.PNG "Upload image")

Press Upload

Deploying the image
-------------------

Once the status of the image is 'Ready' press Action > Allocate.

Name the container and insert the Command:
/usr/sbin/nginx

![alt text](https://cohesive.net/dnld/Docker_Windows_Util/allocate_image.PNG "Allocate image")

Press Allocate

Connect to the container
------------------------

Click on Container > Containers and check the IP of the deployed container. If no other containers are deployed then the default is 192.51.100.2

![alt text](https://cohesive.net/dnld/Docker_Windows_Util/container_IP.PNG "Container IP")

Click on Connections > Firewall and create a NAT rule to the container in the Edit rules box:

MACRO_CUST -o eth0 -s 198.51.100.0/28 -j MASQUERADE
PREROUTING_CUST -i eth0 -p tcp -s 0.0.0.0/0 --dport 8888 -j DNAT --to 198.51.100.2:8888

![alt text](https://cohesive.net/dnld/Docker_Windows_Util/firewall_rules.PNG "Firewall rules")

Press 'Save and activate' to create the connection.

Use the container
-----------------

The web server running on the container can now be reached using the manager IP and port 8888:

![alt text](https://cohesive.net/dnld/Docker_Windows_Util/web_server.PNG "Web server")

