FROM python

WORKDIR /app

# Copy the requirements.txt file into the Docker image
COPY requirements.txt .

# Install dependencies
RUN pip install -r requirements.txt

# Copy the rest of the application code
COPY . .

# Specify the command to run the application
CMD [ "python", "healthcheck.py" ]
