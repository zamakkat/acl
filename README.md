# ACL Anthology#
Basic instructions on running the ACL anthology. Note: If you are setting up the server and/or working on the server, please set the environment variable RAILS_ENV=production. One way is, each time you ssh to the server, run this once:
```
$ export RAILS_ENV=production
```

## Installation ##
These are the main steps to getting the ACL anthology running on your local machine. The whole process should take at least an hour or so, so be prepared. This is best done on any Linux based OS (MacOS, Ubuntu, Linux...) for the convience of tools used.

### Prerequisites ###
The installation of this rails app assumes that you have a running Ruby on Rails installation with the following versions of core services:
```
  * Rails (4.0.1)
  * Ruby (2.0.0)
  * Bundle (1.3.5)
  * PostgreSQL (9.3.1)
  * Java (1.5 or higher)
```
If you don't have a running copy, we recommend using RVM to install Rails and all its dependencies. Also, git command line tools or GUI is needed to clone the repository.

### Cloning ###
Browse to your designated folder and clone it using this git command (or with a git GUI tool). The process should take a while since the repository is quite big (about 150MB):
```
$ git clone https://github.com/zamakkat/acl
```
After the cloning is done, go to the ACL directory and install all gems:
```
$ cd acl
$ bundle install
```

### Database ###
Run the following commands to initialize the database and run migrations:
```
$ rake db:create
$ rake db:migrate
```
Ingest the database with the ACL anthology information using this command:
```
$ rake db:seed
```
This command requires Internet connectivity as it gets the data directly from the ACL website. To fully ingest the data, it will take a very long time, at least 30 mins. So you can go grab a coffie or a meal or something :) 

If there is any error with the seeding process, most probably there is a problem with some of the xml files. Go to db/seeds.rb, ignore those files for the time being and inform the ACL editor to edit those xml files. In this case, or if for any reason you want to recreate and reseed the database, please use the drop command:
```
$ rake db:drop
```
After that you can start over and recreate the database.

This is only one of the two methods to create the database. You can [click here](https://github.com/zamakkat/acl/wiki/Seeding-1:-Initial-database-seeding) to learn more about it or alternatively, you can use the second method: [Seeding 2: Individual elements](https://github.com/zamakkat/acl/wiki/Seeding-2:-Individual-elements)

### Indexing ###
Before using the search functionality, we will need to run the Solr server locally and index the data. First, we will need to set the user. Open `jetty/solr/blacklight-core/conf/data-config.xml` (or just use find to find this file) and change line 5 to your current user account:
```
user="user_account"
password=""
```
Then in the `jetty/solr/blacklight-core/conf/solrconfig.xml`, edit line 67 (it is for server only):
```
<str name="config">/var/opt/solr/solr/blacklight-core/conf/data-config.xml</str>
Change to:
<str name="config">data-config.xml</str>
```
After saving all files, we can start the Solr server:
```
$ cd jetty; java -jar start.jar &
```
Open a new terminal window and index the data:
```
$ rake acl:reindex_solr 
```
You can check the indexing process by going here (this is for the production server, and you have to be connected to SoC network):
```
http://acl-anthology-i.comp.nus.edu.sg:8983/solr/#/blacklight-core/dataimport//dataimport
```
At this point, your setup is completed and you can run the rails server to test the app:
```
$ rails server
```
You can go to the ACL rails app by going to http://localhost:3000/ in you browser.

## Exporting data ##

For the export functions to work properly, we must first run some rake tasks to pre-generate the exported files. [Click here](https://github.com/zamakkat/acl/wiki/Exporting) to read more about exporting.

## License ##
ACL materials are Copyright (C) 1963-2013 ACL; other materials are copyrighted by their respective copyright holders. All materials here are licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 3.0 License . Permission is granted to make copies for the purposes of teaching and research.

Credits: The institutions and individuals behind the ACL Anthology.

Min-Yen Kan (Editor, 2008-) / Steven Bird (Editor, 2001-2007) 
