FROM ubuntu:12.04

# original MAINTAINER lendup
MAINTAINER cohesivenet

# update local package directory
# install ssh server and python tools
# install ruby 1.9.1
# set ruby 1.9.1 to be the default
# install synapse
RUN apt-get update && \
    apt-get install -y openssh-server supervisor build-essential \
      python-software-properties ruby1.9.1 ruby1.9.1-dev \
      rubygems1.9.1 irb1.9.1 rdoc1.9.1 \
      libopenssl-ruby1.9.1 libssl-dev zlib1g-dev &&\
    update-alternatives --install /usr/bin/ruby ruby /usr/bin/ruby1.9.1 400 \
      --slave   /usr/share/man/man1/ruby.1.gz ruby.1.gz \
                /usr/share/man/man1/ruby1.9.1.1.gz \
      --slave   /usr/bin/ri ri /usr/bin/ri1.9.1 \
      --slave   /usr/bin/irb irb /usr/bin/irb1.9.1 \
      --slave   /usr/bin/rdoc rdoc /usr/bin/rdoc1.9.1 &&\
    gem install synapse --no-rdoc --no-ri

# add PPA for haproxy 1.5 and install newest version
RUN add-apt-repository ppa:vbernat/haproxy-1.5 &&\
    apt-get update &&\
    apt-get install -y haproxy

# set up SSH access for root using provided authorized key
# set up supervisord
RUN mkdir -p /var/run/sshd &&\
    mkdir -p /root/.ssh &&\
    mkdir -p /var/log/supervisor

# bring in configuration files
ADD authorized_keys /root/.ssh/
ADD supervisord.conf /etc/supervisor/conf.d/
ADD synapse.json.conf /etc/synapse.json.conf

# set correct permissions for authorized_keys file
RUN chmod 700 /root/.ssh &&\
    chmod 600 /root/.ssh/authorized_keys &&\
    chown -R root /root/.ssh

# expose 22 (SSH) and 8080 (our haproxy listener)
EXPOSE 22 8080

# default command is to run supervisord (which will run everything else)
CMD /usr/bin/supervisord
