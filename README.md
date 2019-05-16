# nginx-for-php-fpm

This Docker image allows for serving php-fpm apps with nginx. It uses Alpine Linux, so it's lean and mean. Designed to be easily used within Docker Swarm.

## Usage instructions

This image is designed for use within a Compose file. To use it, you should:

1. Create a service in your docker-compose.yml file using this image (joshwalsh/nginx-for-php-fpm)
2. Create a service in your docker-compose.yml file which runs your PHP application with PHP-FPM
    a. The PHP-FPM image can contain the application code anywhere in its filesystem (`/var/www/html` is commonly used)
    b. The php-fpm.conf file should contain a `chdir` directive which points to the directory that you want served by nginx (e.g. for a Laravel app, you might wish to serve `/var/www/html/public/`)
    c. The PHP-FPM service should either be named `php-fpm`, or should be aliased as `php-fpm` on a network that the nginx service has access to
    d. The PHP-FPM service should listen on port 9000
3. Create a [named volume](https://docs.docker.com/compose/compose-file/#volume-configuration-reference) to store the content that you want nginx to serve
4. Add this volume to the php-fpm service so it maps to the location from step 2b
5. Add this volume to the nginx service so it maps to `/var/www/html`
6. Make it so that the nginx service `depends_on` the php-fpm service
7. If you want to apply any customisations to the nginx configuration, (e.g. redirect or rewrite rules) you can use a [config](https://docs.docker.com/compose/compose-file/#configs) with the target path set to /etc/nginx/conf.d/custom.conf or you can share it from your php-fpm image via a named volume