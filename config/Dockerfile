FROM python:3.6-stretch

LABEL maintainer="Chi Shen <njnuko@163.com>"

#copy the files
RUN mkdir /www
COPY . /www
WORKDIR /www

RUN useradd ko
RUN chown -R ko:ko /www


#change the apt source list
RUN mv /etc/apt/sources.list /etc/apt/sources.list.back
COPY ./config/sources.list /etc/apt

RUN apt-get update
RUN apt-get install nginx vim openssh-server net-tools -y

#config the ssh server
RUN mv /etc/ssh/sshd_config /etc/ssh/sshd_config.back
COPY ./config-sample/sshd_config /etc/ssh


RUN mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.back
COPY ./config-sample/nginx.conf /etc/nginx

RUN echo "root:1" | chpasswd
# Removed the section that breaks pip installations

# && apt-get remove --purge --auto-remove -y apt-transport-https ca-certificates 
# Standard set up Nginx finished


# Install uWSGI
RUN pip install uwsgi wagtail psycopg2-binary -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com

EXPOSE 80
EXPOSE 443

# Make NGINX run on the foreground
# Remove default configuration from Nginx
RUN rm /etc/nginx/sites-enabled/default
# Copy the modified Nginx conf
COPY ./config-sample/ko.conf /etc/nginx/sites-enabled/



# Install Supervisord
RUN apt-get update && apt-get install -y supervisor \
&& rm -rf /var/lib/apt/lists/*
# Custom Supervisord config
COPY ./config-sample/supervisord.conf /etc/supervisor/conf.d/supervisord.conf



CMD ["/usr/bin/supervisord"]

