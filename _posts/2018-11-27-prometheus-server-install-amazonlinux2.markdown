---
layout: post
title:  "Install Prometheus Server in Amazon Linux 2"
date:   2018-11-27 21:00:00 +0800
categories: jekyll update
---

First download Prometheus from release page: https://github.com/prometheus/prometheus/releases
Here we use v2.5.0 for example

    $ mkdir ~/downloads
    $ cd downloads/
    $ wget "https://github.com/prometheus/prometheus/releases/download/v2.5.0/prometheus-2.5.0.linux-amd64.tar.gz"

Then decompress and install under /usr/local/, 

    $ cd /usr/local/
    $ mkdir prometheus
    $ cd prometheus/
    $ tar -xvzf ~/downloads/prometheus-2.5.0.linux-amd64.tar.gz
    $ mv prometheus-2.5.0.linux-amd64.tar.gz/ prometheus

Then we need to create service file for prometheus:

    sudo nano /etc/systemd/system/prometheus.service

The content should be:

    [Unit]
    Description=prometheus_server
    [Service]
    Type=simple
    ExecStart=/usr/local/prometheus/prometheus/prometheus --config.file /usr/local/prometheus/prometheus/prometheus.yml
    [Install]
    WantedBy=multi-user.target

Then reload daemon, start the service and check. If everything went well, you should be able to visit your Prometheus server at http://<serverIP>:9090/

    sudo systemctl reload daemon
    sudo systemctl start prometheus
    sudo systemctl status prometheus








