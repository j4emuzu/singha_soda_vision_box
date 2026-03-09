# Build stage to install dependencies
FROM python:3.10-slim AS builder
WORKDIR /app
RUN pip install --no-cache-dir ultralytics fastapi uvicorn python-multipart pillow

FROM python:3.10-slim
WORKDIR /app

# Install minimal system dependencies for OpenCV
RUN apt-get update && apt-get install -y \
    libgl1-mesa-glx \
    libglib2.0-0 \
    && rm -rf /var/lib/apt/lists/*

# Copy installed libraries from the builder stage
COPY --from=builder /usr/local/lib/python3.10/site-packages /usr/local/lib/python3.10/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin

# Embed model and source code for immediate functionality
COPY ai/best.pt /app/best.pt
COPY ai/main.py /app/main.py

# Expose API port
EXPOSE 8000

# Start the application
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]