# WORKING WITH D8 SITES IN GROUP SPACES #

## Purpose ##
Working with Drupal Sites with in Drupal can be a bit tricky. This tutorial will help you with installing your Drupal 8 site with composer in your group space.

## Environment Setup ##
Make sure you are able to SHH into your BYU group space. If you are on Windows, [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) is a good tool for this. 
Once you have access to your group space, then you are ready for the next steps. If you are having trouble with this step, talk to someone who knows more about this.

## Ensure PHP 7 or higher is installed ##
Drupal 8 with composer requires that you have php 7 or higher. Make sure you have PHP 7 installed on the server you are working with. I am SSH-ing into cbrgsl-05.et.byu.edu at the moment. Daniel set this up for me so we can have php 7 on a group space server. Only thing is that every time you open a new session on here, you will have to enable PHP. This can be done with the following command:

`scl enable rh-php70 bash`

The environment that I am working in took a long time to set up (Mostly figuring out what to do). There were a few things that we had to change to get it running correctly. As of when I wrote this, we are using the mentioned cbrgsl-05.et.byu.edu server. If you don't know about this, I would recommend talking to either Shawn or Daniel to figure more out about this. Basically, if you try to do this from another web group space server, there might be a few issues and it won't work properly. 

## Install composer ##
Once PHP is enabled, we are now going to install composer inside our groups folder. The reason for this is so we are able to use composer for any of our group spaces easier. 
Head over to [Composer's Website](https://getcomposer.org/download/) and follow the instructions to get composer installed. 
### NOTE ###
If you want to change the file name from *composer.phar* to *composer*, change the third command in the list from this:

`php composer-setup.php`

To this:

`php composer-setup.php --filename=composer`

Once installed, you should be able to run a command like this from anywhere to get composer to work:

`php ~/groups/composer`

### NOTE ###
Your path may be different and need to be changed

## Install Drupal 8 with composer ##
Now that composer is set up, lets install Drupal. 
Use this [Drupal.org Page](https://www.drupal.org/docs/develop/using-composer/using-composer-to-install-drupal-and-manage-dependencies#download-core) to follow along with for additional info. 


The directory you are going to install the project to is most likely going to be name www or something similar. This will be where your site will be hosted. The folder has to be completely empty before you can install Drupal. Once the folder is empty run this command to install Drupal:

`php ~/groups/composer create-project drupal-composer/drupal-project:8.x-dev install_location_dir --no-interaction`

Where install_location_dir will most likely be a folder named *www*.
This will take anywhere from 5 to 15 minutes in my experience. 

### Trouble Shooting  ###
It is very likely that this command caused some error for you. A likely problem is that you are missing some php packages. If you are able to install these packages your self, then go for it. If needed, refer to someone else with admin access to assist you in installing them. 

If you are running into any other major problems, you might have to do some research before finding an answer. 

## Install Drupal site on browser ##
This step should be pretty simple. Go to your web browser and open up your site URL. This will prompt you to setup your site. You might have to go to www.yourURL.com/web. This is because although you installed Drupal inside your www folder, the actual site itself resides within the web folder. There are a few errors that you can encounter. 
Sometimes the Web Server will have permission errors, and other common problems.

## Using Drush ## 
Drush is a tool to help us with common drupal tasks. Updating the database, clearing the cache, etc. 

To use Drush, navigate to your Drupal root. Drush it self is found under vendor/drush/drush and the executable is named drush.
### NOTE ###
I had problems getting Drush to work, if you are getting some errors about PHP MYSQL or something else, I would talk to Daniel as he helped me fix them. 

So to run drush use the following:

`php vendor/drush/drush/drush`

You can clear the cache with the following:

`php vendor/drush/drush/drush cr`

Update Database:

`php vendor/drush/drush/drush updatedb`

## Running Updates ##
To run an update, follow [this guide on drupal.org](https://www.drupal.org/docs/8/update/update-core-via-composer). 
Make sure you run the second command listed to run the update since we used drupal-composer/drupal-project to install drupal:

`composer update drupal/core webflo/drupal-core-require-dev --with-dependencies`

### Updating DB ###
It is recommended to update your database using drush. You can also do this by visiting the update.php page; however, we were having issues with this and required Daniel to do his magic once again. 

## Additional Setup/Info ##
* If you aren't seeing any style showing up on your site, then you will need to turn off aggregate CSS and JS files. It is in the configuration settings. 
* You might have to change the .htaccess in the web/sites/default/files and remove the following from Options: -ExecCGI -Includes -MultiViews
* This method of installing drupal hasn't been perfected yet and probably will need refinement. However, we understand that this setup will make updates and other tasks much easier. 
