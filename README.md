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

<h3>Step 1</h3>
<p>Fork my GitHub repo.</p>
