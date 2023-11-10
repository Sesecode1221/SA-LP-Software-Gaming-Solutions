# SA-LP-Software-Gaming-Solutions
A gaming Software Solutions by LP &amp; SA Software Gaming Solutions
SA-LP-Software-Crypto Casino — Documentation
System requirements
PHP 7.1.3 or higher.
PHP extensions: cURL, Mbstring, OpenSSL, PDO, Tokenizer, XML, Ctype, JSON, BCMath
URL rewrite enabled.
The application should be installed to the web root folder of a domain or sub domain (it will not work if installed to a sub folder).
Installation
Step 1
Download the application package from CodeCanyon, unzip it and upload the contents to the web root folder of your domain (on shared hosting environments the web root folder is usually named public_html).

Note for VPS or dedicated server users
If you install the application on a VPS or dedicated server please ensure that the web server (usually www-data user) has write permissions to the following folders:

.. (web root)
bootstrap/cache
public
storage
storage/app/public/games
storage/framework
storage/framework/cache
storage/framework/sessions
storage/framework/views
storage/logs
The easiest way to ensure proper permission is to make www-data user owner of the above folders (and files recursively). It can be done with the following shell commands:

 

>_ cd /var/www/myproject
>_ chown www-data public_html
>_ cd public_html
>_ chown www-data public
>_ chown -R www-data storage bootstrap/cache
Please also check that URL rewrite is enabled and custom .htaccess files are allowed (AllowOverride directive set to All) in the Apache web server configuration. You can check the latter by inspecting your virtual host config file (e.g. /etc/apache2/sites-enabled/yourwebsite.conf) or the main Apache config (/etc/apache2/apache2.conf). It should contain a block similar to this:

 

<Directory /var/www/myproject/public_html/>
    Options FollowSymLinks MultiViews
    AllowOverride All
    Require all granted
    Order allow,deny
    allow from all
</Directory>
Please note that you need to restart the Apache service after making configuration changes.

Step 2
Create a MySQL (or MariaDB) database on your server, as well as a database user who has all privileges for accessing and modifying it.

Step 3
Run the application installation script by accessing the following URL in a web browser: http://yourwebsite.com/install.php (substitute yourwebsite.com with your actual domain name). Follow the on-screen setup instructions to complete the installation.

After installation
Please make sure to add a system cron job in order to automatically execute scheduled tasks (such as bots). The system cron job parameters are provided on the last step of installation and on the Maintenance page in the backend.

Frequently Asked Questions
Why do I get "Not found" error when installing the app?
It happens when necessary .htaccess files are either missing (there should be 2 .htaccess files - one in the application root folder and another in public sub folder) or not allowed by the Apache web server.

After installation Slots game does not load and stuck at X%.
Please create a symbolic link (symlink) from public/storage to storage/app/public folder. Normally, it should be created automatically during installation, but on some hosts automatic creation is disabled, in which case the symlink should be added manually.

If you have SSH access you can create the symlink by navigating to the web root folder of your domain and running the following command:

>_ php artisan storage:link
If you do not have SSH access please ask your hosting support to create the symlink for you.

After enabling SSL (HTTPS) games are not loading.
The following error is displayed in the browser console: Mixed content: The page at ... was loaded over HTTPS, but requested an insecure resource.

If you are using Cloudflare please check that Automatic HTTPS Rewrites are enabled in the Cloudflare settings. Please also check that you added a wildcard redirect from HTTP to HTTPS version of your website. If this still doesn't help please edit .env file and add the following line:

FORCE_SSL=1
I get 419 error when accessing backend.
Try to add the following line to .env file:

SESSION_DOMAIN=yourdomain.com
Clear cache on the maintenance page after that.
Why does user always win in Slots?
All games results are completely random, you can't manually set the win rate or otherwise affect the roll process. However, you can adjust the Slots game settings to make it more difficult for a user to win (or to be more precise - make it more difficult to have the positive net result, because winning 5 on a 20 credits bet is not actually a win). You can:

Increase the min bet size
Decrease the payout amounts
Disable scatter and / or wild symbols
Increase the min number of symbols that can win (e.g. only 4 or 5 symbols on a line win)
How to enable chat?
Chat uses Pusher to function. In order to enable chat functionality you need to create a free account at pusher.com, create a new channels app and obtain its credentials (such as app ID, key, secret and cluster). After that input these credentials in the application settings in the backend (under Integration tab). The chat should then start working automatically.

