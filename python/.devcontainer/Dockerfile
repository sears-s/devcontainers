FROM python:slim

# Install packages
COPY apt_packages.txt /
RUN apt-get update && apt-get install -y $(cat /apt_packages.txt) && rm -rf /var/lib/apt/lists/*

# Install Python modules
COPY python_modules.txt /
RUN pip install --no-cache-dir -r /python_modules.txt
