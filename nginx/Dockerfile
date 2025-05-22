FROM nginx:1.23.4-alpine 

# removing the default nginx configuration file to avoid conflicts with the custom configuration file.
RUN rm -rf /etc/nginx/conf.d/default.conf

# copying the custom nginx configuration file to the nginx configuration directory.
COPY nginx.conf /etc/nginx/conf.d/default.conf