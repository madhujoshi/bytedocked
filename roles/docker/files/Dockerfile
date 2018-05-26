FROM centos
RUN yum install epel-release -y
RUN yum install openssl -y
RUN yum install nginx -y

RUN mkdir -p /etc/ssl/private
RUN /usr/bin/openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt -subj '/C=US/ST=Virginia/L=ByteCubed/OU=IT'

RUN echo "BYTECUBED DEVOPS CHALLENGE" > /usr/share/nginx/html/index.html

COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
