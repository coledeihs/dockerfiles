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

# Make sure everything is up to date
RUN apt-get update

# Install dependencies
RUN apt-get install -y libpcre3-dev build-essential apache2-dev libxml2-dev \
    wget libxslt1-dev libgd2-xpm-dev libgeoip-dev

WORKDIR /root/

# Download and unpack modsecurity and nginx
RUN wget http://www.modsecurity.org/tarball/2.9.0/modsecurity-2.9.0.tar.gz &&\
    tar -xvf modsecurity-2.9.0.tar.gz &&\
    wget http://nginx.org/download/nginx-1.8.0.tar.gz &&\
    tar -xvf nginx-1.8.0.tar.gz

# Build modsecurity
RUN cd modsecurity-2.9.0 && \
    ./configure --enable-standalone-module --disable-mlogc && \
    make


# Build nginx with Ubuntu like config
RUN cd nginx-1.8.0 &&\
    ./configure --prefix=/usr/share/nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --http-log-path=/var/log/nginx/access.log \
    --error-log-path=/var/log/nginx/error.log \
    --lock-path=/var/lock/nginx.lock \
    --pid-path=/run/nginx.pid \
    --http-client-body-temp-path=/var/lib/nginx/body \
    --http-fastcgi-temp-path=/var/lib/nginx/fastcgi \
    --http-proxy-temp-path=/var/lib/nginx/proxy \
    --http-scgi-temp-path=/var/lib/nginx/scgi \
    --http-uwsgi-temp-path=/var/lib/nginx/uwsgi \
    --with-debug --with-pcre-jit --with-http_ssl_module \
    --with-http_stub_status_module --with-http_realip_module \
    --with-http_auth_request_module --with-http_addition_module \
    --with-http_dav_module --with-http_geoip_module \
    --with-http_gzip_static_module --with-http_image_filter_module \
    --with-http_spdy_module --with-http_sub_module \
    --with-http_xslt_module --with-mail --with-mail_ssl_module \
    --add-module=/root/modsecurity-2.9.0/nginx/modsecurity &&\
    make 

CMD ["/usr/share/nginx/sbin/nginx", "-g", "daemon off;"]

# Example usage: sudo docker run -i cohesivenet/build_nginx_modsecurity cat /root/nginx-1.8.0/objs/nginx > nginx
