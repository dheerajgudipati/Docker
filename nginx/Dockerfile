# Set the base image to Ubuntu
FROM ubuntu


MAINTAINER dheerajgudipati<dheeraj.vn513@gmail.com>

# Update the repository
RUN apt-get update

# Install necessary tools
# RUN apt-get install -y nano wget dialog net-tools

# Download and Install Nginx
RUN apt-get install -y nginx  

ADD default.conf /etc/nginx/conf.d/

RUN echo "daemon off;" >> /etc/nginx/nginx.conf

EXPOSE 80

CMD service nginx start
