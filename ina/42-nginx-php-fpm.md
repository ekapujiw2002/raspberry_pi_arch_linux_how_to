# Web server nginx
1. Install nginx dan php-fpm dengan perintah : `sudo pacman -S --needed nginx php-fpm` . Untuk instalasi **php** dan **mysql** dapat dilihat pada **http://www.github.com/ekapujiw2002/raspberry_pi_arch_linux_how_to/ina/23-Apache%20-%20PHP%20-%20MySql.md**
2. Jalankan nginx dengan perintah : `sudo systemctl start nginx` . Lalu jalankan php-fpm dengan perintah : `sudo systemctl start php-fpm`
3. Buka link **http://192.168.1.250** pada browser dan akan ditampilkan halaman awal nginx.
4. Ubah konfigurasi nginx dengan mengedit file **/etc/nginx/nginx.conf**. Ubah sesuai isian berikut ini : 
	
	```
	#user html;
	worker_processes  4;
	
	events {
	worker_connections  1024;
	}
	
	http {
	    include       mime.types;
	    default_type  application/octet-stream;
	
	    access_log off;
	
	    #fastcgi cache off
	    fastcgi_temp_path  /tmp/fastcgi_temp 1 2;
	
	    sendfile        off;
	    #tcp_nopush     on;
	
	    #no cache please
	    expires off;
	
	    #keepalive_timeout  0;
	    keepalive_timeout  65;
	
	    #gzip  on;
	
	    #include all vshost on this directory
	    include /etc/nginx/sites-enabled/*;
	}
	```  
5. Buat sebuah folder dengan nama **sites-enabled** di dalam folder **/etc/nginx** dengan perintah `sudo mkdir /etc/nginx/sites-enabled`
6. Buat sebuah di folder **sites-enabled** dengan nama **default** dan isi dengan konfigurasi berikut ini : 

	```
	server {
	    listen 80;
	    server_name localhost;
	    root /srv/http/;
	    index index.php index.html index.htm;
	
	    autoindex on;
	
	    location /img {
	        alias /dev/shm;
	    }
	
	    include php.conf;
	    include drop.conf;
	}
	``` 
7. Buat sebuah file **php.conf** di folder **/etc/nginx** dengan isian sebagai berikut :

	```
	location ~ \.php {
	    # for security reasons the next line is highly encouraged
	    try_files $uri =404;
	
	    fastcgi_param  QUERY_STRING       $query_string;
	    fastcgi_param  REQUEST_METHOD     $request_method;
	    fastcgi_param  CONTENT_TYPE       $content_type;
	    fastcgi_param  CONTENT_LENGTH     $content_length;
	
	    fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
	
	    # if the next line in yours still contains $document_root
	    # consider switching to $request_filename provides
	    # better support for directives such as alias
	    fastcgi_param  SCRIPT_FILENAME    $request_filename;
	
	    fastcgi_param  REQUEST_URI        $request_uri;
	    fastcgi_param  DOCUMENT_URI       $document_uri;
	    fastcgi_param  DOCUMENT_ROOT      $document_root;
	    fastcgi_param  SERVER_PROTOCOL    $server_protocol;
	
	    fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
	    fastcgi_param  SERVER_SOFTWARE    nginx;
	
	    fastcgi_param  REMOTE_ADDR        $remote_addr;
	    fastcgi_param  REMOTE_PORT        $remote_port;
	    fastcgi_param  SERVER_ADDR        $server_addr;
	    fastcgi_param  SERVER_PORT        $server_port;
	    fastcgi_param  SERVER_NAME        $server_name;
	
	    # If using a unix socket...
	    # fastcgi_pass unix:/tmp/php5-fpm.sock;
	    fastcgi_pass unix:/run/php-fpm/php-fpm.sock;
	
	    # If using a TCP connection...
	    #fastcgi_pass 127.0.0.1:9000;
	}
	``` 
8. Buat sebuah file **drop.conf** di folder **/etc/nginx** dengan isian sebagai berikut :

	```
	location = /robots.txt  { access_log off; log_not_found off; }
	location = /favicon.ico { access_log off; log_not_found off; }	
	location ~ /\.          { access_log off; log_not_found off; deny all; }
	location ~ ~$           { access_log off; log_not_found off; deny all; }
	```
9. Restart **nginx** dengan : `sudo systemctl restart nginx` dan juga **php-fpm** dengan `sudo systemctl restart php-fpm` 
10. Silakan masukkan semua konten website ke folder **/srv/http**
