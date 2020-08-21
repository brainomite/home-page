---
layout: post
title: How I Set Up a Server To Host Multiple Node.JS Projects & Save Money
description: >
  We'll go through the steps of setting up a Ubuntu 20.04 server to host multiple Node.JS projects in order to save money.
sitemap: true
comments: true
---

1. this unordered seed list will be replaced by the toc
{:toc}

Hodor! Hodor hodor. Hodor, hodor, hodor hodor. HODOR HODOR! Hodor... Hodor hodor. Hodor, hodor, hodor hodor. HODOR hodor, hodor. Hodor, hodor. Hodor. Hodor. HODOR HODOR! HODOR HODOR! Hodor hodor HODOR! Hodor. Hodor? Hodor, hodor, hodor hodor. HODOR hodor, hodor.

HODOR hodor, hodor. Hodor, hodor, hodor hodor. HODOR? Hodor? Hodor hodor HODOR! Hodor. Hodor hodor HODOR! Hodor.

We are going to:

1. Deploy a DigitalOcean Droplet
2. Setup a non-root admin user with sudo privileges
3. Optionally lock down the server to only permit remote login via SSH Keys
4. Setup a firewall
5. Install nginx

Hodor hodor! HODOR hodor, hodor. Hodor hodor HODOR! Hodor. Hodor hodor. Hodor? Hodor, hodor. Hodor. Hodor. HODOR? Hodor? HODOR hodor, hodor.

Hodor hodor. Hodor, hodor. Hodor. Hodor. Hodor hodor! HODOR hodor, hodor. Hodor... Hodor hodor. Hodor hodor. HODOR? Hodor, hodor, hodor hodor. Hodor hodor! Hodor, hodor. Hodor. Hodor. Hodor hodor. HODOR hodor, hodor. Hodor! Hodor hodor.

## Deploy a DigitalOcean Droplet (Virtual Private Server) and initially set it up

### Steps to deploy a new droplet:

1.  Login to DigitalOcean
2.  In top right corner, click 'Create' then 'Droplets'
3.  Make sure Ubuntu is selected with '20.04 (LTS) x64'
4.  Because we can always scale up is needed I personally selected the cheapest
    (scroll to the left) basic server, which is $5 at the time of this post
5.  Choose a datacenter that makes sense for your region
6.  Enable IPv6, and Monitoring in Additional options
7.  If you know about, and have an SSH key, use that for auth, otherwise setup a
    password
8.  Today we are only deploying 1 droplet
9.  Give the droplet a clever name
10. Optionally enable backups (additional cost)
11. Click 'Create Droplet'

### Setup a non-root user with sudo privileges

For the following steps, I will be using 'john' as a user, substitute it for whatever you want.
{:.note}

