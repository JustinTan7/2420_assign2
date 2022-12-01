# 2420 Assignment 2 README

## Setting up droplets, load balancer, and firewalls

For the purpose of this assignment you will need to create two droplets and connect a load balancer to both of them, as well as a firewall. The setting up of the droplets, load balancer, and firewall won't be covered here because it is extensively covered in these videos:
    
[Digital Ocean Droplet Setup](https://vimeo.com/758870226/f75da348fc?embedded=true&source=vimeo_logo&owner=17609105)
    
[Load Balancer and Firewall Setup](https://vimeo.com/775412708/4a219b37e7)
    
At this point you should have both your droplets created(with ssh for regular users and root login blocked), and the load balancer and firewall connected to both of them. Your project home page should look something like this:

![Digital Ocean Homepage with Load Balancer Connected](/images/do_homepage.png "DO Homepage")

## Installing Caddy 
For this assignment we will be using Caddy as our web server for both our droplets. The instructions to install can be found here: [Caddy Setup](https://caddyserver.com/docs/install)

We are going to install the stable version with these commands:

```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
sudo chown root:root /usr/bin/caddy
```
Once you have this you are ready to start creating your web app.

## Creating the Web App
On your local machine (for me I'm on windows, so its **WSL**) create a new directory to hold your web app. I'm naming mine **2420-assign-2**, you can make yours more personalized to you.

Inside of this directory create two more directories named **html** and **src**.

Inside of the html directory create an **index.html** page

Inside of your index.html create a simple but complete html document, it should look something like this: 

![index.html](/images/index_html_template.png "index.html")

### Installing Volta
We will be using Volta to install node onto our local machine.

Run these commands to install node using volta:
```
# install Volta
curl https://get.volta.sh | bash

# install Node
volta install node

# start using Node
node

```
### Installing Fastify for your NodeJS server

Inside of your src directory create a new node project.

To initialize it you will need to run these commands:

`npm init`

`npm i fastify` to install fastify