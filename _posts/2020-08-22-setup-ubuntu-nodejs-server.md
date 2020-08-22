---
layout: post
title: How I Set Up a Server To Host Multiple Node.JS Projects & Save $54+ Per App Annually vs Heroku
description: >
  We'll go through the steps of setting up a single Ubuntu 20.04 server to host
  multiple Node.JS projects in order to save money
sitemap: true
comments: true
---

1. this unordered seed list will be replaced by the toc
{:toc}

## What and Why

I have built several small lightly used Node.js projects on [Heroku][4]. I want
several of them to not have the limitations of a free server, namely the fact it
sleeps after 30 minutes & the ability to have free SSL certificates. There are a
few other gotchas with Heroku in general. According to their pricing, I'd
need to get, at minimum, a 'Hobby' server, one for every app. Each one costs $7
a month plus taxes which equates to more than $84 a year for each app.

I decided that I can do better. I went to [VULTR][6]'s site and looked
into their servers, which they call Cloud Compute. Their cheapest server
(and we can upgrade after deployment if needed), according to their
pricing, is $2.50 a month or $30 a year. When you add the fact that,
I can now host multiple apps on one server, the savings get greater with each
app hosted.

A few requirements before we dive right in.
*  Apps will need to support a single Node.js version, at the time of writing it
   is 12.18.3. To check your version once the server is setup simply run
   `node -v`
*  You need to know the difference between IPv4 and IPv6
*  You need to know how to set up DNS. The method is different for each DNS
   host
*  Each app needs to have its own apex domain, subdomain or combinations
   thereof assigned to it
*  All apps should be listening for the PORT environment variable
*  In the instructions, wherever you see `john` replace it with the non-root
   user you'll be creating and using

With that being said, you can use any server running Ubuntu 20.04 x64 should you
wish not to use [VULTR][6], some other options include [Linode][13],
[Amazon EC2][14], [DigitalOcean][4], and many more. Just skip the 'Steps to
deploy a new Cloud Compute server' section deploy the server elsewhere then
start with [Setup a non-root user with sudo privilege][12].
{:.note}

## Deploy a server and initially set it up

### Steps to deploy a new [VULTR][6] Cloud Compute Server:

1.  Login to [VULTR][6]
2.  In the top right corner, click the circle with a plus sign then 'Deploy New Serve'
3.  Because we can always scale up, I personally selected the cheapest
    type of server Cloud Compute server
4.  Choose a datacenter region that makes sense for your region
5.  Make sure Ubuntu is selected with '20.04 (LTS) x64'
6.  For the server size, I chose the cheapest which is $2.50 a month. If I ever
    need more resources, I can scale up.
7.  Optionally, but suggested, enable auto backups (additional cost)
8.  If you know about and have an SSH key, use that for auth. Either way, you
    will be able to find root's password where you can manage the server on
    [VULTR][6]'s site.
9.  Give the droplet a clever hostname
10. Optionally give the server an alternate label to find it on [VULTR][6]'s site
11.  Today we are only deploying 1 server
12. Click 'Deploy Now'
13. Log into your server using root over SSH (if you don't know how see
    [this article][1]). You will find the IP address info when you go to manage
    your Cloud Compute server's settings on [VULTR][6]'s site.
14. To update the list of packages run `apt update`
15. To upgrade out of date items run `apt upgrade`

### Setup a non-root user with sudo privileges

For the rest of this post, I will be using 'john' as a user, substitute it for whatever you want.
{:.note}

1.  Log into root via SSH if you are not already connected
2.  Let's set a temp variable to make the next bunch of steps `newuser=john`
3.  Run the command `adduser $newuser` and fill in the answers, if you want
4.  Run the command `usermod -aG sudo $newuser`

#### Optional 1 - If you wish to secure the server via SSH keys

1. Run the following set of commands to set up SSH for the new user account
```
mkdir -p /home/$newuser/.ssh
touch /home/$newuser/.ssh/authorized_keys
chmod -R go= /home/$newuser/.ssh
chown -R $newuser:$newuser /home/$newuser/.ssh
nano /home/$newuser/.ssh/authorized_keys
```
2. Find the public SSH key that you wish to use and paste it into the SSH session where nano is
   running, it should only take up one line
3. If you want multiple keys for the account, optionally add additional keys, 1 per line
4. Press 'ctrl-x' to exit, then 'Y' to save and then finally press enter to accept the file name

