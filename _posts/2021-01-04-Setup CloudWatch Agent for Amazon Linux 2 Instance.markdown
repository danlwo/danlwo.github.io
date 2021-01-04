---
layout: post
title:  "Setup CloudWatch Agent for Amazon Linux 2"
date:   2021-01-04 13:00:00 +0800
categories: jekyll update
---

# Setup CloudWatch Agent for Amazon Linux 2 Instance

## IAM Permissions

Create a role (named 'CloudWatchAgentServerRole' here) with the following policies:

* CloudWatchAgentServerPolicy
* AmazonEC2RoleforSSM
* AmazonSSMManagedInstanceCore
* AWSNetworkManagerFullAccess

Then attach the role to the target instance.
***

## Install CloudWatch Agent

Update AWS CLI & SSM & etc.

    $ sudo yum update --skip-broken

If encoutering "Disk Requirements" error
> At least XXXMB more space needed on the /boot filesystem.

Delete some outdated kernal for space:

    $ rpm -qa | grep kernel
    $ sudo rpm -e kernel-2.6.32-220.23.1.el6.x86_64 kernel-2.6.32-220.el6.x86_64
    $ sudo yum update kernel

Make sure AWS SSM is running with latest version:

    $ sudo systemctl restart amazon-ssm-agent -l
    $ sudo systemctl status amazon-ssm-agent -l

Install CloudWatch Agent with yum:

    $ sudo yum install amazon-cloudwatch-agent
***

## Config CloudWatch Agent

Use built-in wizard to create config.json:

    $ sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard

Start CloudWatch Agent service and check service status:

    $ sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a start
    $ sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status

Or:

    $ sudo systemctl restart amazon-cloudwatch-agent.service -l
    $ sudo systemctl status amazon-cloudwatch-agent.service -l
***
