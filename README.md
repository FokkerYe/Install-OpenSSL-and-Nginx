# Install-OpenSSL-and-Nginx

### Step-by-Step Guide to Creating a Self-Signed SSL Certificate and Configuring Nginx
### Step 1: Install OpenSSL and Nginx
First, ensure that OpenSSL and Nginx are installed on your system.
```
sudo apt update
sudo apt install openssl nginx -y

```
### Step 2: Generate a Self-Signed SSL Certificate

Generate the SSL certificate and key with the following command:
```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/test.key -out /etc/nginx/ssl/test.crt -batch

```
### Step 3: Configure Nginx to Use the SSL Certificate

Edit the Nginx default configuration file to enable SSL.
```
sudo nano /etc/nginx/sites-available/default
```
Modify the file to include the SSL configuration. Ensure you have the following lines within the server block:
```
server {
    listen 443 ssl default_server;
    listen [::]:443 ssl default_server;

    ssl_certificate /etc/nginx/ssl/test.crt;
    ssl_certificate_key /etc/nginx/ssl/test.key;

    # Your additional configuration
    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        try_files $uri $uri/ =404;
    }
}

```
### Step 4: Redirect HTTP to HTTPS (Optional)

To ensure that all HTTP traffic is redirected to HTTPS, add the following server block:
```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name _;

    return 301 https://$host$request_uri;
}

```
### Step 5: Test the Nginx Configuration

Before restarting Nginx, test the configuration to ensure there are no syntax errors.
```
sudo nginx -t

```
### Step 6: Restart Nginx

If the configuration test is successful, restart Nginx to apply the changes.
```
sudo systemctl restart nginx

```
## Step 7: Verify SSL
Open a web browser and navigate to your server's domain or IP address using HTTPS (e.g., https://your_domain_or_ip).
You should see a warning about the certificate being self-signed, which is expected.
