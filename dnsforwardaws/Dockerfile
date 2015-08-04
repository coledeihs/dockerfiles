FROM ubuntu:12.04

MAINTAINER @cohesivenet

# This copyrighted material is the property of
# Cohesive Flexible Technologies and is subject to the license
# terms of the product it is contained within, whether in text
# or compiled form.  It is licensed under the terms expressed
# in the accompanying README.md and LICENSE.md files.
#
# This program is AS IS and  WITHOUT ANY WARRANTY; without even
# the implied warranty of MERCHANTABILITY or
# FITNESS FOR A PARTICULAR PURPOSE.

# make sure everything is up to date
RUN apt-get update

# install bind and curl
RUN apt-get install -y bind9 bind9utils curl

# customised config to be a DNS forwarder
ADD named.conf.options.template /etc/bind/named.conf.options.template
# start script to template in VPC DNS IP
ADD startdns.sh /startdns.sh

EXPOSE 53/udp

CMD ["/bin/bash /startdns.sh"]
