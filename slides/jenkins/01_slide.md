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
