FROM rocker/rstudio:3.5.3

USER root


# R pre-requisites
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    fonts-dejavu \
    gfortran \
    unixodbc-dev \
    gnupg2 \
    curl \
    libssl1.0.0 \
    gcc \
    apt-transport-https \
    ca-certificates \
    apt-utils \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update -qq && \
  apt-get -y --no-install-recommends install \
  libxml2-dev \
  libcairo2-dev \
  libsqlite-dev \
  libmariadbd-dev \
  libmariadb-client-lgpl-dev \
  libpq-dev \
  libssh2-1-dev

RUN install2.r --error \
    --deps TRUE \
    tidyverse \
    dplyr \
    devtools \
    formatR \
    remotes \
    selectr \
    BiocManager
RUN install2.r --error \
    --deps TRUE \
    #ggplot2 \
    #igraph \
    odbc \
    MarketMatching

# Install MSServer ODBC driver
RUN sudo su && \
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
#Download appropriate package for the OS version
#Debian 9
curl https://packages.microsoft.com/config/debian/9/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
#exit && \
sudo apt-get update && \
sudo ACCEPT_EULA=Y apt-get -y --no-install-recommends install msodbcsql17 && \
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install -y --no-install-recommends mssql-tools && \
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile && \
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
SHELL ["/bin/bash", "-c", "source ~/.bashrc"]

#source ~/.bashrc && \
# optional: for unixODBC development headers
#RUN sudo apt-get install unixodbc-dev
