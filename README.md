[5/28/2024 11:27 PM] : EXP 6 : Ubuntu

sudo su
curl -fsSL https://get.docker.com -o get-docker.sh
ls
sh get-docker.sh
docker pull httpd
docker run -d -p 80:80 httpd  // assigns port for running the page & generates a number
docker exec -it [number] bash

*************************************************************************************************************************

[5/28/2024 11:27 PM] : EXP 7 : Amazon

sudo su
yum update -y
yum install docker -y
sudo service docker start
sudo usermod -a -G docker ec2-user
docker version
vi Dockerfile  // paste the below after pressing 'i'

FROM httpd:2.4
EXPOSE 80
CMD ["httpd", "-D", "FOREGROUND"]

// after typing press Esc -> :wq! -> ENTER

docker build -t latest .
docker images

// go create access key by : Click Profile -> Security credentials -> scroll -> Create Access Key -> CLI -> last check box -> Next -> Create

aws configure
Access key : …………..
secret key: ………..
Region name: ...........
Output format: text

// Create repository by : Search ECR -> Elastic Container Repository -> Create Repository -> public -> name -> scroll -> create
// After creating : Click repository name -> View Push Commands
now paste all commands one by one in order

*************************************************************************************************************************

[5/28/2024 11:27 PM] : EXP 9 : Amazon

sudo su
yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user
docker version
docker pull httpd
docker run -d -p 80:80 httpd // assigns port for running the page & generates a number
docker exec -it [number] bash
