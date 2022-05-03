# Turning your Monero Node Into a 'Service'

## Prerequisites:

* Local Node

<hr/>

### INTRO

Are you tired of initialising your Node manually every time you start your computer?

Do you wish there was a hands off, easy solution for restarting it when it fails?

Well, we've got good news for you, there is! and in this short video we'll be explaining how.


### WHAT IS A SERVICE?

A service is a common way to describe a program that runs in the background of any Operating System (OS).

Many programs run as services to make your computer use simple and stress free without you noticing.

Typically, you will have something called a service manager, which takes charge and deals with these programs for you. 

Today we'll be setting some rules to tell our service managers to run our Monero Nodes in the background every time our PCs starts.

As usual, we will be covering Linux and Windows in separate sections. Please use the time stamps in the description to get where you want to be.

### SETTING UP OUR SERVICE - LINUX

As Linux users, we often find that things are slightly more complex and time consuming to get set up. 

But as always, there are advantages.

We will continue by assuming you followed our video to set up your node. If you already have you  node set up via other means. Please check out a video to understand how we approached things.

The first thing we’re going to do is change the directory in which our monerod folder is located. 

Right now our folder is located in the home directory and we’re going to move it to the /opt/ directory. /opt/ aka “Option” or “Optional”, is traditionally used to hold software and programs which are installed as an addition.

Changes to this directory require elevated privileges and therefore cannot be altered easily. 

To do this, we are doing to use the mv command.

`sudo mv ~/monerod /opt/`

Next we’re going to create a new user for our systems called “monerod”. Doing this means we can isolate the privileges of the service to working folder, which is great for security amongst other things.

This first command will add the new user, create a new user group under the same name and then set the home directory to that which we just moved.

`sudo useradd --user-group --home-dir=/opt/monerod monerod`

Next we will change the owner of the monerod directory to match the username. This is give the monerod user the correct privileges to alter the files inside. 

`sudo chown -R monerod:monerod /opt/monerod`

Now that’s done we need to get on and specify the service we will be creating.

Systemd is the name of the service manager typically found in Linux distributions. And it’s within its directory which we will need to place our config.

With your favourite text editor, you will need to create a new file in the */lib/* directory.

We’ll be using vim, and the following command:

`sudo vim /lib/systemd/system/monerod.service`

It’s in this file that you will need to define the configuration of your service.

We have already put together a template for you to use, which can be found on your Github page.

Simply copy and paste it into the terminal and your done!

All that’s left to do is reload systemd, enable our new service and then start it. We can do that with the next three commands:

`sudo systemctl daemon-reload`
`sudo systemctl enable monerod.service`
`sudo systemctl start monerod.service`

To check that everything is running smoothly we have a few commands that we can use the command `systemctl status monerod.service`. If all is well, you should see that it is Active and running.

Now every time you start your PC your node will be up and running and if it ever fails, systemd will be there to restart it.

### SETTING UP OUR SERVICE - WINDOWS

Setting up your windows service is quite simple as its execution has been built into the daemon.

To install the daemon as a service, first run Powershell "as administrator". If you already have a node and used the default directory, then you only need to add the "--install-service" flag.
If you followed our guide, or set your data directory manually, you should also include the "--data-dir" flag, followed by the correct location. 

 C:\monerod\monerod.exe --install-service --data-dir C:\monerod\data


Now when we head to the *Services* application in Windows, we will find one called "Monero Daemon".

So that this service runs and restarts automatically, we will need to edit the configuration.

Right click on the service and select *Properties*.

First we want to change the startup type. I'm going to select *Automatic*, with a delayed start. This way I know that essential services like networking start ahead of it.

The next thing we want to turn our attention to is the *recovery* tab. Here we can tell Windows what to do if the node fails for any reason. For each failure, I want Windows to restart the Monero Daemon, so that it's up an running as much as possible.

After making those changes, we can start the process. If you're already running the daemon, you should stop it first.

There we go. Your Node should now be running as a Windows service.

If you're interested to know the status of your node you can also check the logs. Our logs are located in the data directory and they should be printed quite regularly.

If for whatever reason you are unable to access your node. You should start with these.

### OUTRO

Well there you have it. Your node is now an integrated part of your computing experience.

If you have any questions please let us know either in the comments or via one of our contact options on the [about section](https://moneroguides.org/about/) of our website.

Finally, if you found this useful and want to send us a tip our address can be found on screen. 

That’s it from us, see you in the next one.
