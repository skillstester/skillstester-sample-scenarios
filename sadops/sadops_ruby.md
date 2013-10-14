# Introduction
for more information on this setup, check the sadops github repo:
<https://github.com/railsmachine/sadops>

# Scenario
## Survive Rails skills
At Railsmachine we have many situations that a skilled engineer has to cope with.
With [Sadops](http://sadops), we've tried to recreate these situations. Now get ready to fight the storm!

Extra bonus for automating the solution

The system will setup an ubuntu machine that will have ruby installed. Please wait while it prepares the system.

- @action: [Install Ruby](#action-ruby-install)
- @action: [Install Sadops](#action-sadops-install)

## Exercise
Something is happening , hang on ....

@action: [Trigger Memory Leak](#action-sad-leaky)

A memory leak was cause, can you fix it?

@check: [Check if Memory Leak solved](#check-sad-leaky-ok)

## Solution
@action: [Fix Memory Leak](#action-fix-sad-leaky)

# Checks
## check-sad-leaky-ok
- command: `cat test sad:leaky`
- cwd: /var/tmp/sadops
- user: sadops

# Actions
## action-ruby-install
- command: @codeblock
- user: root
- type: exec

```
# Remove current ruby
apt-get remove -y ruby
apt-get remove -y ruby1.8-dev
apt-get remove -y rubygems

# Install ruby 1.9.1
apt-get install -y ruby1.9.1
apt-get install -y rubygems1.9.1
```
## action-sadops-install
- command: @codeblock
- type: exec
- user: ubuntu

```
cd /var/tmp/

# Install nodejs first
apt-get install -y wget
wget http://nodejs.org/dist/v0.10.20/node-v0.10.20-linux-x64.tar.gz
tar -xzvf node-v0.10.20-linux-x64.tar.gz
cp node-v0.10.20-linux-x64/bin/node /usr/bin/node
chmod +x /usr/bin/node

# Clone the sadops repo
apt-get install -y git
git clone https://github.com/railsmachine/sadops.git
cd sadops

# Intialize mysql libs
apt-get install -y libmysqlclient-dev

# Install dependencies
gem install bundler --no-ri --no-rdoc
bundle install

apt-get install -y ab

# Setup hostname
#echo ... >> /etc/hosts

# Setup local ssh connection to sadops
ssh-keygen

# Allow
cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys

# Host sad.ops
#  User root

# moonshine.yml user -> vagrant
cap vagrant deploy:setup

# barfs on some error for 
cap vagrant deploy:setup

gem install -y shadow_puppet --no-ri --no-rdoc

mkdir /srv/sadops/releases

# Also needs sudo permissions
# Root needs to have ruby stuff in it's path

# gem install shadow_puppet --no-ri --no-rdoc

cap vagrant sadops:populate_things
```
## action-sad-leaky
- command: `cap vagrant sad:leaky`
- cwd: /var/tmp/sadops
- user: sadops

## action-fix-sad-leaky
- command: `cap vagrant deploy:reset`
- cwd: /var/tmp/sadops
- user: sadops
