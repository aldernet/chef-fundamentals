!SLIDE
# Jenkins

Section Objectives

* Install real-world software service - Jenkins 
* Cover extra recipes that must be used in production

!SLIDE
# Download and extract Jenkins cookbooks

* apt
* java
* windows
* build-essential
* chef_handler
* apache2
* jenkins

!SLIDE
# Download old cookbook version

    > knife cookbook site download runit 0.16.2

!SLIDE
# Upload all cookbooks

    > sudo knife cookbook upload -a

!SLIDE
# Add recipes to node run_list

    > knife node run_list add NODE1 \
      'recipe[apt],recipe[apache2],recipe[jenkins]'
    run_list:
        recipe[apt]
        recipe[apache2]
        recipe[jenkins]
    > knife node show NODE1

!SLIDE
# Set node["jenkins"]["http_proxy"] attribute

    > knife node edit NODE1

    {
      ...
      "normal": {
        "jenkins": {
          "http_proxy": {
            "variant": "apache2"
          }
        }
      }


!SLIDE
# Install Jenkins

    > knife ssh "name:*" "sudo chef-client" \
      -x chefadmin -P violinrocks

Visit in your web browser:
http://NODE1

(username/password: jenkins/jenkins)

!SLIDE
# Jenkins cookbook created attributes

    > knife node edit NODE1

!SLIDE
# Install Git

Download and install the following cookbooks:

* dmg
* yum
* git

!SLIDE
# Install Git

    > knife node run_list add NODE1 'recipe[git]'
    > knife ssh "name:*" "sudo chef-client" \
      -x chefadmin -P violinrocks
    > knife ssh "name:*" "git --version" \
      -x chefadmin -P violinrocks

!SLIDE
# Remember from hello world

    > ssh chefadmin@NODE1

    > ls /etc/chef
    client.pem     client.rb     first-boot.json
    validation.pem

    > ls -al /usr/bin/chef*
    /usr/bin/chef-client ->
      /opt/chef/bin/chef-client

    > exit

!SLIDE
# More polish

    > knife ssh "name:*" "ls /etc/chef" \
      -x chefadmin -P violinrocks

Use the chef-client cookbook to remove validation.pem

Download and install these cookbooks:

* cron
* chef-client

!SLIDE
# More polish (contd.)

    > knife node run_list add NODE1 \
      'recipe[chef-client::delete_validation]'
    > knife ssh "name:*" "sudo chef-client" \
      -x chefadmin -P violinrocks
    > knife ssh "name:*" "ls /etc/chef" \
      -x chefadmin -P violinrocks

!SLIDE
# Print Chef info in motd

    > knife node run_list add NODE1 \
      'recipe[motd-tail]'
    > knife ssh "name:*" "sudo chef-client" \
      -x chefadmin -P violinrocks
    > ssh chefadmin@NODE_NAME


