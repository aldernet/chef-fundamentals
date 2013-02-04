# Further Refinements

Section Objectives

* 
* 
*

# Cowsay cookbook

    > knife cookbook site download package_installer
    > tar xzvf package_installer*.tar.gz -C cookbooks
    > less cookbooks/package_installer/README.me
    > less cookbooks/package_installer/recipes/default.rb
    > git add cookbooks/package_installer/*
    > git commit -v
    > sudo knife cookbook upload package_installer

# Use knife to edit node runlist

    > knife node edit NODEIP

# Use knife to edit node runlist (contd.)

    @@@javascript
    {
      "name": "mt-ubuntu.misheska.local",
      "chef_environment": "_default",
      "normal": {
        "package_installer": {
          "packages": {
            "cowsay": null
          }
        },
        "tags": [
        ]
      },
      "run_list": [
        "recipe[fortune]",
        "recipe[package_installer]"
      ]
    }

# Knife ssh can run arbitrary commands

    > knife ssh "name:*" "uptime" -x chefadmin -P violinrocks
    > knife ssh "name:*" "fortune" -x chefadmin -P violinrocks

# Use knife ssh to run chef-client on node

    > knife ssh "name:*" "sudo chef-client" -x chefadmin -P violinrocks

# Fortunes via cow

    > ssh chefadmin@NODEIP
    > fortune -a | cowsay
    > fortune -a | cowsay -f duck
    > fortune -a | cowsay -f elephant
    > fortune -a | cowsay -f stegosaurus

# What happens when a recipe is removed from a runlist?

    > knife node run_list remove NODE_NAME 'recipe[fortune]'
    > knife node show NODE_NAME
    > knife ssh "name:*" "sudo chef-client" -x chefadmin -P violinrocks
    > knife ssh "name:*" "fortune" -x chefadmin -P violinrocks

# Cookbook must explicitly support removal

    > knife node edit NODE_NAME

# Cookbook must explicitly support removal (contd.)

    @@@javascript
    {
      "name": "mt-ubuntu.misheska.local",
      "chef_environment": "_default",
      "normal": {
        "package_installer": {
          "packages": {
            "cowsay": {
              "action": "remove"
            }
          }
        },
        "tags": [
        ]
      },
      "run_list": [
        "recipe[fortune]",
        "recipe[package_installer]"
      ]
    }

# Cowsay no more!

    > knife ssh "name:*" "sudo chef-client" -x chefadmin -P violinrocks
    > knife ssh "name:*" "fortune -a | cowsay" -x chefadmin -P violinrocks
