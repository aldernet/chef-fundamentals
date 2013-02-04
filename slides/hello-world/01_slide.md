# Hello World

Section Objectives

* Walk through basic Chef configuration workflow
* Install and configure fortune on node

# List nodes

Use `knife node list` to list all the node objects on the Chef Server.

    > knife node list

# Bootstrap chef-client on node

Integrating a new node into the infrastructure is easy with knife
bootstrap.

    > sudo knife bootstrap NODEIP --sudo \
      -x chefadmin -P violinrocks
    Bootstrapping Chef on NODE
    [lots of stuff]
    INFO: Running Report handlers
    INFO: Report handlers complete

# List nodes

    > knife node list
    NODE

# What did you do?

You installed chef-client on a node!

<center><img src="../images/overview_chef_quick.png"></center>

# Investigating further

    > ssh chefadmin@NODEIP
    > ls /etc/chef
    client.pem       client.rb
    first-boot.json  validation.pem
    > ls -al /usr/bin/chef
    /usr/bin/chef-client ->
      /opt/chef/bin/chef-client

# Create Chef Git Repository

    > git config --global user.name "Your Name"
    > git config --global user.email you@example.com

    > git clone https://github.com/opscode/chef-repo.git
    > cd chef-repo
    > rm -rf .git
    > git init
    > git add .
    > git commit -m "Initial Chef Repository"

# Download Cookbook from Community site

    > knife cookbook site download fortune
    Downloading fortune from the cookbooks site at version 0.0.1
     to /home/chefadmin/chef-repo/fortune-0.0.1.tar.gz
    Cookbook saved: /home/chefadmin/chef-repo/fortune-0.0.1.tar.gz

# Extract and view Cookbook
    > tar xzvf fortune*.tar.gz -C cookbooks
    > less cookbooks/fortune/README.md
    > less cookbooks/fortune/recipes/default.rb
    package "fortune" do
      action :install
    end

# Version control in SCM repository
    > git add cookbooks/fortune/*
    > git commit -m "Adding fortune cookbook"

# Upload cookbook to Chef Server
    > sudo knife cookbook upload fortune
    Uploading fortune       [0.0.1]
    Uploaded 1 cookbook.

# Add fortune to node's runlist

    > export EDITOR=vi
    > knife node edit NODEIP
    {
      "name": "xxx.example.com",
      "chef_environment": "_default",
      "normal": {
        "tags": [
        ]
      },
      "run_list": [
        "recipe[fortune]"
      ]
    }

# Run chef-client on node

    > ssh chefadmin@NODEIP
    > sudo chef-client

# Try it out!

    > fortunue

# Run chef-client AGAIN
    > sudo chef-client

# How does chef-client know?
    > ohai
