# Deployment-React-NGINX

1. `go to /var/www/`
   
2. **clone your project**
  ```bash
git clone github.url
```

3. **cd to your project**
   ```bash
   cd <project_name>
   ```
   
4. **build your project**
   ```bash
   npm run build
   ```

## Install Nginx (if you haven't already)

**On Ubuntu/Debian :**
```bash
sudo apt update
sudo apt install nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```

**On CentOS/RHEL:**
```bash
sudo yum install epel-release
sudo yum install nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```

## Nginx Configuration

```bash
sudo nano /etc/nginx/sites-available/your-react-app.conf
```

**sample nginx configuration**
```conf
server {
    listen 80;
    server_name your_domain.com www.your_domain.com; # Replace with your domain or IP

    root /var/www/your-react-app/dist; # Path to your React build directory
    index index.html index.htm;

    location / {
        try_files $uri $uri/ /index.html;
    }

    access_log /var/log/nginx/your-react-app.access.log;
    error_log /var/log/nginx/your-react-app.error.log;

    # Optional: Enable Gzip compression for faster loading
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
    gzip_proxied any;
    gzip_comp_level 5;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
    gzip_disable "MSIE [1-6]\.(?!.*SV1)";
    gzip_vary on;
}
```
proxy_pass
```conf
server {
    listen 80;
    server_name your_domain.com www.your_domain.com; # Replace with your domain or IP

    # Specifies the root directory for NGINX to serve files from.
    # This is often not needed with a proxy, but can be used for static files.
    # root /var/www/your-react-app/dist; 
    # index index.html index.htm; 

    location / {
        # This is the key directive that proxies requests to your React container.
        # Replace 'react-app-container' with the name of your React service
        # or container name in your docker-compose file.
        # Replace '3000' with the port your React app is listening on.
        proxy_pass http://react-app-container:3000;
        
        # Recommended proxy headers for passing information like the host and real IP.
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    access_log /var/log/nginx/react-app.access.log;
    error_log /var/log/nginx/react-app.error.log;

    # Optional: Enable Gzip compression
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
    gzip_proxied any;
    gzip_comp_level 5;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
    gzip_disable "MSIE [1-6]\.(?!.*SV1)";
    gzip_vary on;
}
```

## enable the NGINX Configuration
```bash
sudo ln -s /etc/nginx/sites-available/your-react-app.conf /etc/nginx/sites-enabled/
```

## Test and Restart NGINX
```bash
sudo nginx -t
```

**restart nginx**
```bash
sudo systemctl restart nginx
```

## Set File Permissions 
```bash
sudo chown -R www-data:www-data /var/www/your-react-app/html
sudo chmod -R 755 /var/www/your-react-app/html
```



