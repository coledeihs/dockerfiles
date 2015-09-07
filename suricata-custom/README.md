Suricata-custom
=============

Dockerfile and associated config to install Suricata (release 2.1.0) and customised rules. Includes a single demo rule to detect Mastercard numbers.

SSH Key file
------------

authorized_keys should also be replaced to use your own keys

To generate a keypair:

    ssh-keygen -t rsa
    
Then copy the public key to authorized_keys

    cp id_rsa.pub authorized_keys

Customised Rules
----------------

The custom.rules file comes with a single demo rule to detect Mastercard numbers. This can be replaced by additional rules (e.g. a subset of the [Emerging Threats.net Open rulesets](http://rules.emergingthreats.net/))
