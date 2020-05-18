FROM vns3local:vns3_base
SHELL ["/bin/bash", "-c"]
MAINTAINER @cohesivenet
# This copyrighted material is the property of
# Cohesive Flexible Technologies and is subject to the license
# terms of the product it is contained within, whether in text
# or compiled form.  It is licensed under the terms expressed
# in the accompanying README.md and LICENSE.md files.
# This program is AS IS and  WITHOUT ANY WARRANTY; without even
# the implied warranty of MERCHANTABILITY or
# FITNESS FOR A PARTICULAR PURPOSE.
# Double apt-get update - for some reason the first one is erroring out.

RUN apt-get -y update
RUN apt-get -y update
RUN apt-get install -y -f nano apt-utils perl wget git build-essential tree vim curl autoconf automake libtool libcurl4-openssl-dev libgeoip-dev libpcre++-dev libxml2-dev libxml2 libyajl-dev pkgconf zlib1g-dev sudo

RUN apt-get -y clean
RUN apt-get -y update
RUN sudo apt install -y libperl-dev git libgd3 libgd-dev libgeoip1 geoip-bin libxslt1.1 libxslt1-dev
RUN sudo apt-get -y update
RUN sudo apt install -y software-properties-common build-essential

RUN sudo add-apt-repository -y ppa:maxmind/ppa
RUN sudo apt-get -y update

#create missing directories , Scott.
RUN mkdir -p /var/lib/nginx
RUN mkdir -p /var/lib/nginx/body
RUN mkdir -p /var/lib/nginx/fastcgi

##Let's Encyrpt SSL configuration script
ADD LetsEncryptSetup.sh /opt/LetsEncryptSetup.sh

##openssl
RUN cd /usr/local/src/ && wget https://www.openssl.org/source/openssl-1.1.1c.tar.gz && tar xzvf openssl-1.1.1c.tar.gz  && rm openssl-1.1.1c.tar.gz


##add modsecurtiy connector
ADD usrlocalmodsecurity.tar.gz /usr/local/
ADD ModSecurity-nginx/ /opt/ModSecurity-nginx/

##add compiled directories for nginx to the correct locations
RUN mkdir -p /usr/share/nginx/sbin
ADD nginx /usr/share/nginx/sbin/
RUN mkdir -p /etc/nginx
ADD conf /etc/nginx/
ADD nginx.conf /etc/nginx/

RUN mkdir -p /run
RUN mkdir -p /var/log/nginx
ADD html/ /usr/share/nginx

##add modsecurity module to correct locations
RUN mkdir -p /var/log/nginx
RUN mkdir -p /usr/lib/nginx/modules
RUN mkdir /usr/share/nginx/modules
ADD ngx_http_modsecurity_module.so /usr/share/nginx/modules/
ADD ngx_http_modsecurity_module.so /usr/lib/nginx/modules

RUN ln -s /usr/share/nginx/sbin/nginx /usr/sbin/

RUN mkdir -p /etc/nginx/ssl
ADD ssl.cert /etc/nginx/ssl/
ADD ssl.key /etc/nginx/ssl/


##add modsec example and OWASP rules
RUN mkdir /etc/nginx/modsec
RUN wget -P /etc/nginx/modsec/ https://raw.githubusercontent.com/SpiderLabs/ModSecurity/v3/master/modsecurity.conf-recommended --no-check-certificate
RUN mv /etc/nginx/modsec/modsecurity.conf-recommended /etc/nginx/modsec/modsecurity.conf
RUN sed -i 's/SecRuleEngine DetectionOnly/SecRuleEngine On/' /etc/nginx/modsec/modsecurity.conf
RUN cd /etc/nginx/modsec/ && git clone https://github.com/SpiderLabs/owasp-modsecurity-crs.git


ADD main.conf /etc/nginx/modsec/main.conf
ADD unicode.mapping /etc/nginx/modsec/


ADD supervisornginx.conf /etc/supervisor/conf.d/supervisornginx.conf
