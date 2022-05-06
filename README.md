http://security.ubuntu.com/ubuntu/pool/main/a/apt/

JDK 11安装
----------
cd /usr/local/
sudo wget https://mirrors.huaweicloud.com/java/jdk/11+28/jdk-11_linux-x64_bin.tar.gz
sudo tar -zxvf jdk-11_linux-x64_bin.tar.gz
java -version
sudo rpm -qa | grep java
sudo rpm -e --nodeps "package_name"

sudo vi /etc/profile
export JAVA_HOME=/usr/local/jdk-11
export CLASSPATH=$:CLASSPATH:$JAVA_HOME/lib/
export PATH=$PATH:$JAVA_HOME/bin

source /etc/profile ; java -version



1、docker安装
参考：https://docs.docker.com/engine/install/ubuntu/
sudo apt-get remove docker docker-engine docker.io containerd runc

sudo apt-get update

sudo apt-get install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
	
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

将hello用户添加到docker组中
usermod -aG docker hello

配置加速源
vi /etc/docker/daemon.json
{
"registry-mirrors": ["https://pee6w651.mirror.aliyuncs.com"]
}




2、jenkins
内存：4g
磁盘：100G
docker pull jenkins/jenkins:lts-jdk11
sudo docker pull jenkins/jenkins:2.332.3-lts-jdk11

sudo docker run --name jenkins -u root -m 4G --restart=always -p 8010:8080 -p 51000:51000 -v /etc/localtime:/etc/localtime:ro -v /opt/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -d jenkins/jenkins:2.332.3-lts-jdk11

在浏览器输入ip:8010
sudo cat /opt/jenkins_home/secrets/initialAdminPassword

master在
使用JAVA WEB方式配置slave，参考：https://www.yuque.com/duduniao/notes/py0tu7

3、gitlab
参考：https://segmentfault.com/a/1190000018991630
      https://blog.csdn.net/qq_43626147/article/details/109160229  # 修改端口
-- 下载镜像
sudo docker pull gitlab/gitlab-ce
vi /etc/profile
export GITLAB_HOME=/opt/gitlab
source !$

下面的端口需要改一下：
sudo docker run --detach \
  -u root \
  -m 4G \
  --hostname gitlab.example.com \
  --publish 2443:443 --publish 8020:80 --publish 51022:22 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab \
  --volume $GITLAB_HOME/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/data:/var/opt/gitlab \
  --shm-size 256m \
  gitlab/gitlab-ce:latest
