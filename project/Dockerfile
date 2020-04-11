FROM python:3.8
WORKDIR /code/app
CMD ["gunicorn", "-b :8000", "conf.wsgi"]

COPY requirements.txt /code

RUN apt-get update \
	&& apt-get -y install python3-dev \
	&& pip install --no-cache-dir -r /code/requirements.txt \
	&& apt-get -y purge python3-dev \
	&& apt-get -y autoremove

COPY ./app /code/app
