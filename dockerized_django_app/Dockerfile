# Use an official lightweight Python image.
# https://hub.docker.com/_/python
FROM python:3.9-slim

ENV APP_HOME /app
WORKDIR $APP_HOME

RUN apt-get update \
  && apt-get upgrade \
  && apt-get install -y nginx \
  && apt-get install -y supervisor

# Install dependencies.
COPY requirements.txt .

RUN pip install -U pip && pip install -r requirements.txt

# Copy local code to the container image.
COPY . .

# Service must listen to $PORT environment variable.
# This default value facilitates local development.
ENV PORT 8000

# Setting this ensures print statements and log messages
# promptly appear in Cloud Logging.
ENV PYTHONUNBUFFERED TRUE


# Run the web service on container startup. Here we use the gunicorn
# webserver, with one worker process and 8 threads.
# For environments with multiple CPU cores, increase the number of workers
# to be equal to the cores available.
CMD exec gunicorn --bind 0.0.0.0:$PORT --workers 3 --threads 8 --timeout 120 --preload dockerized_django_app.wsgi