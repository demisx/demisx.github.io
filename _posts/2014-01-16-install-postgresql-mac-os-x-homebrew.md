---
layout: post
title:  "How to Install PostgreSQL 9 DB on Mac OS X Mavericks with Homebrew"
date:   2014-01-16 17:52:12
categories: ruby-on-rails
---

Just moved from MySQL to PostgreSQL for the new rails apps. I think it is time I try
something new after being so faithful to MySQL DB for the past X years. My first PostgreSQL
install didn't go as smooth as I expected, but wasn't too bad. Here is a summary of the installation
steps that worked for me (assuming you are using the awesome [Homebrew](http://brew.sh/) package
manager).

<!--more-->

### Uninstall Previous Version of PostgreSQL (if any)
If you'd like to do a fresh re-install of PostgreSQL DB existing version can be uninstalled this way.
<div class="alert alert-danger"><i class="fa fa-exclamation-circle fa-2x"></i> Make sure to export all of your data from the existing database prior
to executing steps below or it will be deleted!</div>

{% highlight console %}
# Stop current DB server if running
[dmoore]$ launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist

# Ensure there are no PostgreSQL processes running. Quit whatever is running
[dmoore]$ ps -ef | grep postgres | grep -v "grep postgres"
#501 39289     1   0  8:47PM ??         0:00.01 postgres: dmoore beotracker_development [local] idle
[dmoore]$ kill -QUIT 39289

# Remove current package
[dmoore]$ brew rm postgresql --force

# These files may need to be deleted manually, if present
[dmoore]$ rm -fr /usr/local/var/postgres
[dmoore]$ rm ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
[dmoore]$ rm /Library/Caches/Homebrew/Formula/postgresql.brewing
{% endhighlight %}

### Install PostgreSQL DB with Homebrew

{% highlight console %}
# ensure there are no errors with your existing packages
[dmoore]$ brew doctor

# update latest formulaes
[dmoore]$ brew update

# Install DB server
[dmoore]$ brew install postgresql

# Enable server to auto start at boot
# (Make sure to update `9.3.2` in the path below to match your PostgreSQL version, if different)
[dmoore]$ cp /usr/local/Cellar/postgresql/9.3.2/homebrew.mxcl.postgresql.plist ~/Library/LaunchAgents/
{% endhighlight %}

At this point the PostgreSQL DB server is installed and ready to start. Config file is located
at `/usr/local/var/postgres/postgresql.conf`

{% highlight console %}
# Start server
[dmoore]$ launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist

# Stop server
[dmoore]$ launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
{% endhighlight %}

### Managing PostgreSQL with Lunchy Gem

I recommend installing this gem to simplify starting and stopping PostgreSQL server:

{% highlight console %}
[dmoore]$ gem install lunchy

# Start server
[dmoore]$ lunchy start postgres

# Stop server
[dmoore]$ lunchy stop postgres
{% endhighlight %}

### Testing PostgreSQL Installation

Make sure the server is running, then execute commands below. You should see:

1. `which` returning Homebrew's version of `psql` file located at `/usr/local/bin/psql`; and
2. `\l` reporting `postgres` db with user (or "role" as postgres calls it) being set to your current
shell user (e.g. "dmoore" in my case).

{% highlight console %}
[dmoore]$ which psql
# /usr/local/bin/psql

[dmoore]$ psql postgres
# psql (9.3.2)
# Type "help" for help.

postgres=# \l
                               List of databases
   Name    | Owner  | Encoding |   Collate   |    Ctype    | Access privileges
-----------+--------+----------+-------------+-------------+-------------------
 postgres  | dmoore | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 template0 | dmoore | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/dmoore        +
           |        |          |             |             | dmoore=CTc/dmoore
 template1 | dmoore | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/dmoore        +
           |        |          |             |             | dmoore=CTc/dmoore

{% endhighlight %}

Hopefully, you see what I see.

___
##### Today my environment was:

- Mac OS X Mavericks 10.9.1
- Homebrew 0.9.5
- PostgreSQL 9.2.3e
