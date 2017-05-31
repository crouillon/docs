# Installation

Make sure you have basic knowledge of PHP5, Apache or Nginx and MySQL to install BackBee.

## Download and extract

This documentation showcases 2 different ways to launch the install.

### Install from ZIP archive

Download the [zip archive](http://backbee.com/backbee/backbee-latest.zip) and extract its contents into your webserver at a convenient location - web root folder or subfolder - for example `/backbee`. 

Then open your browser and navigate to the URL pointing to that specific folder. 

You will be automatically redirected to the install procedure described in the **Web Installer** section. 

> Need more details on those first steps ? We created a [video tutorial](https://www.youtube.com/watch?v=Xy6lFTmGSaA) specifically for WAMP users. 

### Install with command line (recommended)

First, you have to get the project by using Composer.

Open a command line interface and get BackBee Standard Edition:

    $ composer create-project "backbee/backbee-cms" /path/to/your/folder "~1.2" --no-interaction

From the `public` directory, launch the builtin server of PHP:

    $ cd /path/to/your/folder/public && php -S localhost:8000

Then browse to the URL [`http://localhost:8000/install.php`](http://localhost:8000/install.php) and follow the instructions detailed in the next section.

> Detailled steps for command line users are explained in this [short video](https://www.youtube.com/watch?v=Fe6Jg6s_Quw).

## Web installer

### Step 1 - Requirements checks

To see the first installation step of BackBee, go to [http://blogbee.dev:8000/install.php](http://blogbee.dev:8000/install.php) with your favorite web browser:

![BackBee Installer - first step](http://i.imgur.com/xWpgPlP.png "BackBee Installer - first step")

The `cache/`, `log/`, `repository/Data/`, `public/` and `repository/Config` folders must be owned by the apache or nginx user. If this user is unknown then set the rights to `0777`. Refresh and modify your environment until everything is green so you can go to the **second step**.

### Step 2 - General application configuration

![BackBee Installer - step 2](http://i.imgur.com/pvaDJIH.png "BackBee Installer - step 2")

If required, set folder rights to be `0777` for `repository/Config` and then go to the **third** step.

### Step 3 - Database and super admin configurations

BackBee Installer automatically creates and populates the database. The only thing you have to do is to set your database settings. Note that we highly recommend to use MySQL or MariaDB.

![BackBee Installer - step 3](http://i.imgur.com/f6ejuwI.png "BackBee Installer - step 3")

You also want to define your super admin user, fill the credentials then save and go forward to the **fourth** step.

![BackBee Installer - step 3 bis](http://i.imgur.com/ZENfnSS.png "BackBee Installer - step 3 bis")

### Step 4 - Site configuration

As last step, you have to choose your **site name** and fill its URL.

![BackBee Installer - step 4](http://i.imgur.com/fSlenGX.png "BackBee Installer - step 4")

At the end of this step, BackBee will create an empty file named `INSTALL_OK` in the `public/` folder. As long as this file exists the installation cannot start. So if for any reason you want to re-run the installation process, you have to remove this file first.

### Apache2 and Nginx virtualhosts configurations

Installation is now completed. BackBee generates and provides the `Apache2` or `Nginx` virtualhost configuration. To get the best experience of BackBee CMS, we recommend you to setup the one that match your environment.

![BackBee Installer - step 5](http://i.imgur.com/T13tVjT.png "BackBee Installer - step 5")

![BackBee Installer - step 5 bis](http://i.imgur.com/ePKuLX5.png "BackBee Installer - step 5 bis")

When your web server is set up, you can start using BackBee with the URL **[http://blogbee.dev](http://blogbee.dev)**.

If everything is correctly installed, you should see the BackBee welcome message:

![BackBee welcome message](http://i.imgur.com/LIKWK1D.png "BackBee welcome message")

> *If you can't see BackBee welcome message, make sure that `cache` and `log` folders have the correct rights. (755 or 777)*

You can switch into edition mode by pressing simultaneously `CTRL + ALT + B`. The expected credentials are these you setted up at step 4.

![BackBee Installed - Edition mode](http://i.imgur.com/8kxLbfx.png "BackBee Installed - Edition mode")

## Category and article fixtures

If you want to load some fixtures, you can run this command from your project root directory:

    $ ./backbee fake:data:generate --article-limit 20 --category-limit 5

You can choose the number of articles and categories to generate by respectively changing `20` and `5`.

## Ubuntu and PHP's sessions

If your operating system is Ubuntu, you might meet the following error while PHP is trying to write its sessions:

```
SessionHandler::read(): open(/var/lib/php5/sessions/sess_ne8pqe6qshr0hc6sn9nrmhpfc5, O_RDWR) failed: Permission denied (13) in /var/www/html/bb/vendor/symfony/http-foundation/Symfony/Component/HttpFoundation/Session/Storage/Proxy/SessionHandlerProxy.php on line 69
```

This is due to the user which executes PHP. He has not the right to write in the session's storage folder. You can solve this by using the command `chmod` or changing the session storage folder.

You can also take a look at [puphpet/puphpet#1195](https://github.com/puphpet/puphpet/issues/1195) to learn more about this issue.
