 ># **M300**  - LB2 
![Vagrant](https://upload.wikimedia.org/wikipedia/commons/thumb/8/87/Vagrant.png/150px-Vagrant.png) 
![Git](https://duckduckgo.com/i/d11b358b.png)
<br><br>

---


## Sections
- [Description](#description)
- [Introduction](#introduction)
- [Code](#code)
- [Links](#links)

# Introduction

This repository is made for my LB, which contains my small Vagrant project. It will be described in this document.
<br><br>

# Description
 
 The purpose of this Vagrant file is to create an Ubuntu VM that serves as a Web Server. Additionally it has its own html website that will start up as soon as we lookup its also own address predefined in the .conf file that it will read from the M300_repository
<br><br>


 # Code

## Private IP
The private IP I used for this VM is ```192.168.68.8```, this was done by inserting the following line in the Vagrant File code


```
config.vm.network :private_network, ip: "192.168.68.8"
```

## Shell

In order for Vagrant to open a different file inside of the Vagrant VM folder, I inserted the following line:

```
config.vm.provision :shell, :path => ".provision/bootstrap.sh"
```
Inside of this file I have all of the different shell commands that will be used by the Vagrant VM during the installation


```
#!/usr/bin/env bash

# nginx
sudo apt-get -y install nginx
sudo service nginx start

# set up nginx server
sudo cp /vagrant/.provision/nginx/nginx.conf /etc/nginx/sites-available/site.conf
sudo chmod 644 /etc/nginx/sites-available/site.conf
sudo ln -s /etc/nginx/sites-available/site.conf /etc/nginx/sites-enabled/site.conf
sudo service nginx restart

# clean /var/www
sudo rm -Rf /var/www

# symlink /var/www => /vagrant
ln -s /vagrant /var/www
```
Here we are installing Nginx and starting it, ```-y``` allows the machine to autimatically answer to "yes" when the input is required by the user.
Then, the server configuration from the ```nginx.conf``` file will be copied into Nginx's ```sites-available``` folder and then create a symlink from the ```sites-enabled``` to ```sites-available```, after this Nginx will be restarted to use the new configuration settings.<br>
in the last lines a link from ```/var/www``` to ```/vagrant``` will be created. <br>

```/var/www``` is usually where the code goes on a server, the link is optional but I suposed I might aswell add it, what it does is basically link this one server with the Vagrant VM shared folder, which is /vagrant

<br>

## Server config File 

```
server {
  listen 80;

  server_name vagrant-cardoso.local.com;
  access_log /var/log/nginx/access.log;
  error_log /var/log/nginx/error.log;
  root   /var/www;

  location / {
  }
}
```
Under the folder ```.provision,``` I added a folder named ```nginx``` and there I made a new file, named ```nginx.conf``` which contains this inside. This is a basic Nginx server configuration. The only thing I really changed was the server name to make the project look a little more of my own, I think it makes it look much better than just having a normal IP as the website adress.
<br>

## Matching private IP

Now, reaching the final steps I added the IP Adress and the server name to the "hosts" file under ```Windows/System32/Drivers/etc/``` (this has to be done from the Admin account, which, if not on, will have to be activated manually)<br>
```
192.168.68.8    vagrant-cardoso.local.com
```
This is the line that I added to the ```hosts``` file

And now the last step is to create a ```.html``` file. 
Here is what I wrote on mine:
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Vagrant Web</title>
    </head>
    <body>
        <h1>Hi, welcome to my Vagrant website</h1>
        <br>
		</h2>Author: Juan Cardoso</h2>
	<br>
		</h2>Motive: LB2</h2>
	<br>
<br>
<br>
 <img src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fupload.wikimedia.org%2Fwikipedia%2Fcommons%2Fthumb%2F8%2F87%2FVagrant.png%2F150px-Vagrant.png&f=1&nofb=1">
    </body>
</html>
```
I didn't do a big thing for this since I don't think it matters a lot with the main task which was creating the Vagrant File itself not program a ```.html``` site, but I did add a couple of things like an image or some headers with a short text.

Now to check if everything works, we just lookup ```http://vagrant-cardoso.local.com``` in the browser and thats it :)
 <br><br>

 # Links

- [Kapitel 10](https://github.com/mc-b/M300/tree/master/10-Toolumgebung)
- [Kapitel 20](https://github.com/mc-b/M300/tree/master/20-Infrastruktur)

 <br><br>
