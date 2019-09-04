---
layout: post
title:  "Simple Salt Master-Minion Architecture Up and Running"
date:   2018-04-30 13:47:45
description: Simple Salt Master-Minion Architecture Up and Running
categories:
- blog
---
<head><link rel='shortcut icon' type='image/png' href='/favicon.png' /></head>

When I started working with Salt the only thing I wanted to was to have a simple architecture, one master and one minion, up and running to help me understand the concept and all the theory I had been reading, and that is why I decided to write this post. So the idea is basically that you have a simple Master-Minion architecture that you can use to apply different configuration and expand your knowledge in Salt. For that I recommend you to read the [Salt documentation](https://docs.saltstack.com/en/latest/contents.html). If you read this post until the end and follow the instructions you will have a salt-master and a salt-minion running locally in your computer, in a production environment these two will most likely be located in different machines.

## Salt installation
The first thing to do is installing Salt, an easy way to do this is by using the salt bootstrap. Get it by running the following command.
```
wget -O bootstrap-salt.sh https://bootstrap.saltstack.com/develop
```
After the download has finished install the salt-minion and salt-proxy by executing:
```
sudo sh bootstrap-salt.sh
```
And finally the slat-master with the command:
```
sudo sh bootstrap-salt.sh -M
```
## Minion config file
After you are done with the installation the first thing to do is to configure the minion config file to specify to the minion that the Master can be found in localhost, this means the minion and the master are going to be running in the same machine, in this case the machine where you installed salt.

Open the minion config file `/etc/salt/minion` and on top of the file the line:

```
master: localhost
```

## Accept minion key in the master
Restart the minion service:
```
service salt-minion restart
```
Now you need to accept the minion key. See the list of keys with the command:
```
sudo salt-key
```
Under `Unaccepted Keys:` you will see the name of your device, i.e. your minion. Yes this is the first time you see your device name, and yes you never assigned this name before. The minion take this name from the device you are running, so you should see that somehow it is related to the machine where the minion was installed.

You can accept the keys in the `Unaccepted Keys:` section by running:
```
sudo salt-key -A
```
This will accept all the keys in the section, you can also accept the keys individually with the command:
```
sudo salt-key -a <device_name>
```
Where, as always, `<device_name>` must be replaced by the name of your device. Yes, the name of your device that is under `Unaccepted Keys:`, don't look any further.

## Do several tests
You can test your configuration with the command:
```
sudo salt '*' test.ping
```
With this command the server is telling to all the minions to execute `test.ping` in parallel and return the result. The result of executing this command should be something like:
```
<device_name>:
    True
```
And that's all. I guess now you can see better what the basic idea is. Of course Salt capabilities don't end in telling the minions to do a ping. There are plenty of functions to be executed in minions which of course increase the complexity and capabilities of Salt.

After having this little example up and running I truly recommend you to [Sending the First Commands](https://docs.saltstack.com/en/latest/topics/tutorials/walkthrough.html#sending-the-first-commands) section or even better, move forward to the [Salt States](https://docs.saltstack.com/en/latest/topics/tutorials/walkthrough.html#salt-states) section which I find much more interesting and fun to play with. Until next time.

## References:
[1] https://docs.saltstack.com/en/latest/topics/tutorials/walkthrough.html
[2] https://github.com/napalm-automation/napalm-salt
[3] https://mirceaulinic.net/2016-11-17-network-orchestration-with-salt-and-napalm/

