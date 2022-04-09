# Create-simple-custom-docker-image

## Description

We are creating an custom docker image with help of existing docker image and we are creating an container using the custom/created docker image. Let's move on to the process.

## Installation

If you need to download docker to the EC2 instance, then follow this:

~~~sh
sudo yum install docker -y
sudo systemctl restart docker.service
sudo systemctl enable docker.service
sudo systemctl status docker.service
sudo usermod -a -G docker ec2-user
~~~

Please exit from the instance and login back to reflect the above changes

## Steps involved

## 1. Getting HTTPD conf file

We need to get an HTTPD docker image, this is done to get the httpd.conf file from the image. We use this conf file for our custom docker image. You can use this link for getting [docker image](https://hub.docker.com/_/httpd)

~~~sh
docker container run --name webserver -d httpd:alpine
~~~
> Need to copy the conf file
~~~sh
docker container cp webserver:/usr/local/apache2/conf/httpd.conf .
~~~
> Now we can stop & remove the container as well as the docker image
~~~sh
docker container stop webserver ; docker container rm webserver
docker image rm httpd:alpine
~~~

Now we have conf file with us, we can make our desired changes in this conf file and save it.
![image](https://user-images.githubusercontent.com/100773863/162553134-84f0b48c-6666-4ba9-89ad-db4b1e699bf0.png)


## 2. Adding sample template to instance

Download an sample HTML template to the EC2 instance, you can use this link for getting [sample HTML](https://www.tooplate.com/) . 
Here we downloaded the template to /home/ec2-user/website/ location.

> To get sample HTML template

~~~sh
wget https://www.tooplate.com/zip-templates/2124_vertex.zip /home/ec2-user/website/
cd /home/ec2-user/website/
unzip 2124_vertex.zip
rm -rf 2124_vertex.zip
~~~

![image](https://user-images.githubusercontent.com/100773863/162551874-8a37fd2e-de7f-4737-8b57-0da2c3d47a3e.png)


So, we have the site template in /home/ec2-user/website/


## 3. Move conf and template to one directory

Now we create a directory for handling our project and we move these 2 files (website and conf) to that directory.

~~~sh
mkdir project/
cp -r website/ httpd.conf devops-project 
cd project/
ls -l project/
total 24
-rw-r--r-- 1 ec2-user ec2-user 20827 Apr  4 10:00 httpd.conf
drwxrwxr-x 6 ec2-user ec2-user   106 Apr  9 02:29 website
~~~

## 4. Create file for building image

Now we will create a file in project directory for building our custom docker image. We create file "Dockerfile" and we add these:

~~~sh
vim Dockerfile
~~~

![image](https://user-images.githubusercontent.com/100773863/162553504-30dbc508-3687-4e42-a0ea-72093c0a1e50.png)

> Let's build the image, please ensure you run this command from the corect woreking directory (Project):

~~~sh
docker image build -t dockerimage:1 .
~~~

> Result

![image](https://user-images.githubusercontent.com/100773863/162553606-604eabbf-adb8-4274-b058-107a25263cb7.png)


## 5. Pushing custom image to Docker

Now, let's add this custom docker iumage to our docker hub, sign up in [docker hub](https://hub.docker.com/) and then follow this:

~~~sh
docker login
~~~
> pass your credentials and you will get login succeeded message/result

**We will rename the custom image (dockerimage:1) to desired one, here I rename it to:**

~~~sh
docker image tag dockerimage:1 devasivaram/dockerimage:custom
docker image tag devasivaram/dockerimage:custom devasivaram/dockerimage:latest
~~~

> Result
![image](https://user-images.githubusercontent.com/100773863/162553993-ec61ef20-4d1b-43c1-899e-fb76fe9363e8.png)





## Conclusion

This is how an docker container is created with sample HTML teamplate added to docker image. Please contact me when you encounter any difficulty error while using this terrform code. Thank you and have a great day!


### ⚙️ Connect with Me
<p align="center">
<a href="https://www.instagram.com/dev_anand__/"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/dev-anand-477898201/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a>
