CloudBackupBundle
=================

This bundle helps you to backup your databases and upload it to the cloud with only one symfony2 command.

You can :
* Dump one database
* Dump all databases
* Different types of databases can be dumped each time
* Upload to several Cloud services

Databases supported
* MongoDB
* MySQL (soon..)

Cloud service supported
* Dropbox (with the help of [DropboxUploader by hakre](https://github.com/hakre/DropboxUploader))
* CloudApp (soon..)
* Google Drive (soon..)



Installation
------------

### Composer

Download CloudBackupBundle and its dependencies to the vendor directory. You can use Composer for the automated process:

```bash
$ php composer.phar require dizda/cloud-backup-bundle dev-master
```

Composer will install the bundle to `vendor/dizda` directory.

### Adding bundle to your application kernel

```php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = array(
        // ...
        new Dizda\CloudBackupBundle\DizdaCloudBackupBundle(),
        // ...
    );
}
```

Configuration
-------------

Here is the default configuration for the bundle:

```yml
dizda_cloud_backup:
    cloud_storages:
        # Dropbox account credentials (use parameters in config.yml and store real values in prameters.yml)
        dropbox:
            user:     ~ # Required
            password: ~ # Required

    databases:
        mongodb:
            all_databases: false # Only required when no database is set
            database:     ~ # Required if all_databases if false
            db_user:     ~ # Not required, leave empty if no auth is required
            db_password: ~ # Not required
```

It is recommended to keep real values for logins and passwords in your parameters.yml file, e.g.:

```yml
# app/config/config.yml
dizda_cloud_backup:
    cloud_storages:
        dropbox:
            user:     %dizda_cloud_dropbox_user%
            password: %dizda_cloud_dropbox_password%

    databases:
        mongodb:
            all_databases: false
            database: %dizda_cloud_mongodb_user%
            db_user:  %dizda_cloud_mongodb_user%
            db_pass:  %dizda_cloud_mongodb_password%
```

```yml
# app/config/parameters.yml
	# ...
    database_driver: pdo_mysql
    database_host: localhost
    database_port: null
    database_name: myDatabase
    database_user: myLogin
    database_password: myDatabasePassword
    # ...
    dizda_cloud_dropbox_user:     myDropboxUser
    dizda_cloud_dropbox_password: MyDropboxPassword
    dizda_cloud_mongodb_user:     mongodbUser
    dizda_cloud_mongodb_password: mongodbPass
	# ...
```


Usage
-----

The bundle adds one command to symfony console: ``app/console dizda:backup:start`` which you execute periodically as a cron job.
For example the following cron command dumps your database every days at 6am on a server :
```
# m h  dom mon dow   command
0 6 * * * php /opt/www/symfony-project/app/console dizda:backup:start
```

Info : To edit crontab for the user www-data (to prevent permissions error) :
```bash
$ crontab -u www-data -e
```

End
---
Enjoy, PR are welcome !