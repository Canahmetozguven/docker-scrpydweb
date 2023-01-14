FROM python:3.7.1


WORKDIR /home/spider_workplace/TLNewsCrawl

COPY ./requirements.txt ./
RUN apt-get update 
RUN rm -f /etc/localtime && cp /usr/share/zoneinfo/Europe/Istanbul /etc/localtime \
    && echo Europe/Istanbul > /etc/timezone && export TZ=Europe/Istanbul && export SCHEDULER_TIMEZONE=Europe/Istanbul\
    && python -m pip install --upgrade pip \
    && pip install -r requirements.txt \
    && pip install --upgrade pymysql  
    
CMD ["pip", "list"]