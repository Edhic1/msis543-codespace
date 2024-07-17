# Use Ubuntu as the base image
ARG VARIANT="jammy"
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

# Install required packages and dependencies
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive && apt-get install -y \
    libaio1 net-tools bc alien && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Download Oracle XE 21c
RUN curl -o oracle-xe-21c.rpm -L https://download.oracle.com/otn_software/linux/instantclient/216000/oracle-instantclient-basic-21.6.0.0.0-1.el8.x86_64.rpm

# Convert RPM to Debian package
RUN alien -d oracle-xe-21c.rpm && \
    dpkg -i oracle-instantclient-basic_21.6.0.0.0-2_amd64.deb && \
    rm oracle-xe-21c.rpm oracle-instantclient-basic_21.6.0.0.0-2_amd64.deb
    
# Set environment variables for Oracle
ENV ORACLE_HOME=/opt/oracle/product/21c/dbhomeXE \
    PATH=$PATH:/opt/oracle/product/21c/dbhomeXE/bin \
    ORACLE_SID=XE \
    ORACLE_PWD=oracle

# Expose ports
EXPOSE 1521 5500

# Start Oracle listener and database
CMD ["/opt/oracle/product/21c/dbhomeXE/bin/tnslsnr", "start"]
