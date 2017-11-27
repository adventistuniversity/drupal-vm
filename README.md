# Drupal VM for ADU Pantheon sites

## Setup

1. Install [Vagrant](https://www.vagrantup.com/downloads.html) and [VirtualBox 5.1.26](http://download.virtualbox.org/virtualbox/5.1.26/) (newer versions do not yet work with folder sync)
1. Download or clone this repo
1. Copy `default.config.yml` to `config.yml` and make the following modifications:
    * You can set the `vagrant_hostname`, `vagrant_machine_name`, and `vagrant_ip` to whatever you like, which is useful if you are making multiple boxes for multiple sites
    * Point `local_path` to your local copy of a Pantheon site repo
    * You can change `destination` if you want, it's the path that will be on the Vagrant box
    * Set `drupal_core_path` to the same value you set `destination` above
    * Change `php_version` to match the PHP version of the Pantheon site you're working on (most likely `5.6`)
1. On the command line, `cd` into this project and run `vagrant up`
1. Make a copy of the database on the Vagrant box:
    * Run `vagrant ssh`
        * On Windows, this requires that you can run ssh from the command line - if this fails, try installing [Chocolatey](https://chocolatey.org/) and running `choco install git -params "/GitAndUnixToolsOnPath"` to quickly get ssh working
    * [Authenticate with Terminus](https://pantheon.io/docs/terminus/install/#authenticate)
    * Create a new backup if needed: `terminus backup:create` _[site]`.`[environment]_ `--element=db`
    * Download the database backup: `curl -o mydb.sql.gz "$(terminus backup:get` _[site]`.`[environment]_ `--element=db)"`
    * Create a database in MySQL
    * Unzip the database backup into the database you just created: `gunzip < mydb.sql.gz | mysql -uroot -proot` _[database name]_
    * Clean up the database backup files
1. If you're not using the vagrant-hostsupdater or vagrant-hostmanager plugins, go to the `vagrant_ip` in your browser (192.168.88.88 by default) and use the directions in the upper right corner to add the right info to your hosts file (`C:\Windows\System32\drivers\etc\hosts` on Windows - must open editor as admin)
1. Go to the `vagrant_hostname` in your browser to see the site! :sunglasses::+1:

## Usage

Work on the files in your local copy of the Pantheon repo as normal and view your changes at `vagrant_hostname` in your browser. 

Read about the [different ways to stop the Vagrant box from running](https://www.vagrantup.com/intro/getting-started/teardown.html) when you want to shut it down. Then simply go to the folder for this project again and run `vagrant up` to start it up again. If you ran `vagrant destroy`, you will need to create the database again.
