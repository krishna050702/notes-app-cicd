# Simple Notes App
This is a simple notes app built with React and Django and deployed on AWS using Jenkins to demonstarte CI-CD piepline concepts

## Requirements
1. Python 3.9
2. Node.js
3. React

## Installation
1. Clone the repository
```
git clone https://github.com/krishna050702/notes-app-cicd.git
```

2. Build the app
```
docker build -t notes-app .
```

3. Run the app
```
docker run -d -p 8000:8000 notes-app:latest
```

## Nginx

Install Nginx reverse proxy to make this application available

`sudo apt-get update`
`sudo apt install nginx`

<hr>
<h3>Here is short overview and all the commands what I have did in the video.</h3>
<hr>
Linkedin Post:- 
<hr>

<h4>Step 1</h4>
<p>Fork my GitHub repo. </p>
<h4> Step 2</h4>
<ul>
<li> Go to your AWS Dashboard. <ul>
    <li> Go to EC2.
    <li> Select Ubuntu as your IOS image.
    <li> use free tier version.
    <li> create a key pair login, use .pem key in it.
    <li> Launch the instance.</ul>
    </li>
</ul>

<h4>Step 3</h4>
<ul>
<li> Connect the instance. <ul>
<li> I am going to use windows terminal. Open the terminal and go to the location where you have saved your .pem key.
<li> use the following commands to give access to the key

```
icacls.exe key.pem /reset
icacls.exe key.pem /grant:r "$($env:username):(r)"
icacls.exe key.pem /inheritance:r
```

<li> No click on the instance and click connect select SSH client and copy the cmd .
</ul>

<h4> Step 4</h4>
<ul>
<li> Clone the Github Repo</li>

``` git clone https://github.com/krishna050702/notes-app-cicd.git```

```cd notes-app-cicd ```

```sudo apt-get update```

```sudo apt install docker.io```

```docker --version```

<li> We neet to give permission to ubuntu user t o access docker</li>

```sudo usermod -aG docker $USER```

```sudo reboot```

<li> Login to ssh client again through terminal</li>

```ssh -i "key.pem" ubuntu@ec2-3-86-99.compute-1.amazonaws```  only example cmd 

```cd notes-app-cicd```

```cat Docketfile```

```docker build -t notes-app-cicd .```

</ul>

<h4>Step 5 Install Jenkins</h4>
<ul>

```sudo apt update```
```sudo apt install openjdk-17-jre```
```java --veriosn```
<li> Java is installed it's time for jenkins

```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

<li>Check is jenkins active or not</li>

```service jenkins status```

</ul>

<h4>Step 6</h4>
<ul>
<li> Edit the inbound rules as shown in my video to give access to the jenkins port i.e 8080
<li> Unlock the jenkins by running the cmd on terminal 

```sudo cat var/lib/jenkins``` the rest command you can see at your public address:8080 of EC2 instance

<li> Create admin user
<li> Install suggested plugins.
</ul>

<h4>Step 7</h4>
<ul>
<li> Create a Job on jenkins dashboard
<li> Setup the pipeline as shown in my video
<li> Create evn variable to save your docker hub passwords and id
<li> Create docker hub account
</ul>

<h5>Piepline Script</h5>

```
pipeline {
    agent any 
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/krishna050702/notes-app-cicd.git", branch: "main"
            }
        }
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t notes-app-cicd ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag notes-app-cicd ${env.dockerHubUser}/notes-app-cicd:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app-cicd:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
```

<li> Add one more inbound rule for your docker image at port 8000
<li> On your terminal give jenkins permissions to the docker

```sudo usermod -aG docker jenkins```

<li> Build the pipeline 
<li> Congrats access your app through your instance public ip:8000

<hr>

LinkedIn Profile :- https://www.linkedin.com/in/krishna-mundada/
