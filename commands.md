// local
// docker desktop running in background 

git clone https://github.com/atulkamble/github-actions-sonarqube-project.git
cd github-actions-sonarqube-project


sudo docker-compose up -d

http://localhost:9000

admin admin 

change password 

Admin@123456



// on ec2 
amazon linux 2023 | sonar.pem | SSD - 40GB | t3.medium 
SG: SONAR-9000, SSH-22

sudo yum update -y 
sudo yum install git docker tree -y 
sudo systemctl start docker 
sudo systemctl enable docker  
git clone https://github.com/atulkamble/github-actions-sonarqube-project.git
cd github-actions-sonarqube-project
sudo mkdir -p /usr/local/lib/docker/cli-plugins
sudo curl -SL https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64 -o /usr/local/lib/docker/cli-plugins/docker-compose
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose

sudo docker compose up -d

http://instance-ip:9000

admin admin 

change password 

Admin@123456


