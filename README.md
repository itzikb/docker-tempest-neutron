# docker-tempest-neutron

[![Build Status](https://travis-ci.org/itzikb/docker-tempest.svg?branch=master)](https://travis-ci.org/itzikb/docker-tempest-neutron)  
It's meant to make it easy to configure and run tempest against an existing cloud.

**Note** This image contains tempest tests and the tests from the [neutron tempest plugin](https://github.com/openstack/neutron-tempest-plugin). 


**Note:** If you already have the image use the following to get the latest build
```
$ sudo docker pull itzikb/docker-tempest-neutron
```

Docker should be installed and running.
To install it run(Fedora):
```
$ sudo dnf -y install docker && sudo systemctl start docker && sudo systemctl enable docker
```
or For other RHEL based distros:
```
$ sudo yum -y install docker && sudo systemctl start docker && sudo systemctl enable docker
```

Before running you need to have a directory with overcloudrc file and set the SELinux file context. 

For example:
```
$ mkdir /home/stack/authdir
$ cp /home/stack/overcloudrc /home/stack/authdir
$ sudo chcon -Rt svirt_sandbox_file_t /home/stack/authdir
```

To use it run the following (Here overcloudrc file is under /home/stack/authdir):
```
$ sudo docker run -it --name mytempest --network host -v /home/stack/authdir:/env itzikb/docker-tempest-neutron  /bin/bash 
```

**Note: Don't use the /home/stack directory as it may result in an unexpected behavior**

If you need to run the other_tempest_config.sh script (for now just adding lbaasv2 extenstion) run as follows:

```
$ sudo docker run -it --name mytempest --network host -e OTHER_CONFIG=/home/centos/scripts/other_tempest_config.sh -v /home/stack:/env itzikb/docker-tempest-neutron  /bin/bash
```

Inside the container run
```
$ cd ~/tempest-upstream/tempest
$ sudo ~/scripts/run_tempest.sh
$ source ../bin/activate
$ stestr list
```
**itzikb/docker-neutron-tempest** is the docker image to use (don't change)  


You can access the container by running
```
$ sudo docker exec -it <container-name> /bin/bash
```
You can see all the containers by running
```
$ sudo docker ps -a 
```

## Troubleshooting
After Redployment tests are complaining about connections timeout
Make sure you either run a new container or copy over the overcloudrc to the directory you use (e.g. authdir) and run the ~/scripts/run_tempest.sh script
