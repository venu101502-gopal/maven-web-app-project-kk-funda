# maven-web-app-project-kk-funda
###############################################
server {
    listen 443 ssl;
    server_name sutitravel001.sutisoft.in;

    ssl_certificate "/etc/nginx/sutisoftCerts/zabbix.sutisoft.in.crt";
    ssl_certificate_key "/etc/nginx/sutisoftCerts/sutisoft.in.key";

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    access_log "/var/log/nginx/nginx.ssl-access.log";
    error_log "/var/log/nginx/nginx.ssl-error.log";

    add_header X-XSS-Protection "1; mode=block" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header Strict-Transport-Security "max-age=15768000; includeSubDomains" always;
    add_header Cache-Control "max-age=0, no-cache, no-store, must-revalidate";
    add_header Pragma "no-cache";

    root /var/www/html/travel-b2c/;
    index index.html;

    location / {
        try_files $uri $uri$args/ /index.html;
    }

    location /travel-b2b {
        proxy_pass http://127.0.0.1:8080;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_headers_hash_max_size 512;
        proxy_headers_hash_bucket_size 128;
    }

#         location ~* \.(jpg|ttf|JPG|JPEG|svg|SVG|pdf|PDF|doc|DOC|xls|XLS|ods|swf|docx|DOCX|png|PNG|gif|GIF|BMP|bmp|zip|css|txt|ppt|pptx|PPT|PPTX|rtf|odt|odp|log|log*|jrxml|php|tif|TIF|tiff|TIFF|csv|cur|ico|html|iif|IIF|images|javascript|js|css|flash|media|static)$  {
  #    root /var/www/html/travel-b2c;
      #root /var/www/html;
 #     expires 30d;
# }


    location /nginx_status {
        stub_status;
    }
}

server {
    listen 80;
    server_name sutitravel001.sutisoft.in;
    return 301 https://$host$request_uri;
}

########################################################

server {
    listen           80;
    server_name      192.168.0.244;
    access_log       "/var/log/nginx/nginx.access.log";
    error_log        "/var/log/nginx/nginx.error.log debug";
    charset UTF-8;

    # Security Headers
    add_header X-XSS-Protection "1; mode=block" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header Strict-Transport-Security "max-age=15768000; includeSubDomains" always;
    add_header Cache-Control "max-age=0, no-cache, no-store, must-revalidate";
    add_header Pragma "no-cache";
    add_header Allow "GET, POST, HEAD" always;

    # Proxy settings
    location / {
        proxy_pass      http://127.0.0.1:8080;
        proxy_redirect  off;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # Serve static files
    location ~* \.(JSON|json|jpg|ttf|JPG|JPEG|pdf|PDF|doc|DOC|xls|XLS|ods|swf|docx|DOCX|png|PNG|gif|GIF|BMP|bmp|zip|css|txt|ppt|pptx|PPT|PPTX|rtf|odt|odp|log|log*|jrxml|php|tif|TIF|tiff|TIFF|csv|cur|ico|html|iif|IIF|images|javascript|js|css|flash|media|static)$ {
        root /var/www/html/;
        expires 30d;
    }
}

# Redirect HTTP to HTTPS
server {
    listen 80;
    server_name sutiap001qa.sutisoft.in;
    return 301 https://$host$request_uri;
}

