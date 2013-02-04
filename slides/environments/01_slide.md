# Chef Node

Section Objectives:

* Environments

# Chef Environment

Every node has an environment. If you do not explicitly set one,
`_default` is used.

Chef environments allow you to set cookbook version constraints to
nodes in a particular environment, e.g. "production".

Retrieve the value of the node's environment with the
`Chef::Node#chef_environment` method.

    @@@ruby
    node.chef_environment # => "_default"

# Environments

Environments are used to enforce cookbook version constraints based on
logical infrastructure environments.

Different logical environments in the infrastructure may include
"production," "staging," "development" and so on.

Environments are assigned to nodes. They are managed as separate
entities on the Chef Server.

# Cookbook Version Constraints

Environments are most often used to set version constraints for
cookbooks.

This means that when Chef runs, it will only download the version
specified for the node's environment.

Version constraints can use operators to determine the version.

# Version Constraint Operators

    = Equal to
    > Greater than
    < Less than
    >= Greater than or equal to
    <= Less than or equal to

Advanced operators are available similar to those of RubyGems, see the
Version Constraint documentation.

# Example Environment

    name "production"
    description "Systems in production"
    cookbook_versions(
      "apache2" => "1.0.8",
      "mysql" = "1.0.0"
    )

# Environments: Nodes

The `Chef::Node` object has a method, `chef_environment` that can be
used to return the node's environment.

The default environment if none is assigned is `_default`. Version
constraints cannot be applied to the `_default` environment.

Searches can be constrained to the node's environment with a boolean
operator in the query.

    search(:node, "platform:ubuntu AND chef_environment:production")

# Environments: Configuration

The node gets its environment from the `Chef::Config[:environment]`
setting.

This can be passed to the `chef-client` command through the `-E`
option.

It can be set directly in the `/etc/chef/client.rb` configuration
file.

    environment "production"

# Environments: Knife

The knife `environment` sub-command can be used to work with
environments in the Chef Repository or on the Chef Server similar to
roles.

In the Chef Repository, environments are Ruby DSL or JSON files in the
`environments` directory.

    > knife environment --help
    > knife environment from file production.rb

Similar to roles, pick your file workflow between Ruby or JSON.

Knife bootstrap can set the node's environment at the first Chef run
with the `-E` option.

    > knife bootstrap IPADDRESS -E 'production'

# Environments: Recipes

The purpose of environments is to enforce versions of cookbooks for
particular systems. However, they can be used to constrain searches or
affect other behavior in recipes.

For example, search for other nodes that share *this* node's
environment:

    @@@ruby
    search(:node, "chef_environment:#{node.chef_environment}")

