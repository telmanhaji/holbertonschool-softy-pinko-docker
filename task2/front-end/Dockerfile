FROM nginx:latest

# Copy front-end files into the container
COPY ./softy-pinko-front-end /var/www/html/softy-pinko-front-end

# Copy custom nginx config into container
COPY ./softy-pinko-front-end.conf /etc/nginx/conf.d/default.conf

EXPOSE 9000