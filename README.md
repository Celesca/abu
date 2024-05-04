# Use the official Jetson base image
FROM nvcr.io/nvidia/l4t-base:r32.6.1

# Install necessary dependencies
RUN apt-get update && \
    apt-get install -y \
    python3 \
    python3-pip \
    libopencv-dev \
    # Add any additional dependencies needed for your YOLO model here
    && rm -rf /var/lib/apt/lists/*

# Set up your working directory
WORKDIR /app

# Copy your YOLO model and Python script into the container
COPY best.pt .
COPY app.py .

# Install Python dependencies
RUN pip3 install numpy opencv-python

# Set the entry point for your container
CMD ["python3", "app.py"]

docker build -t yolo_app .
docker run --rm --runtime nvidia yolo_app
