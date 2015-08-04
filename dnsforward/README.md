dnsforward
==========

Forwarding DNS server based upon bind9 named

`10.10.10.2` in named.conf.options needs to be set to your DNS server address. In an AWS VPC this will be the VPC root +2 e.g. if the VPC is 10.0.0.0/16 then the DNS server will be 10.0.0.2

The allow-query parameter can also be configured to lock down the forwarder to specific networks.
