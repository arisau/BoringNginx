# BoringNginx
Build script to build current stable Nginx with Google's BoringSSL instead of the default OpenSSL.

#### Currently Tested Working On:
* **Nginx 1.10.0** - Debian Jessie *(with Grsec/PaX)*, Debian Stretch *(with Grsec/PaX)*
* **Nginx 1.11.0** - Debian Jessie *(with Grsec/PaX)*, Debian Stretch *(with Grsec/PaX)*

### Enabling PHP
To enable PHP on this installation of nginx, it is as simple as installing the `php5-fpm` package and adding the regular PHP directives to your `/etc/nginx/nginx.conf` file. On Grsec/PaX kernels you do not need to set any MPROTECT excaptions on any binaries to get a fully working server with PHP support (I have now tested this).

To enable PHP, I add the following to my `nginx.conf` server block. The `try_files` directive ensures that Nginx does not forward bad requests to the PHP processor:
<pre>
	location ~ \.php$ {
		try_files $uri =404;
		fastcgi_split_path_info ^(.+\.php)(/.+)$;
		fastcgi_pass unix:/var/run/php5-fpm.sock;
		fastcgi_index index.php;
		fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
		include fastcgi_params;
	}
</pre>

You will also need to ensure that the `index` directive of your site is set up to serve `index.php` files.

### Enabling Passenger (for Ruby-on-Rails)
To enable Passenger in Nginx, you need to compile the Passenger module into Nginx. Passenger has a helpful script to do this for you (`passenger-install-nginx-module`), but you will need to pre-prepare your Nginx source to build against BoringSSL before running this tool. Fortunately, the devs have a helpful "advanced" option that you will be guided towards that allows you to feed the script a custom directory with Nginx source.

Install Ruby:
<pre>sudo apt install ruby ruby-dev</pre>

Install Rails:
<pre>sudo gem install rails</pre>

Install Passenger (tool for deploying Rails apps):
<pre>sudo gem install passenger</pre>

Run the Passenger version of the BoringNginx build script:
<pre>./build-debian-passenger.sh</pre>

Since building in this fashion bypasses Passenger's auto-compile script that automatically builds its module into Nginx for you, you will also miss out on some of the other things the script does.

If you attempt to run a Rails app and end up with the following in your Nginx `error.log`:
<pre>The PassengerAgent binary is not compiled. Please run this command to compile it: /var/lib/gems/2.1.0/gems/passenger-5.0.28/bin/passenger-config compile-agent</pre>

Then you can fix this by following the error's instructions and running something like this:
<pre>sudo /var/lib/gems/2.1.0/gems/passenger-5.0.28/bin/passenger-config compile-agent</pre>

To find out what configuration directives you need to set inside your `nginx.conf` file before Passenger will function, please see the [Nginx Config Reference](https://www.phusionpassenger.com/library/config/nginx/reference/) page on the Passenger site.
