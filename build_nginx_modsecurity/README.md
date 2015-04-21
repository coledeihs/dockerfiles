build_nginx_modsecurity
=======================

Builds nginx 1.6.3 with modsecurity 2.9.0 from sources.

The nginx binary can be extracted from the image with:

    NGINX=$(sudo docker run -d cohesivenet/build_nginx_modsecurity) &&\
    wget http://$(sudo docker inspect --format='{{.NetworkSettings.IPAddress}}' $NGINX)/nginx &&\
    sudo docker kill $NGINX

