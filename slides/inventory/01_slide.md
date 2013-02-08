!SLIDE
# Inventory database

Section Objectives

* Use knife to query node database
* Ohai overview

!SLIDE
# Add another node

    > sudo knife bootstrap NODE2 --sudo \
      -x chefadmin -P violinrocks

!SLIDE
# Use knife search to query the node database

    > knife search node 'name:*student*'

!SLIDE
# How does it know?

    > ssh chefadmin@NODE1
    > ohai
