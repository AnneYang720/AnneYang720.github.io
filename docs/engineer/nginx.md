## 1 Nginx

NGINX is open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more. In addition to its HTTP server capabilities, NGINX can also function as a proxy server for email (IMAP, POP3, and SMTP) and a reverse proxy and load balancer for HTTP, TCP, and UDP servers.

![img](https://raw.githubusercontent.com/dunwu/images/dev/cs/web/nginx/nginx.jpg)

> **_NOTE:_**  "Reverse" refers to using a proxy server to accept connection requests on the Internet, then forwarding the requests to the server on the internal network, and returning the results obtained from the server to the client. At this time, the proxy server appears as a reverse proxy server to the client.


## 2 Getting Start

Commands:
|||
|-|-|
| systemctl start nginx | start nginx |
| systemctl stop nginx | stop nginx, kill all processes |
| systemctl quit nginx | similar to stop but more graceful |
| systemctl restart nginx | restart nginx, basically performs a stop then a start |
| systemctl reload nginx | similar to restart but more graceful |
| systemctl status nginx | view server status |
| systemctl config nginx | test nginx configuration |
| systemctl -v nginx | check nginx version (use -V for more details) |



## 3 Nginx Practice

### Http Reverse Proxy

A basic `nginx.conf`: 

```nginx
#user somebody;

#number of worker processes, usually equal to CPU cores
worker_processes  1;

#configures logging
error_log  D:/Tools/nginx-1.10.1/logs/error.log;
error_log  D:/Tools/nginx-1.10.1/logs/notice.log  notice;
error_log  D:/Tools/nginx-1.10.1/logs/info.log  info;

#PID file that will store the process ID of the main process
pid        D:/Tools/nginx-1.10.1/logs/nginx.pid;

#configuration file context that specifies connection processing
events {
    worker_connections 1024;    #max number of simultaneous connections by one worker
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       80;
        server_name  www.helloworld.com;
        charset utf-8;

        location / {
            root   D:\01_Workspace\Project\github\zp\SpringNotes\spring-security\spring-shiro\src\main\webapp;
            index  index.html index.htm;
        }

		#proxy parameters
        proxy_connect_timeout 180;
        proxy_send_timeout 180;
        proxy_read_timeout 180;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarder-For $remote_addr;

        #Static files, handled by nginx
        location ~ ^/(images|javascript|js|css|flash|media|static)/ {
            root D:\01_Workspace\Project\github\zp\SpringNotes\spring-security\spring-shiro\src\main\webapp\views;
            expires 30d;
        }

        #Block access to .htxxx files
        location ~ /\.ht {
            deny all;
        }

		#Error handling page
		#error_page   404              /404.html;
		#error_page   500 502 503 504  /50x.html;
        #location = /50x.html {
        #    root   html;
        #}
    }
}
```

> **_NOTE:_** `conf/nginx.conf` is the defaul configuration file, or use nginx -c to specify the config


### Https Reverse Proxy
A few things you need to know when using nginx to configure https:

- The fixed port number of HTTPS is 443, which is different from HTTP’s port 80
- The SSL standard requires security certificates, so in nginx.conf you need to specify the certificate and its corresponding key

```nginx
server {
    listen       443 ssl;
    server_name  www.helloworld.com;

    ssl_certificate      cert.pem;
    ssl_certificate_key  cert.key;

    #ssl parameters (selective configuration)
    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;
    #digital signature(MD5 here)
    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers  on;

    location / {
        root   /root;
        index  index.html index.htm;
    }
}
```

### Load Balancer
![img](https://raw.githubusercontent.com/dunwu/images/dev/cs/web/nginx/nginx-load-balance.png)

```nginx
http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    access_log    /var/log/nginx/access.log;

    upstream load_balance_server {
        #higher weight -> greater probability
        server 192.168.1.11:80   weight=5;
        server 192.168.1.12:80   weight=1;
        server 192.168.1.13:80   weight=6;
    }

   server {
        listen       80;
        server_name  www.helloworld.com;

        location / {
            proxy_pass  http://load_balance_server ;
        }
    }
}
```

#### Load Balancing Strategy

1. Round Robin and weighted Round Robin
```nginx
upstream bck_testing_01 {
  server 192.168.250.220:8080       weight=3
  server 192.168.250.221:8080       # default weight=1
  server 192.168.250.222:8080       # default weight=1
}
```

2. Least Connections and weighted Least Connections
```nginx
upstream bck_testing_01 {
  least_conn; # compare conns/weight

  server 192.168.250.220:8080       weight=3
  server 192.168.250.221:8080       # default weight=1
  server 192.168.250.222:8080       # default weight=1
}
```

3. IP Hash (based on client IP address) and weighted IP Hash
```nginx
upstream bck_testing_01 {
  ip_hash;

  # with default weight for all (weight=1)
  server 192.168.250.220:8080
  server 192.168.250.221:8080
  server 192.168.250.222:8080
}
```

4. Hash (on specified request characteristics)
```nginx
upstream bck_testing_01 {
  hash $request_uri;

  # with default weight for all (weight=1)
  server 192.168.250.220:8080
  server 192.168.250.221:8080
  server 192.168.250.222:8080
}
```

5. Consistent (ketama) Hash
6. Random with Two Choices

### Website with Mutiple WebApps
```nginx
http {
	#skip basic configs

    #Defines a group of servers
	upstream product_server{
		server www.helloworld.com:8081;
	}

	upstream admin_server{
		server www.helloworld.com:8082;
	}

	upstream finance_server{
		server www.helloworld.com:8083;
	}

	server {
		#skip basic configs

		location / {
			proxy_pass http://product_server;
		}

		location /product/{
			proxy_pass http://product_server;
		}

		location /admin/ {
			proxy_pass http://admin_server;
		}

		location /finance/ {
			proxy_pass http://finance_server;
		}
	}
}
```

### Static Site
Static sites include only html files and other static resources.

For example, if all static resources are placed in `/app/dist` directory:

```nginx
http {
    server {
		listen       80;
		server_name  static.zp.cn;

		location / {
			root /app/dist;
			index index.html;
		}
	}
}
```

### A Simple File Server
```nginx
autoindex on;               # show directory
autoindex_exact_size on;    # show file size
autoindex_localtime on;     # show file last modified time

server {
    charset      utf-8,gbk; # gbk for Chinese
    listen       9050 default_server;       # ipv4
    listen       [::]:9050 default_server;  # ipv6
    server_name  _;
    root         /share/fs; # root path
}
```

### Cross-Origin

**Cross-origin resource sharing (CORS)** is a mechanism that allows restricted resources on a web page to be accessed from another domain outside the domain from which the first resource was served.

https://aws.amazon.com/cn/what-is/cross-origin-resource-sharing/


1.  **CORS**
Add the allowed domain to server configuration file:
```
Access-Control-Allow-Origin: https://news.example.com
```

and response with with `Access-Control-Allow-Credentials: true` header.

2.  **CORS** with nginx
For example, www.helloworld.com consists frontend 9000 and backend 8080. There will be CORS problem when frontend interacts with backend using http, because the default port of http:// is 80.

Set cors in the `enable-cors.conf`:
```nginx
# allow origin list
set $ACAO '*';

# set single origin
if ($http_origin ~* (www.helloworld.com)$) {
  set $ACAO $http_origin;
}

if ($cors = "trueget") {
	add_header 'Access-Control-Allow-Origin' "$http_origin";
	add_header 'Access-Control-Allow-Credentials' 'true';
	add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
	add_header 'Access-Control-Allow-Headers' 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
}

if ($request_method = 'OPTIONS') {
  set $cors "${cors}options";
}

if ($request_method = 'GET') {
  set $cors "${cors}get";
}

if ($request_method = 'POST') {
  set $cors "${cors}post";
}
```

Include `enable-cors.conf` in conf:

```nginx
upstream front_server{
    server www.helloworld.com:9000;
}
upstream api_server{
    server www.helloworld.com:8080;
}

server {
    listen       80;
    server_name  www.helloworld.com;

    location ~ ^/api/ {
        include enable-cors.conf;
        proxy_pass http://api_server;
        rewrite "^/api/(.*)$" /$1 break;
    }

    location ~ ^/ {
        proxy_pass http://front_server;
    }
}
```

3.  **jsonp**
Skip