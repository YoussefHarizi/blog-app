# Mini Blog laravel8 vuejs
A simple mini blog for demonstration purpose. Based on Laravel 8.

# Installation
Clone the repository-
```sh
clone https://github.com/YoussefHarizi/blog-app.git blog-app
```
Then cd into the folder with this command-
```sh
cd blog-app
```
Then if you use docker run :
```sh
docker run --rm \
    -u "$(id -u):$(id -g)" \
    -v $(pwd):/opt \
    -w /opt \
    laravelsail/php80-composer:latest \
    composer install --ignore-platform-reqs
```
Create a environment file using this command-
```sh
cp .env.example .env
```
change this lines in .env file :
```sh
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=blog-app
DB_USERNAME=sail
DB_PASSWORD=password
```
Then run application with :
```sh
./vendor/bin/sail up -d
```
Then do a composer install
```sh
./vendor/bin/sail composer install
```

Then do a database migration using this command-
```sh
./vendor/bin/sail artisan migrate
```
generate application key, which will be used for password hashing, session and cookie encryption etc.
```sh
./vendor/bin/sail artisan key:generate
```
Run 
```sh
 ./vendor/bin/sail npm install 
 ./vendor/bin/sail npm run dev 
 ``` 
 to install all front end dependencies
 
Run ``` npm run watch ``` to build vue 
# Screenshots
<p align="center"><img src="https://github.com/YoussefHarizi/blog-app/blob/main/public/images/img1.png" style="width:400;box-shadow: 2px 2px 5px black;margin-bottom:2px;"></p>
<p align="center"><img src="https://github.com/YoussefHarizi/blog-app/blob/main/public/images/img4.png" style="width:400;box-shadow: 2px 2px 5px black;margin-bottom:2px;"></p>





