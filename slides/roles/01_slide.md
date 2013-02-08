!SLIDE
# Roles

Section Objectives:

* Create roles
* View roles on the Chef Server
* Apply roles to nodes

!SLIDE
# Add another node

    > sudo knife bootstrap NODE2 --sudo \
      -x chefadmin -P violinrocks

    > knife node list

!SLIDE
# CentOS needs yum cookbook

Download & install yum cookbook

!SLIDE
# Now knife ssh wildcards matter

    > knife ssh "name:*" "uptime" \
      -x chefadmin -P violinrocks

    > knife ssh "platform:ubuntu*" "uptime" \
      -x chefadmin -P violinrocks

    > knife ssh "platform:centos*" "uptime" \
      -x chefadmin -P violinrocks

    > knife search "platform:ubuntu*"

!SLIDE
# Building Roles

What kind of roles do we need?

* `base` role.
* platform roles.
* per-service roles.

!SLIDE
# Create a base role

    > knife role create base

!SLIDE
# Export role to file

    > knife role show base -Fj > roles/base.json

    > knife role from file base.json

!SLIDE
# roles/base.json

    @@@ruby
    {
      "name": "base",
      "description": "Base role applied to all nodes.",
      "json_class": "Chef::Role",
      "default_attributes": { },
      "override_attributes": { },
      "chef_type": "role",
      "run_list": [ 
        "recipe[chef-client::delete_validation]"
      ],
      "env_run_lists": { }
    }

!SLIDE
# Apply Roles to a Node

    > knife node run_list add NODE2 'role[base]'

    > knife node run_list remove NODE1 \
      'recipe[apt],recipe[apache2],recipe[jenkins]'

    > knife node run_list add NODE1 'role[base]'

.notes On Windows, use cmd.exe not powershell, else an erroneous entry
will be made (e.g., recipe[roles]).

!SLIDE
# Run chef-client on all the nodes

    > knife ssh "name:*" "sudo chef-client" -x chefadmin -P violinrocks

!SLIDE
# Create an ubuntu role

    > knife role create ubuntu

!SLIDE
# Export role to file

    > knife role show ubuntu -Fj > roles/ubuntu.json

    > knife role from file ubuntu.json

!SLIDE
# roles/ubuntu.json

    @@@ruby
    {
      "name": "ubuntu",
      "description": "Role applied to all Ubuntu nodes",
      "json_class": "Chef::Role",
      "default_attributes": { },
      "override_attributes": { },
      "chef_type": "role",
      "run_list": [
        "recipe[apt]",
        "role[base]"
      ],
      "env_run_lists": { }
    }

!SLIDE
# Create a CentOS role

    > knife role create centos

!SLIDE
# Export role to file

    > knife role show centos -Fj > roles/centos.json

    > knife role from file centos.json

!SLIDE
# roles/centos.json

    @@@ruby
    {
      "name": "centos",
      "description": "Role applied to all CentOS nodes",
      "json_class": "Chef::Role",
      "default_attributes": { },
      "override_attributes": { },
      "chef_type": "role",
      "run_list": [
        "recipe[yum]",
        "role[base]"
      ],
      "env_run_lists": { }
    }

!SLIDE
# Apply to all CentOS nodes

    > knife node run_list remove NODE2 "role[base]"
    > knife node run_list add NODE2 "role[centos]"

!SLIDE
# Run chef-client on all the nodes

    > knife ssh "name:*" "sudo chef-client" -x chefadmin -P violinrocks


!SLIDE
# Per-service roles

    > knife role create jenkins_master

    > knife role show jenkins_master -Fj > roles/jenkins_master.json

!SLIDE
# roles/jenkins_master.json

    @@@ruby
    {
      "name": "jenkins_master",
      "description": "Systems that act as a Jenkins web dashboard",
      "json_class": "Chef::Role",
      "default_attributes": { },
      "override_attributes": { },
      "chef_type": "role",
      "run_list": [
        "recipe[apache2]",
        "recipe[java]",
        "recipe[jenkins]"
      ],
      "env_run_lists": { }
    }

!SLIDE
# Apply the jenkins_master role

    > knife role from file jenkins_master.json

    > knife node run_list add NODE1 "role[jenkins_master]"
    > knife node run_list add NODE2 "role[jenkins_master]"

    > knife ssh "name:*" "sudo chef-client" \
      -x chefadmin -P violinrocks

!SLIDES
# Jenkins x 2

http://NODE1:8080
http://NODE2:8080
