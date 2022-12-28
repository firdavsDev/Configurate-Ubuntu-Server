# Configuration Ubuntu Servers | Django 
<br>

  ### We need to open up our firewall to normal traffic on port 80
  *     sudo ufw allow 'Nginx Full'
  ### Create gunicorn service file & start
  *     sudo nano etc/systemd/system/{name}.service
  * Wrire code in the etc/systemd/system/{name_file}.service:
        
        [Unit]
        Description={some info about project}
        After=multi-user.target
        [Service]
        Type=simple
        Restart=on-failure
        WorkingDirectory=/var/www/{project_path}
        ExecStart=/var/www/{project_path}/{venv}/bin/gunicorn -b 0.0.0.0:8001 {file}.wsgi
        [Install]
        WantedBy=multi-user.target

  * venv | `pip install gunicorn`
  *     sudo systemctl enable {name}.service
  *     sudo systemctl start {name}.service
  *     sudo systemctl status {name}.service
  ### Create nginx file & start
  *     sudo nano etc/nginx/sites-available/{name_file}
  * Wrire code in the etc/nginx/sites-available/{name_file}
     
        server {
     
           server_name {ip_addres};
           listen 80;
           client_max_body_size 20M;

           location /static/ {
               root /var/www/{project_path}/;
           }

           location /media/ {
               root /var/www/{project_path}/;
           }

           location / {
               include         proxy_params;
               proxy_pass      http://localhost:8001;
           }
        }
     
   *     sudo ln -s /etc/nginx/sites-available/{name} /etc/nginx/sites-enabled
   *     sudo systemctl restart nginx.service

### If you change gunicorn systemd service file, reload the daemon and restart the process by typing:

 *     sudo systemctl daemon-reload
 *     sudo systemctl restart gunicorn
 

### If you change the Nginx server block configuration, test the configuration and then Nginx by typing:

 *     sudo nginx -t && sudo systemctl restart nginx

### Checking Logs

 *     sudo journalctl -u gunicorn
 *     sudo systemctl status gunicorn
 *     sudo nginx -t

### Nginx Is Displaying a 502 Bad Gateway Error Instead of the Django Application

  *     sudo tail -F /var/log/nginx/error.log

More: https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-16-04
