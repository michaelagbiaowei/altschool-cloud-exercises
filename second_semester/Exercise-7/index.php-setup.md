**Login in as root**

MUST BE DONE ON SLAVE

Prerequisites: php7.4

**Step 1: Make a Directory for your Site**

You’ll create a directory for your site that you’ll be hosting, within the /var/www folder. This location newly created location is also dubbed the document root location; you’ll need to set this path later in the configuration file. Sub the phpdomain.com for your domain name.

$ mkdir -p /var/www/phpdomain.com/public_html

**Step 2: Set Folder Permissions**

$ chmod -R 755 /var/www

**Step 3: Set up an Index Page**

To see a home page you’ll want to make sure the index.php file is created for your domain. Something as simple as "The e.g given on LMS" can be set within this file.

$ vim /var/www/phpdomain.com/public_html/index.php

Save and quit by hitting the Escape button and typing :wq

**Step 4: Copy the Config File for your Site**

Copy the default configuration file for your site, this will also ensure that you always have a default copy for future site creation.

$ cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/phpdomain.com.conf

**Step 5: Edit the Config File for your Site**

At the bare minimum, you’ll adjust and add the highlighted lines within the <VirtualHost \*:80> and </VirtualHost> tags.

**Note**::
ServerAlias is the alternative name for your domain, in this case and in most, you’ll put www in front of the domain name so people can view the site by either www or non www (ServerName).

$ vim /etc/apache2/sites-available/phpdomain.com.conf

_edith as followed_;

=====================================

<VirtualHost \*:80>

ServerAdmin admin@example.com

ServerName phpdomain.com

ServerAlias www.phpdomain.com

DocumentRoot /var/www/phpdomain.com/public_html

ErrorLog ${APACHE_LOG_DIR}/error.log

CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost \*:80>

===================================

Quit and Save with :wq.

**Step 6: Enable Your Config File**

Out of the box, your server is set to read the default 000-default.conf file. But, in our previous step we made a new config file for your domain. So, we will need to disable the default file.

$ a2dissite 000-default.conf

$ a2ensite phpdomain.com.conf

We restart the Apache service to register our changes.

$ systemctl restart apache2

**Step 7: Verify Apache Configurations**

After starting Apache you now can view that the configurations are working by either editing your /etc/host file on your computer or by editing your domain's DNS.

After either one of these aspects are set, you'll be able to visit your website in a browser to see the index.php pages set in **Step 3**.

## Edited from...
