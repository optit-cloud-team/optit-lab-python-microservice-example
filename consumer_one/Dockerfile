FROM python

WORKDIR /app

# Copy the requirements.txt file into the Docker image
COPY ./consumer_one .


# Install dependencies
RUN pip install -r consumer_one/requirements.txt

# Copy the rest of the application code


# Specify the command to run the application
CMD [ "python", "healthcheck.py" ]
