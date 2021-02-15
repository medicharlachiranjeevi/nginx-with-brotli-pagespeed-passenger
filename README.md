# nginx-with-brotli-pagespeed-passenger

### upate pagackges 
```
apt-get update -y
```

### Add passenger key
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7

```
### Add passenger repo
```
sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger $(lsb_release -cs) main > /etc/apt/sources.list.d/passenger.list'
```
### NGINX key
````
curl -L https://nginx.org/keys/nginx_signing.key | sudo apt-key add -

````
### NGINX repo for soucrce
````
sudo sh -c 'echo deb [arch=amd64]  http://nginx.org/packages/ubuntu/  $(lsb_release -cs) nginx > /etc/apt/sources.list.d/nginx.list'
sudo sh -c 'echo deb-src http://nginx.org/packages/ubuntu/  $(lsb_release -cs) nginx >> /etc/apt/sources.list.d/nginx.list'
sudo apt update
````
### Install Dependencies

```
sudo apt-get install uuid-dev dpkg-dev build-essential zlib1g-dev libpcre3 libpcre3-dev unzip git passenger mmdb-bin  gnupg2 git gcc cmake libpcre3 libpcre3-dev zlib1g zlib1g-dev openssl libssl-dev curl unzip -y
```
###
````
cd /usr/local/src
apt-get source nginx
sudo apt-get build-dep nginx -y
git clone --recursive https://github.com/google/ngx_brotli.git
wget https://github.com/apache/incubator-pagespeed-ngx/archive/v1.13.35.2-stable.tar.gz
wget https://www.modpagespeed.com/release_archive/1.13.35.2/psol-1.13.35.2-x64.tar.gz
tar xpf psol-1.13.35.2-x64.tar.gz
tar xvzf v1.13.35.2-stable.tar.gz
mv incubator-pagespeed-ngx-1.13.35.2-stable/ pagespeed-ngx-1.13.35.2-stable
cd pagespeed-ngx-1.13.35.2-stable/
mv ../psol/ .
cd ..
rm psol-1.13.35.2-x64.tar.gz v1.13.35.2-stable.tar.gz
````
### Next, change the directory to the Nginx source and edit the rules file:
```
cd /usr/local/src/nginx-*/
```
### Find the ‘config.env.nginx‘ and ‘config.env.nginx_debug’ section and add the following line within ./configure line: 
```
--add-module=/usr/local/src/ngx_brotli --add-module=/usr/local/src/pagespeed-ngx-1.13.35.2-stable --add-module=/usr/local/src/headers-more-nginx-module --add-module=$(passenger-config --nginx-addon-dir)
```
### Save and close the file, then compile and build the nginx package with the following command: 
```
dpkg-buildpackage -b -uc -us

```
### Next, install the Nginx by running the both *.deb file:

```
sudo dpkg -i /usr/local/src/*.deb
```


