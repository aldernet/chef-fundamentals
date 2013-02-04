# Data Bags

Section Objectives:

* Data bags

# Data Bags

Data bags are containers of "items" that are plain JSON data.

Items can contain any arbitrary key/value pairs, such as user
information, application setup parameters or DNS entries.

They are centrally available to recipes for processing.

# Data Bags

Data bags are indexed separately for search.

They are not tied specifically to roles or nodes.

Use data bags for storing information that is "infrastructure-wide".

# Creating Data Bags

Data bags go in the "data_bags" directory of the chef-repo.

Create a directory for the bag itself.

Put items in JSON files in the bag's directory.

We do not have a Ruby DSL for Data Bag items because they are free
form and can contain anything you want.

# Data Bag Use Case

As data bags can contain anything, they have unlimited uses. Some
common use cases we practice or have seen:

* User management
* Application information
* Hardware inventory
* Authentication credentials

# Encrypted Data Bags

Data bags can be encrypted with a secret key.

The key is something you generate on your local machine and do not
send to the Chef Server.

You still have to distribute the key file to systems that will need
access to encrypted data bags.

You probably already had a key distribution problem (think SSL).

# Example: Managing Users

    > mkdir data_bags/users
    > touch data_bags/users/USERNAME.json
    > $EDITOR data_bags/users/USERNAME.json
    {
      "id": "USERNAME"
      "groups": "sysadmin",
      "comment": "USERNAME",
      "uid": 2003,
      "shell": "/bin/bash",
      "ssh_keys": "ssh-rsa SSH_PUBLIC_KEY USERNAME@localhost"
    }
    > knife data bag create users
    > knife data bag from file users USERNAME.json

# Search Data Bags

Each bag gets a new index created.

Search can be done just like nodes or roles with knife or in a recipe.

    knife search BAG QUERY
    search(:bag, "QUERY")

Encrypted data bags cannot be searched because the contents are...
encrypted.

# Search Data Bags: Knife

    > knife search users "id:USERNAME"
    1 items found

    _rev:       6-c1c943b594f79bfae3ed043cca3a9de6
    chef_type:  data_bag_item
    data_bag:   users
    id:         USERNAME

Other keys in the item besides id can be used for the QUERY.

# Search Data Bags: Recipe


    user_list = search("users", "*:*")
    # or...

    search("users", "*:*") do |u|
      # something with u
    end

Query here is wildcard for all users, but this could be any query
relevant to information stored in the data bag items.

# Data Bags in Recipes

Besides using Search in recipes, data bags and data bag items can be loaded directly.

    data_bag()
    data_bag_item()

