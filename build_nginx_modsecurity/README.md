build_nginx_modsecurity
=======================

Builds nginx 1.8.0 with modsecurity 2.9.0 from sources.

The nginx binary can be extracted from the image with:

    sudo docker run -i cohesivenet/build_nginx_modsecurity cat /roo t/nginx-1.8.0/objs/nginx > nginx
