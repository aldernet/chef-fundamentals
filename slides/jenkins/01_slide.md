# Jenkins

Section Objectives

* 
* 
*

# Download and extract Jenkins cookbooks

* apt
* runit
* java
* windows
* build-essential
* chef_handler
* jenkins

# Upload all cookbooks

    > sudo knife cookbook upload -a

# Add recipes to node run_list

    > knife node run_list add NODE_NAME 'recipe[apt],recipe[jenkins]'
    run_list:
        recipe[apt]
        recipe[jenkins]
    > knife node show NODE_NAME

# Install Jenkins

    > knife ssh "name:*" "sudo chef-client" -x chefadmin -P violinrocks

Visit in your web browser:
    http://NODE_NAME:8080

# Install Git

    > knife cookbook site download runit 0.16.2

Download and install the following cookbooks:
* dmg
* yum
* runit
* git

    > knife node run_list add NODE_NAME 'recipe[git]'
    > knife ssh "name:*" "sudo chef-client" -x chefadmin -P violinrocks
    > knife ssh "name:*" "git --version" -x chefadmin -P violinrocks

# More polish

    > knife ssh "name:*" "ls /etc/chef" -x chefadmin -P violinrocks

Use the chef-client cookbook to remove validation.pem

Download and install these cookbooks:

* cron
* chef-client

# More polish (contd.)

    > knife node run_list add NODE_NAME 'recipe[chef-client::delete_validation]'
    > knife ssh "name:*" "sudo chef-client" -x chefadmin -P violinrocks
    > knife ssh "name:*" "ls /etc/chef" -x chefadmin -P violinrocks

# Print Chef info in motd

    > knife node run_list add NODE_NAME 'recipe[motd-tail]'
    > knife ssh "name:*" "sudo chef-client" -x chefadmin -P violinrocks
    > ssh chefadmin@NODE_NAME
