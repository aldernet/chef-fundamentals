!SLIDE
# Further Refinements

Section Objectives

* Edit node attributes
* Drive chef-client run remotely 
* Uninstall

!SLIDE
# Cowsay cookbook

    > knife cookbook site download package_installer
    > tar xzvf package_installer*.tar.gz -C cookbooks
    > less cookbooks/package_installer/README.md
    > less cookbooks/package_installer/recipes/default.rb
    > git add cookbooks/package_installer/*
    > git commit -m "Adding package_installer cookbook"
    > sudo knife cookbook upload package_installer

!SLIDE
# Use knife to edit node runlist

    > knife node edit NODE1

!SLIDE small code
# Use knife to edit node runlist (contd.)

    @@@javascript
    {
      ...
      "run_list": [
        "recipe[fortune]",
        "recipe[package_installer]"
      ]
    }

!SLIDE small code
# Use knife to edit node runlist (contd.)

    @@@javascript
    {
      ...
      "normal": {
        "package_installer": {
          "packages": {
            "cowsay": null
          }
        },
        "tags": [
        ]
      },
      "run_list": [ ... ]
    }

!SLIDE
# Knife ssh can run arbitrary commands

    > knife ssh "name:fred.example.com" "fortune" \
      -x chefadmin -P violinrocks

    > knife ssh "name:*" "uptime" \
      -x chefadmin -P violinrocks

!SLIDE
# Use knife ssh to run chef-client on node

    > knife ssh "name:*" "sudo chef-client" \
      -x chefadmin -P violinrocks

!SLIDE
# Fortunes via cow

    > ssh chefadmin@NODE1

    > fortune -a | cowsay
    > fortune -a | cowsay -f duck
    > fortune -a | cowsay -f elephant
    > fortune -a | cowsay -f stegosaurus

    > exit

!SLIDE
# Removing a recipe from a runlist

    > knife node run_list remove NODE1 'recipe[fortune]'
    > knife node show NODE1
    > knife ssh "name:*" "sudo chef-client" -x chefadmin -P violinrocks
    > knife ssh "name:*" "fortune" -x chefadmin -P violinrocks

!SLIDE
# Cookbook must explicitly support removal

    > knife node edit NODE1

!SLIDE code
# Cookbook must explicitly support removal (contd.)

    @@@ javascript
    { 
      ...
      "normal": {
        "package_installer": {
          "packages": {
            "cowsay": {
              "action": "remove"
            }
          }
        },
        "tags": [ ]
      },
      "run_list": [ ... ] 
    }

!SLIDE
# Cowsay no more!

    > knife ssh "name:*" "sudo chef-client" \
      -x chefadmin -P violinrocks

    > knife ssh "name:*" "fortune -a | cowsay" \
      -x chefadmin -P violinrocks
    bash: cowsay: command not found


!SLIDE
# Cleanup

    > knife node run_list remove NODE1 'recipe[package_installer]'
    > knife ssh "name:*" "sudo apt-get -y remove fortune-mod" \
      -x chefadmin -P violinrocks
    > knife node show NODE1

    > knife node edit NODE1

    {
      ...
      "normal": {
        "tags": [
        ]
      },
      ...
    }

    > knife ssh "name:*" "sudo chef-client" -x chefadmin -P violinrocks
