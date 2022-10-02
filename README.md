# uniswap 
[uniswap github](https://github.com/Uniswap/interface)

[.env](https://github.com/nftrun/interface/blob/main/.env) 中配置infura，AMPLITUDE，部署routing-api的网关AWS_API_ENDPOINT及API_KEY

[slice.ts](https://github.com/nftrun/interface/blob/main/src/state/routing/slice.ts)中设置部署好的routing api
```bash
baseUrl: 'https://hucuww71t3.execute-api.us-east-2.amazonaws.com/prod/'
```



# uniswap-routing

获取价格的路由项目参考
[uniswap-routing github](https://github.com/Uniswap/routing-api/tree/l2-gas-estimates)

已上传至
https://github.com/nftrun/uniswap-routing


# 部署
```bash
# 打包
yarn run build

# 运行，默认端口为3000
yarn run serve

```



## nginx配置
```bash
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
	worker_connections 768;
	# multi_accept on;
}

http {

	##
	# Basic Settings
	##

	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	keepalive_timeout 65;
	types_hash_max_size 2048;
	# server_tokens off;

	# server_names_hash_bucket_size 64;
	# server_name_in_redirect off;

	include /etc/nginx/mime.types;
	default_type application/octet-stream;
	client_header_buffer_size 512k;
	large_client_header_buffers 4 512k;
	##
	# SSL Settings
	##

	ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
	ssl_prefer_server_ciphers on;

	##
	# Logging Settings
	##

	access_log /var/log/nginx/access.log;
	error_log /var/log/nginx/error.log;

	##
	# Gzip Settings
	##

	gzip on;

	# gzip_vary on;
	# gzip_proxied any;
	# gzip_comp_level 6;
	# gzip_buffers 16 8k;
	# gzip_http_version 1.1;
	# gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

	##
	# Virtual Host Configs
	##
    server{
        listen 443 ssl;
		server_name upswap.org www.upswap.org;
        ssl_certificate /etc/ssl/Nginx/STAR_upswap_org_integrated.crt;
        ssl_certificate_key /etc/ssl/Nginx/STAR_upswap_org.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;
       root /root/project/interface/build;
        index index.html;		
        if ($host = 'upswap.org' ) {
                rewrite ^/(.*)$ https://app.upswap.org/$1 permanent;
        }
		location / {
			add_header 'Access-Control-Allow-Origin' $http_origin;
			add_header 'Access-Control-Allow-Credentials' 'true';
			add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
			add_header 'Access-Control-Allow-Headers' 'DNT,web-token,app-token,Authorization,Accept,Origin,Keep-Alive,User-Agent,X-Mx-ReqToken,X-Data-Type,X-Auth-Token,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
			add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
			if ($request_method = 'OPTIONS') {
				add_header 'Access-Control-Max-Age' 1728000;
				add_header 'Content-Type' 'text/plain; charset=utf-8';
				add_header 'Content-Length' 0;
				return 204;
			}
			proxy_pass http://localhost:3000;			
        } 
	}
	server { 
        listen	80;
		server_name upswap.org;
	    rewrite ^(.*) https://app.$server_name$1 permanent; #http 跳转 https
    }
}

```



