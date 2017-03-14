# Docker Magento Dev Environment
Sample docker development environment for Magento 2.1

This environment use Nginx for the webserver in its own Docker container, there is a PHP 7.0 container for Magento (build based on php-7.0-fpm official). It is also using MariaDB latest version.

The system is also using Unison for Magento folder sync to avoid performance issue on OSX

# Prerequisites
Have [Unison](http://www.cis.upenn.edu/~bcpierce/unison/) installed on your env 
```shell
brew install unison
```
## optional
Have [FSwatch](https://github.com/emcrisostomo/fswatch) installed also to monitor file changes 
```shell
brew install fswatch
```

# Install
1. Download Magento CE from [Magento Tech](https://magento.com/tech-resources/download)
2. Unzip downloaded file in ~/magento
3. Run docker-compose to launch all containers 
```shell
docker-compose up
``` 
alternatively you can use 
```shell
docker-compose up --build
``` 
to rebuild your images if needed (new option/extension for PHP for example)
4. Now sync your magento folder using Unison
```
~/magento> unison . socket://localhost:5000/ -auto -batch
```
This will take time on first sync, but then only the changed files will be sync again
5. Go to [Magento Setup](http://localhost:8880/setup) follow instruction. Database Information are (host:db, Login/password:magento). For the admin URL I strongly suggest to force admin only instead of having random ID in the name.
6. After setup is done don't forget to change rights on the Magento folder one more time `~/magento> chmod -R a+rwx *`
7. Every time you chnage your Magento file you need to rerun the Unison command to avoid this you can use fswatch
```shell
~/magento>fswatch -o . | xargs -n1 -I{} unison . socket://localhost:5000/ -auto -batch
```
