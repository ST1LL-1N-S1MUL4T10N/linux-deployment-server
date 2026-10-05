# Nginx

Nginx serves the PXE deployment files over HTTP.

## Installation

Nginx was installed with:

```bash
sudo apt-get install -y ipxe dnsmasq nginx python3-yaml
```

## Configuration

The active main configuration is:

```text
/etc/nginx/nginx.conf
```

The active default virtual host is:

```text
/etc/nginx/sites-enabled/default
```

The configuration file used for the default virtual host is:

```text
/etc/nginx/sites-available/default
```

The default virtual host serves:

```text
/var/www/html
```

over HTTP port `80`.

The active server configuration is the standard nginx default configuration:

```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

## Deployment files

Nginx serves the deployment files located under:

```text
/var/www/html/
```

The active deployment content is:

```text
/var/www/html/
├── autoinstall/
├── boot.ipxe
└── ubuntu/
```

## Verification

Test the nginx configuration:

```bash
sudo nginx -t
```

Restart nginx:

```bash
sudo systemctl restart nginx
```

Verify that it is running:

```bash
systemctl is-active nginx
```

Test the deployment files:

```bash
curl -fsS http://172.16.110.10/boot.ipxe >/dev/null
curl -fsS http://172.16.110.10/ubuntu/vmlinuz >/dev/null
curl -fsS http://172.16.110.10/ubuntu/initrd >/dev/null
curl -fsS http://172.16.110.10/autoinstall/meta-data >/dev/null
curl -fsS http://172.16.110.10/autoinstall/vendor-data >/dev/null
curl -fsS http://172.16.110.10/autoinstall/user-data >/dev/null
```

Successful completion of these commands verifies that nginx can serve all required PXE, Ubuntu, and NoCloud files.

## Logs

Nginx access log:

```text
/var/log/nginx/access.log
```

Nginx error log:

```text
/var/log/nginx/error.log
```

Monitor deployment requests with:

```bash
sudo tail -F /var/log/nginx/access.log
```

Monitor both access and error logs with:

```bash
sudo tail -F /var/log/nginx/access.log /var/log/nginx/error.log
```

## Reconstructing nginx

Install nginx:

```bash
sudo apt-get install -y nginx
```

Copy the repository configuration files to their corresponding locations:

```text
configs/nginx/nginx.conf
    -> /etc/nginx/nginx.conf

configs/nginx/default
    -> /etc/nginx/sites-available/default
```

Ensure the default site is enabled through:

```text
/etc/nginx/sites-enabled/default
```

Place the deployment files under:

```text
/var/www/html/
```

Validate and restart:

```bash
sudo nginx -t
sudo systemctl restart nginx
systemctl is-active nginx
```

Then test the HTTP URLs listed above.
