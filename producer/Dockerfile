FROM python

WORKDIR /app

COPY ./producer .
COPY ./producer/requirements.txt .
COPY ./repository .

RUN pip install -r requirements.txt

CMD [ "python", "producer.py" ]
