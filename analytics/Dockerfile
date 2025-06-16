# Use an official Python base image
FROM python:3.8.0-slim

# Set working directory inside the container to match app structure
WORKDIR /workspace/analytics

# Install system dependencies required to build PostgreSQL-related packages
RUN apt-get update -y && \
    apt-get install -y --no-install-recommends \
        build-essential \
        libpq-dev \
        curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Copy the requirements file first (for layer caching)
COPY requirements.txt .

# Upgrade pip and install Python dependencies
RUN pip install --upgrade pip setuptools wheel && \
    pip install -r requirements.txt

# ⚠️ Patch Werkzeug's urls.py to define url_quote manually
RUN sed -i 's/from urllib.parse import quote/from urllib.parse import quote as url_quote/g' /usr/local/lib/python3.8/site-packages/werkzeug/urls.py || true


# Copy the entire workspace
COPY . .

# Set default environment variables (can be overridden at runtime)
ENV DB_USERNAME=myuser
ENV DB_PASSWORD=mypassword
ENV DB_HOST=127.0.0.1
ENV DB_PORT=5433
ENV DB_NAME=mydatabase


# Default command to run your Python app
CMD ["python", "app.py"]
