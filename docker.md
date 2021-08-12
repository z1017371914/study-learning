```shell
 sudo yum update

 sudo yum install -y yum-utils device-mapper-persistent-data lvm2
 sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
 sudo yum install docker-ce-17.12.0.ce
 sudo yum install docker-ce
 sudo systemctl start docker
 sudo systemctl enable docker
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

