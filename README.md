# Why this patch ?

If you want to install Joomla by script, with no manual steps, this patch is for you !

# How does it work ?

This patch removes the web pages containing the install form (the one you need to manually fill in a browser).

And it allows you to provide the install options with a json file. Then you can trigger the install by fetching an URL (with wget, curl or something else). You can even run the install directly through PHP on the web server.

# Usage

Needless to say that you will need to adapt this to your real setup :-)

Let's say you have already created your database and your web site.

Let's say that, on the web server, the content of your site is located here : /var/www/my-joomla-site

Finally, let's say that you have downloaded Joomla 3.4.X and unpacked the installation package at the root of your site (/var/www/my-joomla-site)

Now Here we go:

1/ On the web server, download the patch and put it at the root of your site (/var/www/my-joomla-site)

2/ On the web server, you can now run the following commands
~~~
cd '/var/www/my-joomla-site'
patch -p1 < joomla_3.4.X_install_by_script.patch
cat > '/var/www/my-joomla-site/installation/custom_options.json' <<EOF
{
  "site_name":               "My Joomla Site",
  "site_metadesc":           "My Joomla Site - Installed by script",
  "admin_email":             "admin@myjoomlasite.com",
  "admin_user":              "admin",
  "admin_password":          "azerty",
  "admin_password2":         "azerty",
  "db_type":                 "mysqli",
  "db_host":                 "localhost",
  "db_name":                 "dbjoomla",
  "db_user":                 "dbuser",
  "db_pass":                 "azerty",
  "db_old":                  "remove",
  "db_prefix":               "mjs_",
  "language":                "en-GB",
  "site_offline":            "0",
  "sample_file":             "",
  "summary_email":           "0",
  "summary_email_passwords": "0"
}
EOF
~~~

3a/ Now from the web server or another machine, run the following command install Joomla
~~~
wget http://my.joomla.site.com
~~~

You can then parse the page grabbed by wget to see if the install is a success or not.

3a/ Another way: on the web server, you can run the following commands
~~~
cd /var/www/my-joomla-site/installation
REQUEST_URI='/installation/index.php' HTTP_HOST='my.joomla.site.com' php index.php
~~~

4/ Finally on the web server, you can remove the Joomla installation folder
~~~
rm -rf /var/www/my-joomla-site/installation
~~~

Joomla is now installed :-)

# Limitations

This patch has been tested with for Joomla 3.4.4, 3.4.5, 3.4.6 and 3.4.7 but it should work with all Joomla 3.X version.
Feel free to report your success or failure.
