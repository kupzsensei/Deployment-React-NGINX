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



