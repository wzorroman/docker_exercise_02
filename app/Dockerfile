# pull official base image
FROM python:3.5

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1
ENV WEBAPP_DIR=/home/wilson/django-on-docker/app

# set work directory
#WORKDIR /usr/src/app
WORKDIR $WEBAPP_DIR

# install dependencies
RUN pip install --upgrade pip
#COPY ./requirements.txt /usr/src/app/requirements.txt
COPY ./requirements.txt $WEBAPP_DIR/requirements.txt
RUN pip install -r requirements.txt

# copy project
COPY . $WEBAPP_DIR/
