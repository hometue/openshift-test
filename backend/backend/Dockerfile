# syntax=docker/dockerfile:1

ARG UID=101
ARG GID=101

FROM python:3.11

RUN set -x \
    && addgroup --system --gid $GID djangouser || true \
    && adduser --system --disabled-login --ingroup nginx --no-create-home --home /nonexistent --gecos "nginx user" --shell /bin/false --uid $UID djangouser || true \

RUN pip install django
RUN pip install mysqlclient
RUN pip install djangorestframework
RUN pip install markdown
RUN pip install django-filter
RUN pip install django-cors-headers
WORKDIR /src
COPY ./ ./
RUN python manage.py migrate
RUN chmod 777 .
CMD python manage.py runserver 0.0.0.0:8000
EXPOSE 8000