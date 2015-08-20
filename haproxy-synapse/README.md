## HAproxy-synapse

A proxy server for forwarding into cloud service provider load balancers such as Amazon Web Services (AWS) Elastic Load Balancer (ELB) where the load balancer IP address is ephemeral. The Airbnb [Synapse](https://github.com/airbnb/synapse) transparent service discovery framework is used to keep the HAproxy config in sync with any DNS changes to the load balancer end points. HAproxy is not configured directly, as this is taken care of by Synapse, with config in the `synapse.json.conf`

### Customisation

The port, host and nameserver are likely to require changes to match the load balancer being used:

```
"servers": [
          {
            "port": "80",
            "host": "internal-cpswan-testlb-899384879.us-west-1.elb.amazonaws.com",
          }
        ],
        "nameserver": "10.10.30.2"
```

`port` should simply be the entry port for the load balancer (quite possibly still 80 if using HTTP)

`host` is the DNS name of the load balancer

`nameserver` is the DNS server being used. In an Amazon VPC this should be the '+2' address, so for a VPC with a CIDR such as 10.0.0.0/16 the DNS server will be 10.0.0.2

It might also be desirable to change the port number in the "haproxy" block to remain consistent with the port used for the "servers" block.

### Creating a customised Docker image

Firstly clone this repo and change directory into haproxy-synapse:

```
git clone https://github.com/cohesivenet/dockerfiles.git
cd dockerfiles/haproxy-synapse
```

Once `synapse.json.conf` has been edited to suit the target environment run:

```
sudo docker build -t MyDockerHubName/haproxy-synapse .
SYNAPSE="$(sudo docker run -d MyDockerHubName/haproxy-synapse)"
sudo docker export "$SYNAPSE" > haproxy-synapse.tar
gzip haproxy-synapse.tar
sudo docker kill "$SYNPASE"
```

Now move the `haproxy-synapse.tar.gz` file to a web server that can be reached from the VNS3 instance(s) that you want to run the container.

### Installing the image

Click on the **Images** item in the **Container** section of the VNS3 menu. Then select `Upload Image`.

Give the image a **Name:** e.g. `haproxy-synapse`

Paste the URL of where `haproxy-synapse.tar.gz` was uploaded to into the **Image file url:** box.

Click `Upload`

### Running a container

Once the **Status** of the imported image is **Ready** then click the `Action` button and select `Allocate`.

Give the container a **Name:** e.g. `haproxy-synapse`

The **Command:** is `/usr/bin/supervisord`

Click `Allocate`

Make a note of the IP Address given to the container e.g. `198.51.100.2`

### Routing traffic to the container

Click on the **Firewall** item in the **Connections** section of the VNS3 menu.

Add firewall rules such as:

```
MACRO_CUST -o eth0 -s 198.51.100.0/28 -j MASQUERADE
PREROUTING_CUST -i eth0 -p udp -s 0.0.0.0/0 --dport 80 -j DNAT --to 198.51.100.2:80
```

Where `198.51.100.2` is the IP of the container once allocated. Then click `Save and activate`
