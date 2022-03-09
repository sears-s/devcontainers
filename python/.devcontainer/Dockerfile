FROM python:latest

# Install packages
COPY apt_packages.txt /tmp
RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y $(cat /tmp/apt_packages.txt) \
    && rm -rf /var/lib/apt/lists/* /tmp/apt_packages.txt

# Install Python modules
COPY python_modules.txt /tmp
RUN pip install --no-cache-dir -r /tmp/python_modules.txt \
    && rm /tmp/python_modules.txt
