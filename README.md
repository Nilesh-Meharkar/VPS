# Cloud server
1. Create a cloud server machine.
2. Create a user.
	> sudo adduser nilesh
3. Give sudo access to user.
	> sudo usermod -aG sudo nilesh


# Deploy django on apache.

	> sudo apt-get update
	> sudo apt-get install python3-pip apache2 libapache2-mod-wsgi-py3
	> sudo pip3 install virtualenv
>
	# copy django code in /var/www/workspace
	# create virtualenv


# Copy Code location
    > mkdir /var/www/workspace
    > /var/www/workspace


# Collect static
    > python manage.py collectstatic


# Apache
    > sudo vim  /etc/apache2/sites-available/000-default.conf 
* we can give any name to file.

> We can change port by changing <VirtualHost *:8000> 
>> Need to add `Listen` as well on top.
	Need to add on top.

	 Listen 8000
	 <VirtualHost *:8000>


##### Sample config for port 80

    <VirtualHost *:80>

        ServerAdmin nilshmeharkar43@gmail.com
        #DocumentRoot /var/www/workspace

        <Directory /var/www/workspace/vps/sample/sample>
                <Files wsgi.py>
                        Require all granted
                </Files>
        </Directory>

        WSGIDaemonProcess myproject python-home=/var/www/workspace/venv python-path=/var/www/workspace/vps/sample
        WSGIProcessGroup myproject
        WSGIScriptAlias / /var/www/workspace/vps/sample/sample/wsgi.py

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

    </VirtualHost>

> python-home -> Show environment location.

> python-path -> Show Project location.
wsgi file location.


## Apache commands 

1. Enable the site.

    > sudo a2ensite 000-default.conf

2. Disable the site.
	> sudo a2dissite 000-default.conf
	
3. Check apache error logs.
	> sudo tail -f 15 /var/log/apache2/error.log
	
4. Reload the server.
	> sudo systemctl reload apache2
	
	

# Extra	
## Run django app on gunicorn 
    > pip install gunicorn 
    > cd /var/www/workspace/vps/sample
    > gunicorn --bind 0.0.0.0:8500 sample.wsgi

## Ngnix

    > sudo apt install nginx
    > sudo ufw app list 
    > sudo ufw allow 'Nginx Full'
>
    > sudo vim /etc/nginx/sites-available/sample
    
    
##### Sample config for port 9000

    
    server {
            listen 9000;
            server_name 206.81.7.153;

            location = /favicon.ico { access_log off; log_not_found off; }
            location /static/ {
                    root /var/www/workspace/vps/sample;
            }

            location / {
                    include proxy_params;
                    proxy_pass http://206.81.7.153;
            }
    }


>

    > sudo ln -s /etc/nginx/sites-available/sample /etc/nginx/sites-enabled
    > sudo systemctl restart nginx
    > sudo systemctl reload nginx
