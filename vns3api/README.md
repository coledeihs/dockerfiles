vns3api
=======

A containerised version of our VNS3 API Tool.

Run with:

    sudo docker run -it cohesivenet/vns3api

Most commands take the parameters '-K *api* -S *password* -H *manager_ip*' e.g.

    ./vnscubed.rb -K api -S pa55Word -H 12.34.56.78 desc_status

Will return the status of a manager at 12.34.56.78 in YAML.

    ./vnscubed.rb --help

Will display more options

See the [VNS3 API Documentation](https://cohesive.net/dnld/Cohesive-Networks_VNS3-3.5-API.pdf) for usage examples.
