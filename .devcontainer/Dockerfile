FROM mcr.microsoft.com/devcontainers/dotnet:0-7.0-bullseye


#install node.js & typescript
RUN sudo apt-get update && \
    sudo apt-get install -y  nodejs npm && \
    npm install -g typescript

#install untilities
RUN apt install -y build-essential && \
    apt install -y libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev wget curl zsh mercurial

#download njs & nginx source code from nginx.org
RUN cd ~ && \
    wget http://nginx.org/download/nginx-1.23.3.tar.gz && \
    tar -zxvf nginx-1.23.3.tar.gz && \
    hg clone http://hg.nginx.org/njs

#Compile njs and nginx and install nginx including njs dynamic module
RUN cd ~ && \
    cd njs && \
    sudo ./configure && \
    sudo make && \
    cd .. && \
    cd nginx-1.23.3 && \
    sudo ./configure --sbin-path=/usr/bin/nginx \
        --conf-path=/etc/nginx/nginx.conf \
        --error-log-path=/var/log/nginx/error.log \
        --http-log-path=/var/log/nginx/access.log \
        --with-pcre --pid-path=/var/run/nginx.pid \
        --with-http_ssl_module \
        --with-http_auth_request_module \
        --with-stream \
        --with-stream_ssl_module \
        --with-stream_realip_module \
        --with-stream_ssl_preread_module \
        --modules-path=/usr/local/nginx/modules \
        --with-http_v2_module \
        --add-dynamic-module=$HOME/njs/nginx && \
    sudo make && \
    sudo make install