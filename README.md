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

Next you can either restart your droplets or run the command `source ~/.bashr` to restart the service without restarting your droplet

### Installing Fastify for your NodeJS server

Inside of your src directory create a new node project.

To initialize it you will need to run these commands:

`npm init`

`npm i fastify` to install fastify

Inside your src directory create an index.js file and add in the fastify hello world example

The file should look like this: 

![index.js](/images/indexjs.png "index.js")

Now just test it locally, to do so within your local machine make sure you `cd` the src directory

Run the command `node index.js`, follow the link it gives gives mine is 127.0.0.1:5050

It should open a tab on your browser and display `hello: "Server X"`

At this point you can transfer your index.js file within your src and index.html inside your html directory to your droplets, you will need to put the index.html in `/var/www/html`
You will have to create the /www/html directories on your droplets

The index.js can go anywhere, I chose to create an src directory in my home and then put the index.js in there.

To transfer these files we will be using sftp, you will have to cd into the directory where the file lives and then run this command:
`sftp -i ~/.ssh/[key name] [droplet name]@[ip address]`

then you run: `put [file name]`, both these files should now be in both of your droplets in your home directory.

From there you move the files into the specified directories you made prior to moving the files.

At this point you should have:

index.html in /var/www/html

index.js in home/user/src

## Creating the Caddyfile and Caddy service file

Now you can create the Caddyfile and Caddy service file

Were going to start with writing the Caddyfile, on your local machine create the file. Nanme it **Caddyfile**

It should look like this:

![Caddyfile](/images/Caddyfile.png "Caddyfile")

The ip in the first line should be the one for your load balancer

To start the caddy server you can either run the command `sudo caddy run -- config /etc/caddy/Caddyfile`

or you can create a service file so that you aren't running this command all of the time, I am going to create the service file.

Create the service file on your local machine, it should look like this:

![caddy service file](/images/caddy_service.png "caddy.service")

Now that you have both the Caddyfile and caddy.service created you can move them to your droplets, again I will be using sftp to do so.

Once you have moved them to both to your droplets you can move them to the system directory with:

`sudo cp /home/user/caddy.service /etc/systemd/system`

## Creating the service file for the node project

Now we are going to write the service file to start the web app, within WSL you are going to create the file, it should look like this:

![web service file](/images/hello_web_service.png "hello_web.service")

## Running and testing the services

Now with everything in the correct place you can begin to start the services and test your web app.

First start with the caddy.service file, run these commands to start the service and enable it on startup:

```
sudo systemctl start caddy.service
sudo systemctl enable caddy.service
sudo systemctl status caddy.service

```
The last command will check the status of the service file, it should look like this:

![caddy service status](/images/caddy_service_status.png "caddy service status")


