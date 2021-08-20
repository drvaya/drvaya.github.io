---
layout: post
title:  "Setup Nginx as a reverse proxy for sub-domain with Odoo (OpenERP) on Google Cloud Platform"
author: dharam
categories: [ tutorial ]
tags: [yellow]
image: assets/images/ODOOKZ.png
description: "How to setup Nginx as a reverse proxy for sub-domain with Odoo (OpenERP) on Google Cloud Platform ?"
featured: true
---

**How to setup Nginx as a reverse proxy for sub-domain with Odoo (OpenERP) on Google Cloud Platform ?**

**What is Odoo also known as OpenERP ?**

Odoo is an open source ERP [Enterprise Resource Planning] platform that is freely available and anyone can customize it based on their organization/process requirements. Odoo is an abbreviation for On-Demand Open Object. It’s basically a large set of business/operations modules like CRM, Sales, Inventory, Warehouse, HRMS etc and that essentially form a basis of any ERP Software package. More about Odoo.. [Read here](https://www.odoo.com/documentation/13.0/)

**Odoo on Google Cloud Platform**

Odoo is provided as an application by Bitnami on the GCP Marketplace.

On your GCP Console you can goto Marketplace or click on this link to open the Odoo offering.

<https://console.cloud.google.com/marketplace/details/bitnami-launchpad/odoo>

![ODOO Bitnami on GCP Marketplace](/assets/images/odoo-gcp-bitnami.png "ODOO Bitnami on GCP Marketplace")

As you can see, there is no usage/license fee for Odoo usage, it is only the Compute Engine costs that you need to incur.

**Sub-domains with Odoo**

I have often come across situations where internal divisions use different modules of the Odoo package and they would like to host it as sub-domains under their organization.

Say your company domain is openerp.com and you want the HRMS module to be available as hrms.openerp.com. Likewise if you have several such sub-domains how do you configure this using Nginx as a reverse proxy.

**_Reverse Proxy :_** A reverse proxy is an intermediary proxy service which takes a client request, passes it on to one or more servers, and subsequently delivers the server’s response to the client.

Nginx Reverse proxy has quite some benefits like Load Balancing, Increased Security, Better performance and easy audit logs.
## 
Let us now begin with our configuration of Nginx reverse proxy for sub-domains -
SSH into your VM
Note : Use ‘sudo’ if you are not a root user and install nginx
```
apt-get update
apt-get install nginx
```
Within the **/etc/nginx/sites-available** directory, we shall create the configuration that is responsible for the reverse proxy information and management. Name this file as the **sub-domain** itself, something like **_hrms.openerp.com_** (Not mandatory, but in future if you keep on increasing sub-domains it would just become easy to remember this naming convention).

```
upstream hrmsopenerp {  
    server 127.0.0.1:8069;  
}  
server {  
        listen 443 default;  
        server_name hrms.openerp.com;access_log /var/log/nginx/oddo.access.log;  
        error_log /var/log/nginx/oddo.error.log;if ($scheme = http) {  
                return 301 [https://hrms.openerp.com$request_uri](https://hrms.openerp.com$request_uri);  
        }# SSL cerificate details - Use Lets Encrypt in Live (this is testing)  
        ssl on;  
        ssl_certificate     /etc/nginx/ssl/nginx.crt;  
        ssl_certificate_key /etc/nginx/ssl/nginx.key;# limit ciphers  
        ssl_protocols TLSv1.2 TLSv1.1 TLSv1;  
        ssl_ciphers ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:DHE-RSA-AES256-SHA;  
        ssl_prefer_server_ciphers on;proxy_buffers 16 64k;  
        proxy_buffer_size 128k;  
        location / {  
                proxy_next_upstream error timeout invalid_header http_500 http_502  http_503 http_504;  
                proxy_redirect off;  
                proxy_set_header Host $host;  
                proxy_set_header X-Real-IP $remote_addr;  
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;  
                proxy_set_header X-Forwarded-Proto $scheme;  
                proxy_pass [http://hrmsopenerp](http://hrmsopenerp);  
        }  
        location ~* /web/static/ {  
                proxy_cache_valid 200 60m;  
                proxy_buffering on;  
                expires 864000;  
                proxy_pass [http://hrmsopenerp](http://hrmsopenerp);  
        }  
        gzip_types text/css text/less text/plain text/xml application/xml application/json application/javascript;  
        gzip on;  
}
## http redirects to https ##  
server {  
    listen      80;  
    server_name hrms.openerp.com;  
    # Strict Transport Security  
    add_header Strict-Transport-Security max-age=2592000;  
    rewrite ^/.*$ [https://$host$request_uri](https://$host$request_uri)? permanent;  
}
```

Once this file is created, you need to activate it by linking it to sites-enabled using the command -

```
ln -s /etc/nginx/sites-available/hrms.openerp.com /etc/nginx/sites-enabled
```
Now just restart your nginx and you are good to proceed.
```
sudo systemctl restart nginx
```
Test your application on any browser -
http://hrms.openerp.com/  
https://hrms.openerp.com/

_Note : You must have your DNS entries pointing to your server before applying these steps._

**_Additional Notes:_**

The above configuration also solves the following configuration problems generally encountered -

_Redirect HTTP to HTTPS_

_Reverse Proxy on nginx for custom ports_

Finally, in order to get your sites marked as secured whenever someone visits it, ensure that you have set the right SSL protocols and ciphers in your nginx configs and also preferably use certificates from Let’s Encrypt.
