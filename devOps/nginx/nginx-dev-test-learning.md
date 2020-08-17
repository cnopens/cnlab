# nginx-dev-test-learning.md

---


## nginx: [error] invalid PID number "" in "/run/nginx.pid"

解决方法：

需要先执行

nginx -c /etc/nginx/nginx.conf

nginx.conf文件的路径可以从nginx -t的返回中找到。

nginx -s reload


##nginx 解决css、js请求路径无法加载问题

  location / {
        proxy_pass http://192.168.31.162;
        root   /usr/share/nginx/html;
        index  index.html index.htm;
        client_max_body_size    1000m;
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        }
        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
        {
           proxy_pass http://jfinaldemo;
        }

        location ~ .*\.(js|css)?$
        {
            proxy_pass http://jfinaldemo;
        }

解决方法如上。主要是下面两个配置项的修改

  location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
        {
           proxy_pass http://jfinaldemo;
        }

        location ~ .*\.(js|css)?$
        {
            proxy_pass http://jfinaldemo;
        }

## Nginx+Keepalived主从配置(双机主从热备)+Tomcat集群

https://www.cnblogs.com/rianley/p/11760174.html
