# Use the official NGINX base image
FROM nginx

# Install OpenSSL
RUN apt-get update && apt-get install -y openssl

ARG COUNTRY="IN"
ARG STATE="Tamil Nadu"
ARG LOCATION="Ooty"
ARG ORGANIZATION="DevOps Org"
ARG COMMON_NAME="nginx.devops.org"

# Generate a self-signed SSL certificate
RUN openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 \
    -subj "/C=${COUNTRY}/ST=${STATE}/L=${LOCATION}/O=${ORGANIZATION}/CN=${COMMON_NAME}" \
    -keyout /etc/nginx/cert.key -out /etc/nginx/cert.crt

# Copy the NGINX configuration file
COPY ./nginx.conf /etc/nginx/nginx.conf

# Expose ports
EXPOSE 80 443

# Start NGINX
ENTRYPOINT ["nginx", "-g", "daemon off;"]