#### Optional 2 - If you want to lock down the server and disallow passwords to authenticate an SSH session

1. Run `sudo nano /etc/ssh/sshd_config`
2. Find the line containing 'PasswordAuthentication'. If there is a '#' before
   it, remove it, then make sure the line says `PasswordAuthentication no`
3. Press 'ctrl-x' to exit, 'Y' to save, then 'enter' to keep the existing filename
4. Lastly, run `sudo systemctl restart ssh`

### Setup initial firewall

1. Run the following command to enable the firewall `ufw allow OpenSSH`
2. Run the following command to permit only SSH connections `ufw enable`
3. Log off of the root account by running `exit`. From here on in we will be using the account, john

### Setup Nginx

1.  Connect to the server again using ssh and the account john
2.  Run the following `sudo apt update` to update the apt program, you will be asked for a password most likely, enter it
3.  Run the following `sudo apt install nginx` to install nginx
4.  Run the following `sudo ufw allow 'Nginx Full'` to enable HTTP/HTTPS traffic (ports 80 and 443)
5.  To verify everything is working navigate, from your browser to `http://your_server_ip` (HTTP,
    not HTTPS) you should get a standard 'welcome to nginx' page
6.  To fix a possible memory problem, let's set the hash bucket size by running `sudo nano /etc/nginx/nginx.conf`
7.  Look for the 'server_names_hash_bucket_size' directive and remove the '#' from the line
8.  Exit by pressing 'ctrl-x', save by pressing 'y', accept file name by pressing 'enter'
9.  Validate the config file by running `sudo nginx -t` you should get the following output if all
    is well:
    ```
    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: configuration file /etc/nginx/nginx.conf test is successful
    ```
10.  Restart nginx by running `sudo systemctl restart nginx`

### Prepare server to use [Let's Encrypt][2] SSL Certificates

certbot will manage the Let's Encrypt SSL certificates for us
{:.note}

1. To install certbot and it’s Nginx plugin run `sudo apt install certbot python3-certbot-nginx`

### Set up Node.js's latest LTS version

At the time of this writing, the latest LTS Node.js version is 12.18.3
{:.note}

1. Update apt-get with the correct package by running `curl -sL https://deb.nodesource.com/setup_lts.x | sudo -E bash -`
2. Let's install it by running `sudo apt-get install -y nodejs`
3. Let's install a build tool by running `sudo apt-get install -y build-essential`
4. Let's indicate that, by default, node apps should be running in production by doing the following
    1. Run `sudo nano /etc/environment`
    2. Add a new line containing `NODE_ENV="production"`
    3. Save the file by 'ctrl-x', 'y', and 'enter'
    4. Load the new environment variable by running `source /etc/environment`

### Install PM2, a process manager for Node.js

1. Run the following command to install PM2 `sudo npm install pm2@latest -g`
2. Acquire a command to setup PM2 to run at startup by running `pm2 startup systemd`
3. The last line of the output for the prior command, will be another command, run it
4. Kick off the initial run of the PM2 service by running `sudo systemctl start pm2-john` (replacing john)
5. Optional but recommended if you intend to have logging of your apps, we can
   set up log rotation. This will save 1 log file per period (defaults to daily,
   or when the file is larger than 10MB), and keep a maximum number of log files
   (which defaults to 30).
    1. Let's install pm2-logrotate by running `pm2 install pm2-logrotate`
    2. Optionally change the defaults of pm2-logrotate, see the
       [instructions here][9]

## Set up a new app with its corresponding domains

Three things: First this section can be done multiple times, once for every app
domain combo. Second, replace xyz.com & www.xyz.com with your URL(s).
Third, pick a unique port for this app, no two apps should share a port. Keep it
in mind, you'll need it twice.
{:.note}

### Set up DNS

Please be aware that it can take up to 48 hours for DNS changes to take effect.
{:.note}

1. Go to your DNS server and set up an A record pointing to the IPv4 address
   for each domain you wish to be serviced by this app
2. On your DNS server, set up an AAAA record pointing to the IPv6 address
   for each domain you wish to be serviced by this app

### Using git, copy over and set up the project

In the steps, NPM install is mentioned, no reason you can't use YARN as long as
you install it.
{:.note}

1. Use git to clone the repo into an appropriate folder, replace repo with the
   git address, and replace xyz.com with your chosen domain. `sudo git clone repo /var/www/xyz.com`
