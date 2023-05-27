FROM ubuntu
RUN apt update
RUN apt install apache2 apache2-utils -y
ADD index.html /var/www/html/index.html
EXPOSE 80
ENTRYPOINT ["apachectl", "-DFOREGROUND"]