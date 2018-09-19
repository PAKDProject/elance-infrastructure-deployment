# Elance.site deployment repo - POWERED BY ANSIBLE

### 1. Right what is all this about?
So in order to speed up some time dealing with trying to do the impossible task of ssh-ing into every single server and then running each task of updating, installing, configuring seperately we are going to use some automation powered by Ansible.

### 2. So what is ansible?
Good question! Ansible is an automation tool which allows you to sit back and relax while ansible performs the boring tasks!
For example: each ubuntu server should be updated, firewalled and basic necessary programmes should be installed before you do anything. So instead of this:

```
> ssh yourMachine@server
> yourMachine@server> sudo apt update
> yourMachine@server> sudo ufw allow 80,443/tcp
> yourMachine@server> sudo ufw enable
> yourMachine@server> sudo apt install nginx
> yourMachine@server> service nginx restart
> yourMachine@server> ...
```

All you have to do is this:
```
> ansible-playbook -i production/inventory bootstrap-server.yml
```

### 3. So how does all this work?
You see ansible uses a system of hosts and inventories which allows you to specify what tasks run on which machines.
For example looking at the development inventory we see
```
[frontend]
dev.elance.site
```
What does it all do? It assigns a group called frontend to the link of dev.elance.site under the development environment. 
In simple words, once you run a playbook which does something to frontend servers, using this inventory, dev.elance.site server will be ssh'd into and all the tasks will be ran.
We can extend this much more to include more servers:
```
[frontend]
dev.elance.site
moredev.elance.site
192.168.10.1
```
... or even have ansible roll through many different templated links such as:
```
www[1-99].elance.site
```
*NOTE: THE ABOVE WILL DEPLOY TO ALL www1.elance.site ALL THE WAY TO www99.elance.site SETS OF SERVERS*

### 4. Is that it?
No this is only the basics.
We can also store variables related to each group under *group_vars/* folder using yaml structure which then allows you to make tasks as dynamic as possible.

### 5. What are tasks?
Tasks are simple yaml files which set out what has to be done when they're ran.
For example in order to install nginx:
``` yaml
- name: Install Nginx
  become: yes
  apt: name=nginx state=present
  notify: 
    - Restart Nginx
```

What this does is :
1. It creates a task called 'Install Nginx'
2. It goes into sudo mode
3. It installs nginx
4. I notifies another sub task in handlers folder in same task directory to perform a sub task.

### 6. Right enough. How to run a playbook?
Well to run a playbook we follow this format:

``` 
> ansible-playbook -i path/to/inventory playbookName.yml --extra-vars "addAnyExtraVarsNeeded=Here"
```

### 7. What is this vagrant file?
This is a testing environment set out to test the app locally in a server like environment. To run it:
1. Install [VirtualBox](https://www.virtualbox.org/)
2. Install [Vagrant](https://www.vagrantup.com/)
3. Edit your /etc/hosts to include this line
```
dev.elance.site  10.0.0.10
```
4. Run ```vagrant up``` from the project directory. 