How to enable social login?
To enable authentication (using OAuth) with one of the supported social providers (Facebook, Twitter, Google, LinkedIn, Yahoo, Coinbase, Steem) complete the following steps.

Log in to a developers platform (for instance, Facebook for developers is located at developers.facebook.com).
Create a new web application and fill in necessary details (such as name, URL of your website etc).
Specify Callback URL (can also be called Redirect URL) as https://YOURWEBSITE.COM/login/PROVIDER/callback (example: https://mycasino.com/login/facebook/callback). For your convenience Callback URLs for each provider are displayed under Integration tab on the Settings page.
Obtain Client ID and Client secret (they can also be called App ID and App secret, Consumer key and Consumer secret and alike).
Input Client ID and Client secret on the Settings page (Integration tab).
Here is how the app settings page looks like on Facebook for developers.

Things to note:

Social login will not work on localhost
Your website should have an SSL certificate installed and enabled
How to change the website logo?
To override the default website logo create an image file named logo-udf.png and put it to public/images folder. This way you will be able to keep your custom logo during future updates.

How to change the OG image?
The OG image is used when sharing website content on social media. To override the default OG image create an image file named og-image-udf.jpg and put it to public/images folder. This way you will be able to keep your custom OG image during future updates.

How to change the favicon?
The website favicon files are located in public/images/favicon folder. You can just overwrite the original files with your custom ones.

How to change translations?
You can change translation strings for any language by editing JSON translation files in resources/lang folder.

How to change default text strings in English?
To change the default text strings it's not necessary to modify the application code. All you need to do is to create a JSON translation file called en.json and put it in resources/lang folder. The file can be created as a copy from any other translation file, such as resources/lang/de.json for instance. In this translation file you need to keep only those text strings that you would like to override. The left side represents the original text string in English and the right side represents the overridden string. Example:

{
    "Crypto Casino": "My custom casino name"
}
Where should I add custom CSS styles to?
If you like to apply some custom CSS styles create a file called style-udf.css inside public/css folder and put your styles there. This will help you to preserve your custom styles after upgrading the app to a new version.

How to override header, footer and front page templates?
Sometimes you might want to override default header, footer and front page templates. It's better than just changing the template files directly, because in this case you will not lose your customizations when upgrading the application.

To override these templates create the following files and edit them the way you like:

Header: resources/views/frontend/includes/header-udf.blade.php
Footer: resources/views/frontend/includes/footer-udf.blade.php
Front page: resources/views/frontend/layouts/home-udf.blade.php
How to add a custom static text page?
Suppose you want to add a new static text page, which is available at http://yourwebsite.com/page/my-text-page. To do so create a new template file named my-text-page.blade.php in resources/views/frontend/pages/static folder. You can copy the default page structure from other static pages templates in this folder and then modify it as needed. After the template is created the static page will automatically become available.

I enabled the maintenance mode and accidentally logged out. How to disable it?
To disable the maintenance mode when you don't have access to the backend delete the following file: storage/framework/down.

How to upgrade the app to a new version?
Back up your files and database.
Log in to the backend, navigate to the Maintenance page and enable maintenance mode.
Download a new version of the app from CodeCanyon, unzip and copy all files to your server (overwrite all files).
On the maintenance page click Run database updates and then Clear cache button.
Disable maintenance mode (additionally you might also need to clear cache in your browser).
The application settings will be preserved, but please note that if you changed any of the application files you will have to re-apply customizations manually.

Extra add-ons
Payments (deposits & withdrawals)
Multi-slots
Dice
Dice 3D
Heads or Tails
Lucky Wheel
Blackjack
Casino Holdem
Baccarat
American Roulette
European Roulette
Video Poker
Keno
75 Ball Bingo
Raffle
Credits
PHP framework: Laravel 5.7
Front-end framework: Bootstrap 4.3
How to get support?
Please note that support can only be provided during the application support period. 6 months of free support are provided with your purchase. Should you need support outside of this period you will need to renew it. How to extend / renew the app support?

To get technical support please submit a new ticket at https://support.financialplugins.com. If you see an application error please do the following before submitting a support request:

Enable Debug mode (Settings --> Developer)
Repeat the error to get the full error stack trace
Make a screenshot of the error
Provide the screenshot along with the app error log (you can download it on the Maintenance page)
Provide steps to reproduce the error
In case the issue is difficult to trace our support team can request SSH / cPanel access to your server and / or website admin access.

© Built by FinancialPlugins.com
