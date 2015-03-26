FROM ubuntu:12.04

MAINTAINER @cohesivenet

# This copyrighted material is the property of
# Cohesive Networks and is subject to the license
# terms of the product it is contained within, whether in text
# or compiled form.  It is licensed under the terms expressed
# in the accompanying README.md and LICENSE.md files.
#
# This program is AS IS and  WITHOUT ANY WARRANTY; without even
# the implied warranty of MERCHANTABILITY or
# FITNESS FOR A PARTICULAR PURPOSE.

# add universe repository to /etc/apt/sources.list
# we need it for nginx
RUN sed -i s/main/'main universe'/ /etc/apt/sources.list

# make sure everything is up to date
RUN apt-get update
RUN apt-get install -y nginx wget

# Remove the default Nginx configuration file
RUN rm -v /etc/nginx/nginx.conf

# Create empty directory for logs
RUN mkdir -p /etc/nginx/logs

# Create directory for web server
RUN mkdir -p /var/www

# Copy configuration files from the current directory
ADD ./nginx.conf /etc/nginx/

# Add Windows clients and utilities
RUN wget http://build.openvpn.net/downloads/releases/latest/openvpn-install-latest-stable-i686.exe -O /var/www/OpenVPN-Win32.exe
RUN wget http://build.openvpn.net/downloads/releases/latest/openvpn-install-latest-stable-x86_64.exe -O /var/www/OpenVPN-Win64.exe
RUN wget http://build.openvpn.net/downloads/releases/latest/tap-windows-latest-stable.exe -O /var/www/Tap-Windows.exe

# Expose ports
EXPOSE 80

# Set the default command to execute
# when creating a new container
CMD /usr/sbin/nginx

# Example runline:
# sudo docker run -dp 8888:8888 cohesivenet/windows-util
