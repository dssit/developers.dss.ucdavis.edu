---
layout: post
title:  Manipulating Active Directory with Ruby
date:   2015-06-04
description: Manipulating Active Directory with Ruby can be simple and easy.
categories:
- Ruby
- Active-Directory
- LDAP
permalink: manipulating-active-directory-with-ruby
author: cthielen
---

Ruby and Active Directory may be worlds apart but can play nice together thanks
to Active Directory's LDAP underpinnings.

___

### Active Directory is LDAP

[Active Directory](http://en.wikipedia.org/wiki/Active_Directory) is a popular solution for managing authentication, authorization, and directory listings in computer networks. It is built on top of LDAP version 2 and 3 and a Microsoft variant of Kerberos and DNS.

[LDAP](http://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol) is an open protocol designed for very fast reading. Data within LDAP is organized into a tree-like structure and queries are generally made against a specified subtree.

### Ruby Speaks LDAP Well

[net-ldap](https://github.com/ruby-ldap/ruby-net-ldap) is a well-established Ruby gem for communicating with LDAP servers.

Let's jump right in and search for a user in Active Directory.

{% highlight ruby %}
  require 'net-ldap'

  # Describe server's address and provide authentication
  server = {
    :host => 'ldap.example.com',
    :base => 'ou=People,dc=ldap,dc=example,dc=com',
    :port => 636,
    :encryption => :simple_tls,
    :auth => {
      :method => :simple,
      :username => 'somebody',
      :password => 'something'
    }
  }

  # 'Binding' is LDAP vernacular for 'connecting'
  conn = Net::LDAP.new(server)
  conn.bind

  results = conn.search(:filter => Net::LDAP::Filter.eq("sAMAccountName", 'a_loginid'))

  raise "LDAP error. Code: #{conn.get_operation_result.code }, Reason: #{conn.get_operation_result.message}" unless conn.get_operation_result.code == 0

  raise "No users found or too many users found." unless results.length == 1

  user = results[0]

  # Print their display name (typically their full name)
  puts user[:displayname]

  # Print their user creation date
  puts user[:whencreated]

  # Print their street address
  puts user[:streetaddress]

{% endhighlight %}

If you poke around the *user* object you'll find quite a bit of information, a lot of it you may not need.

What about groups?

{% highlight ruby %}
  # Assuming we've binded 'conn' already in the example above ...

  results = conn.search(:filter => Net::LDAP::Filter.eq("cn", 'group_name'))

  raise "LDAP error. Code: #{conn.get_operation_result.code }, Reason: #{conn.get_operation_result.message}" unless conn.get_operation_result.code == 0

  raise "No groups found or too many groups found." unless results.length == 1

  group = results[0]

  # Print the group's name
  puts group[:name]

  # Print the group's members
  puts group[:member]

  # Count the group's members
  puts group[:member].length

{% endhighlight %}

It's pretty straightforward once you know the layout of your Active Directory trees and what each object looks like.

How about adding a user to a group?

{% highlight ruby %}
  # Assuming 'user' and 'group' are objects we already fetched from Active Directory ...

  result = conn.modify(:dn => group[:distinguishedname][0],
    :operations => [ [ :add, :member, user[:distinguishedname][0] ] ])

  if conn.get_operation_result.code == LDAP_ALREADY_EXISTS
    puts "User already in group."
  elsif conn.get_operation_result.code != 0
    raise "LDAP error. Code: #{conn.get_operation_result.code }, Reason: #{conn.get_operation_result.message}"
  else
    if result == true
      puts "User added to group."
    else
      puts "Something went wrong."
    end
  end

{% endhighlight %}

There exists an [active-directory gem](https://github.com/Mazwak/active_directory) to provide a more attractive and abstract experience when dealing with Active Directory if you prefer. At the time of this writing it does not support reading/writing users and groups from multiple, connected Active Directory servers so the above LDAP methods may prove useful to programmers in that scenario.
