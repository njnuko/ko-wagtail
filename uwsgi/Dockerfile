FROM python:3.6-stretch
LABEL maintainer="Chi Shen <njnuko@163.com>"
#copy the files
RUN mkdir /www
WORKDIR /www
RUN mkdir uwsgi
COPY ./uwsgi.ini /www/uwsgi
RUN useradd ko
#change the apt source list


RUN echo "root:1" | chpasswd
# Removed the section that breaks pip installations

# && apt-get remove --purge --auto-remove -y apt-transport-https ca-certificates 
# Standard set up Nginx finished


# Install uWSGI
RUN pip install uwsgi wagtail psycopg2-binary -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
# Expose 10701 port
EXPOSE 10701 


RUN chown -R ko:ko /www

CMD ["uwsgi","/www/uwsgi/uwsgi.ini"]
