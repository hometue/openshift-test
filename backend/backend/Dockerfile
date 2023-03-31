# syntax=docker/dockerfile:1

FROM python:3.11

RUN pip install django
RUN pip install mysqlclient
RUN pip install djangorestframework
RUN pip install markdown
RUN pip install django-filter
RUN pip install django-cors-headers
WORKDIR /src
COPY ./ ./
CMD python manage.py runserver 0.0.0.0:8000
EXPOSE 8000