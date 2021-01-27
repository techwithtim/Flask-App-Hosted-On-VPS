# Flask-App-Hosted-On-VPS
Files needed to host a flask application on a linux VPS.

## Commands

Run the following commands on the VPS.

### Install Python and Pip

```bash
sudo apt-get install python3
sudo apt-get install python3-pip
pip3 install flask
```

### Install and Configure NGINX

Install nginx and create a new configuration file.
```bash
sudo apt install nginx 
sudo nano /etc/nginx/sites-enabled/<directory-name-of-flask-app>
```

The contents of the confiugration file should be as follows:
```bash
server {
    listen 80;
    server_name <public-server-ip>;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

Unlink the default config file and reload nginx to use the newly created config file.
```bash
sudo unlink /etc/nginx/sites-enabled/default
sudo nginx -s reload
```

### Installing and Using Gunicorn
```bash
sudo apt-get install gunicorn
```

Run the flask web app with gunicorn. The name of your flask instance must be ```app```.
```bash
gunicorn -w 3 flask_app:app
```
Now navigate to your servers public ip from a web browser! :)

### Credits
This information was summarized from [Linode's Guide](https://www.linode.com/docs/guides/flask-and-gunicorn-on-ubuntu/)
