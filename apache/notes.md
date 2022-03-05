# https://hub.docker.com/_/httpd/

docker run -d --name my-apache-app --rm httpd:2.4

https://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-rg-de-4/s1-apache-config.html


Alternative: 

Configure httpd.conf within Dockerfile
RUN sed -i '/LoadModule rewrite_module/s/^#//g' /usr/local/apache2/conf/httpd.conf; \ 
    sed -i '/LoadModule expires_module/s/^#//g' /usr/local/apache2/conf/httpd.conf; \ 
    sed -i '/LoadModule headers_module/s/^#//g' /usr/local/apache2/conf/httpd.conf; \ 
    sed -i '/LoadModule ssl_module/s/^#//g' /usr/local/apache2/conf/httpd.conf; \ 
    sed -i '/LoadModule http2_module/s/^#//g' /usr/local/apache2/conf/httpd.conf; \ 
    sed -i '/LoadModule mpm_event_module modules/mod_mpm_event.so/s/^#//g' /usr/local/apache2/conf/httpd.conf; \ 
    sed -i '/Include conf\/extra\/httpd-vhosts.conf/s/^#//g' /usr/local/apache2/conf/httpd.conf;