2. Take ownership of the folder by running `sudo chown -R $USER:$USER /var/www/xyz.com`
3. Change directory `cd /var/www/xyz.com`
4. If you need to npm install or run build commands, now is the time

### Set up PM2 to launch, manage, and provide environment variables to the app

1. Head back to the home folder by running `cd ~`
2. Let's create an ecosystem file, run `nano xyz.com.ecosystem.config.js`
3. Copy the below text into the SSH session
    ```js
module.exports = {
    apps : [{
    name: "demo",
    script: "/var/www/xyz.com/app.js",

    /*
    * uncomment the next line or two to enable logging both stdout and
    * error outputs to disk and optionally enable prepending the logs with a
    * timestamp.
    */
    // log_file: '/home/john/applogs/xyz.com/combined.log',  // don't forget to change from john
    // time: true, // enable prepending of a time-stamp

    // environment variables
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
    environment variables overriding any system variables if they match.
    Feel free to add additional ones as you need, they are unique to this
    app.
8.  If you wish to log to disk, uncomment the `log_file` line, changing both
    `john` and the `xyz.com`. Optionally uncomment the `time` line to have the
    logs prepended with a timestamp.
9.  Press 'ctrl-x' to exit, 'y' to save, 'enter' to accept the name
10. If opting to log to disk, let's create the folder for it by running
    `mkdir -p ~/applogs/xyz.com`
11. To start the app for the first time run `pm2 start xyz.com.ecosystem.config.js`
12. Let's save the PM2 process list and corresponding environments so that on
    server startup, it'll restart all the apps including the latest app we just
    added. Run `pm2 save`

A couple of commands for PM2 you should know about, especially as we update
our apps. In addition to starting the app as we did in step 11 above we can:
* Stop the app: `pm2 stop xyz.com.ecosystem.config.js`
* Restart the app: `pm2 restart xyz.com.ecosystem.config.js`
* Monitor your apps: `pm2 monit`

Read more in PM2's [documentation][11]

### Set up an nginx server block for this app

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
6. Let's check our work in the nginx configs by running `sudo nginx -t`
7. If all is well, run `sudo systemctl restart nginx`

### Set up SSL certificates for your domain

If this is your first time setting up certificates with certbot, you will be
prompted for an email address, and you'll need to accept an agreement
{:.note}

Please be aware that you may have issues if your DNS changes haven't fully
propagated yet
{:.note}

1. Let's run certbot with the --nginx plugin, using -d to specify the domain
   names we’d like the certificate to be valid for by running
   `sudo certbot --nginx -d xyz.com -d www.xyz.com`
2. Answer the question about how you want to handle non-SSL traffic, I recommend
   'Redirect' to redirect non-secure HTTP traffic to HTTPS.
3. The site should now be alive at http://xyz.com when you navigate there, you
   should be redirected to https://xyz.com

## Summary

We now have a server with one or more NodeJS apps running on it. We configured
the server with a node process manager (PM2) to take care of starting and
stopping Node.js apps as needed. We are now resilient against app crashes, it'll
be restarted automatically. When the server restarts, our apps will start back
up automatically as well. We set up a reverse proxy using nginx so that each app
will respond to its unique domain name(s). Lastly, we saved money vs using
Heroku.

A suggested next step would be if needed, set up your own hosted database, or
Redis server using this server or another.

A good chunk of the steps above was derived from DigitalOcean's wonderful
[tutorial][10] and it's prerequisites. I changed the order, modified some steps,
and added additional steps. I also added to it by illustrating how to set up
multiple apps.

If you've comments, as always please leave them below.

[1]: https://www.digitalocean.com/docs/droplets/how-to/connect-with-ssh/
[2]: https://letsencrypt.org/
[3]: https://nodejs.org/en/download/
[4]: https://m.do.co/c/7b7057283d32
[6]: https://www.vultr.com/?ref=8663193
[9]: https://github.com/keymetrics/pm2-logrotate#configure
[10]: https://www.digitalocean.com/community/tutorials/how-to-set-up-a-node-js-application-for-production-on-ubuntu-20-04
[11]: https://pm2.keymetrics.io/docs/usage/pm2-doc-single-page/
[12]: #setup-a-non-root-user-with-sudo-privileges
[13]: https://www.linode.com/
[14]: https://aws.amazon.com/ec2/
