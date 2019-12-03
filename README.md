# Introduction
This Koha plugin enables a library to accept online payments from patrons using the DIBS payments platform.
See https://tech.dibspayment.com/D2

# Installing
This plugin needs the following perl modules:
* Locale::Currency::Format (liblocale-currency-format-perl)
* Digest::MD5 (libdigest-md5-perl)

To set up the Koha plugin system you must first make some changes to your install.

* Change `<enable_plugins>0<enable_plugins>` to `<enable_plugins>1</enable_plugins>` in your koha-conf.xml file
* Confirm that the path to `<pluginsdir>` exists, is correct, and is writable by the web server
* Add the pluginsdir to your apache PERL5LIB paths and koha-plack startup scripts PERL5LIB
* Restart your webserver

Once set up is complete you will need to alter your UseKohaPlugins system preference. On the Tools page you will see the Tools Plugins and on the Reports page you will see the Reports Plugins.

# Apache setup

You will need to add to the apache config for your site:
```
   Alias /plugin/ "/var/lib/koha/kohadev/plugins/"
   # The stanza below is needed for Apache 2.4+
   <Directory /var/lib/koha/kohadev/plugins/>
         Options Indexes FollowSymLinks
         AllowOverride None
         Require all granted
         Options +ExecCGI
         AddHandler cgi-script .pl
    </Directory>
```

# DIBS configuration

* Contact DIBS to create an account, the plug-in is developed to work with the D2 platform. https://www.dibs.se/. For testing, create a D2 test account. https://www.dibspayment.com/demo-signup
* Login to the admin interface. https://payment.architrade.com/login/login.action
* Enable Order ID and Transaction status code in Integration => Return values.
* Decide on how and when to debit. The Plug-in is verified to work with the batch capture method. Set time to debit in Setup => Closing hour.
* Create MD5 keys in Integration => MD5 Keys

# Plugin configuration

* Make sure that Koha's OPACBaseURL system preference is correctly set
* Report your DIBS Merchant ID and MD5 Keys in the plugin configuration page
