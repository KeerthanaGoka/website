# Use the official Python base image
FROM python:3.10-slim

# Set the working directory
WORKDIR /code

# Copy requirements file and install dependencies
COPY requirements.txt /code/requirements.txt

RUN pip install --no-cache-dir -r /code/requirements.txt

# Copy your FastAPI application code
COPY ./app /code/app

# Command to run the FastAPI app with live reload disabled for production
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "80"]

#docker build -t fastapi-app .
