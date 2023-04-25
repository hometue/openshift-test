# syntax=docker/dockerfile:1

ARG UID=101
ARG GID=101

FROM python:3.11

ARG MYSQL_USERNAME
ARG MYSQL_PASSWORD
ARG MYSQL_HOSTNAME
ARG MYSQL_DB
ARG MYSQL_PORT

RUN set -x \
    && addgroup --system --gid $GID djangouser || true \
    && adduser --system --disabled-login --ingroup djangouser --no-create-home --home /nonexistent --gecos "djangouser user" --shell /bin/false --uid $UID djangouser || true

RUN pip install django
RUN pip install mysqlclient
RUN pip install djangorestframework
RUN pip install markdown
RUN pip install django-filter
RUN pip install django-cors-headers
WORKDIR /src
COPY ./ ./
RUN export 
RUN python manage.py migrate 
RUN chmod 777 .
USER $UID
CMD python manage.py runserver 0.0.0.0:8000
EXPOSE 8000