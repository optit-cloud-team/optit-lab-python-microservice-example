FROM python

WORKDIR /app

COPY ./consumer_two .
COPY ./repository .
RUN pip install -r consumer_two/requirements.txt

CMD [ "python", "item_creation.py" ]
