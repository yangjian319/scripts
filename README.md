# scripts
1、docker安装
sudo apt-get remove docker docker-engine docker.io containerd runc

sudo apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
	
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin





2、jenkins
docker run --name jenkins -u root -m 2G --restart=always -p 8090:8080 -p 51000:51000 -v /etc/localtime:/etc/localtime:ro -v /opt/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -d jenkins/jenkins
