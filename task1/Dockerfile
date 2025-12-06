FROM ubuntu:latest

# Update & install Python + pip
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y python3 python3-pip

# Fix "externally managed" error
RUN rm -f /usr/lib/python3.*/EXTERNALLY-MANAGED

# Install Flask
RUN pip3 install flask

# Create /app inside the container and go there
WORKDIR /app

# Copy your api.py into the container
COPY api.py .

# Expose the port
EXPOSE 5252

# THIS IS THE ONLY COMMAND THAT WORKS 100% OF THE TIME
CMD ["python3", "api.py"]
