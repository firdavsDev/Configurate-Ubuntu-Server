# Deploy django project to Digital Ocean

  * nano etc/systemd/system/{name}.service
    In tc/systemd/system/{name}.service file write 
    " 
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
    "
   
  * pip install gunicorn
  * sudo systemctl enable {name}.service
  * sudo systemctl start {name}.service
  * sudo systemctl status {name}.service
  * nano etc/nginx/site-aviable   {name}
  "
      server {
        server_name {ip_addres};
        listen 8000;
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
    "
    * sudo ln -s /etc/nginx/sites-available/{name} /etc/nginx/sites-enabled
    * sudo systemctl restart nginx.service
