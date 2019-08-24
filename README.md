# Hortonworks Data Platform (HDP) Sandbox on AWS

This will deploy the HDP Sandbox on Amazon Web Services (AWS).
Current instructions are for HDP 2.5 but should require minimal changes for newer versions.

1. Boot an Amazon Linux instance with at least 16GB of RAM

2. Execute the following. It will take a while

![Deploying!](https://imgs.xkcd.com/comics/compiling.png)

```
## install docker
sudo yum update -y
sudo yum install -y docker

## fix docker for importing the large sandbox image
sed -i.backup 's/\(^OPTIONS=.*\)"$/\1 --storage-opt=dm.basesize=20G"/' /etc/sysconfig/docker

## start docker
sudo service docker start

## confirm docker is working
sudo usermod -a -G docker ec2-user
docker info

## download docker image
curl -O http://hortonassets.s3.amazonaws.com/2.5/HDP_2.5_docker.tar.gz

## load docker image
docker load -i HDP_2.5_docker.tar.gz
 
## confirm image is available
docker images

## get sandbox docker startup script
curl -O https://raw.githubusercontent.com/hortonworks/tutorials/hdp-2.5/tutorials/hortonworks/hortonworks-sandbox-hdp2.5-guide/start_sandbox.sh

## start sandbox
bash start_sandbox.sh

## configure to start at boot
echo "bash /root/start_sandbox.sh" >> /etc/rc.local

## Print the URL for accessing the Sandbox
echo -e "##\nAccess the Sandbox at:\nhttp://$(curl -sS4 icanhazip.com):8888\n##"
```

3. Access the Sandbox at the hosts public IP.