1.  Log into your server using root over SSH (if you don't know how see [this article][1])
2.  Run the command `adduser john` and fill in the answers
3.  Run the command `usermod -aG sudo john`

#### Optional 1 - If you wish to secure the server via SSH keys

1. Run the following set of commands to set up SSH for the new user account (remember to replace john with the appropriate user)
```
mkdir -p /home/john/.ssh
touch /home/john/.ssh/authorized_keys
chmod -R go= /home/john/.ssh
chown -R john:john /home/john/.ssh
nano /home/john/.ssh/authorized_keys
```
2. Find your public SSH key that you wish to use
3. Paste the public key into the SSH session where nano is running, it should only take up one line
4. If you want multiple keys for the account, optionally add additional keys 1 per line
5. press 'ctrl-x' to exit, then 'Y' to save and then finally press enter to accept the file name
6. To lock down the server further, run `sudo nano /etc/ssh/sshd_config`

#### Optional 2 - If you want to lock down the server and disallow password to authenticate a SSH session

1. Run `sudo nano /etc/ssh/sshd_config`
2. Find the line containing 'PasswordAuthentication' if there is a '#' before it remove it then make sure the line says `PasswordAuthentication no`
3. Press 'ctrl-x' to exit, 'Y' to save, then 'enter' to keep the existing file name
4. Lastly run `sudo systemctl restart ssh`

### Setup initial firewall

1. Run the following command to enable the firewall `ufw allow OpenSSH`
2. Run the following command to permit only SSH connections `ufw enable`
3. Log off of the root account by running `exit`. From here on in we will using the account, john

### Setup Nginx

1.  Connect to the server again using ssh and the account john
2.  Run the following `sudo apt update` to update the apt program, you will be asked for a password most likely, enter it
3.  Run the following `sudo apt install nginx` to install nginx
4.  Run the following `sudo ufw allow 'Nginx Full'` to enable http/https traffic (ports 80 and 443)
5.  To verify everything is working navigate, from your browser to `http://your_server_ip` (http, not https) you should get a standard 'welcome to nginx' page
6.  To fix a possible memory problem, lets set the hash bucket size by running `sudo nano /etc/nginx/nginx.conf`
7.  Look for the 'server_names_hash_bucket_size' directive and remove the '#' from the line
8.  Exit by pressing 'ctrl-x', save by pressing 'y', accept file name by pressing 'enter'
9.  Validate the config file by running `sudo nginx -t` you should get the following output:
10.  Restart nginx by running `sudo systemctl restart nginx`

### Prepare server to use [Let's Encrypt][2] SSL Certificates

certbot will manage the Let's Encrypt SSL certificates for us
{:.note}

1. To install certbot and it’s Nginx plugin run `sudo apt install certbot python3-certbot-nginx`

### Set up Node.js's latest LTS version

At the time of this writing, the latest LTS Node.js version is 12.18.3
{:.note}

1. Update apt-get with correct package by running `curl -sL https://deb.nodesource.com/setup_lts.x | sudo -E bash -`
2. Lets actually install it by running `sudo apt-get install -y nodejs`
3. Lets install a build tool by running `sudo apt-get install -y build-essential`
4. Lets indicate that, by default, node apps should be running in production by doing the following
    1. Run `sudo nano /etc/environment`
    2. Add a new line containing `NODE_ENV="production"`
    3. Save the file by 'ctrl-x' , 'y', and 'enter'
    4. Load the new environment variable by running `source /etc/environment`

### Install PM2, a process manager for Node.js

1. Run the following command to install PM2 `sudo npm install pm2@latest -g`
2. Acquire a command to setup PM2 to run at startup by running `pm2 startup systemd`
3. The last line of the output for the prior command, will be another command, run it
4. Kick off the initial run of the PM2 service by running `sudo systemctl start pm2-john` (replacing john)

## Set up a new app with it's corresponding domains

Three things: First this section can be done multiple times, once for every app
domain combo. Second, replace xyz.com & www.xyz.com with your url(s).
Third, pick a unique port for this app, keep it in mind, you'll need it twice.
{:.note}

### Set up dns

Please be aware that it can take up to 48 hours for dns changes to take effect.
{:.note}

1. Go to your dns server and set up an A record pointing to the IPv4 address
   for each domain you wish to be serviced by this app
2. On your dns server, set up an AAAA record pointing to the IPv6 address
   for each domain you wish to be serviced by this app

### Using git, copy over and set up the project

In the steps, NPM install is mentioned, no reason you can't use YARN as long as
you install it.
{:.note}

1. Use git to clone the repo into an appropriate folder, replace repo with the
   git address and replace xyz.com with your chosen domain. `sudo git clone repo /var/www/xyz.com`
2. Take ownership of the folder by running `sudo chown -R $USER:$USER /var/www/xyz.com`
3. Change directory, and npm install `cd /var/www/xyz.com`
4. If you need to npm install or run build commands, now is the time

### Set up pm2 to launch, manage, and provide environment variables to the app

1. Head back to the home folder by running `cd ~`
2. Lets create an ecosystem file, run `nano xyz.com.ecosystem.config.js`
3. Copy the below text into the SSH session
    ```js
    module.exports = {
      apps : [{
        name: "demo",
        script: "/var/www/xyz.com/app.js",
        env: {
          PORT: "3000",
        },
      }]
    }
    ```
    ~/xyz.com.ecosystem.config.js
    {:.figcaption}

4.  Replace `demo` with a proper name
5.  Replace the `script` path with the fully qualified path to the entry of
    your program.
6.  Change the PORT to the correct port you wish to use, keeping in mind
    each app should have a unique port.
7.  Within the env object, each pair will be fed into your program as an
    environment variables, overriding any system variables if they match.
    Feel free to add additional ones as you need, they are unique to this
    app.
8.  Press 'ctrl-x' to exit, 'y' to save, 'enter' to accept the name
9.  To start the app for the first time run `pm2 start xyz.com.ecosystem.config.js`
10. Lets save the PM2 process list and corresponding environments so that on
    server startup, it'll restart all the apps including the latest app we just
    added. Run `pm2 save`

### Set up a nginx server block for this app

1. To create and edit the server block file, run `sudo nano /etc/nginx/sites-available/xyz.com`
2. Paste the following into you shell session
    ```
    server {
        listen 80;
        listen [::]:80;

        server_name xyz.com www.xyz.com;

        location / {
            proxy_pass http://localhost:3000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
    ```
3. Replace the xyz.com (there are two domains in this example listed,
   you can do one, two or more on the server_name line)
4. If necessary change the port from 3000 to the correct port the app is
   listening on
5. Next, let’s enable the file by creating a link from it to the `sites-enabled`
   directory, which Nginx reads from during startup by running
   `sudo ln -s /etc/nginx/sites-available/xyz.com /etc/nginx/sites-enabled/`
6. Lets check our work in the nginx configs by running `sudo nginx -t`
7. If all is swell run `sudo systemctl restart nginx`

### Set up SSL certificates for your domain

If this is your first time setting up certificates with certbot, you will be
prompted for an email address, and you'll need to accept an agreement
{:.note}

Please be aware that you may have issues if your DNS changes hasn't fully
propagated yet
{:.note}

1. Lets run certbot with the --nginx plugin, using -d to specify the domain
   names we’d like the certificate to be valid for by running
   `sudo certbot --nginx -d xyz.com -d www.xyz.com`
2. Answer the question about how you want to handle non SSL traffic, I recommend
   'Redirect' to redirect non secure http traffic to https.
3. The site should now be alive at http://xyz.com, when you navigate there, you
   should be redirected to https://xyz.com

[1]: https://www.digitalocean.com/docs/droplets/how-to/connect-with-ssh/
[2]: https://letsencrypt.org/
[3]: https://nodejs.org/en/download/
