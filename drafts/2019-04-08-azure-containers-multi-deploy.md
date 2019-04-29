---
layout: single
classes: wide
title: "How to - Deploying multi container instances to Azure container services"
author_profile: true
catergories:
	- Docker
	- Containers
tags:
    - Handy
	- YAML
---

Azure Container instances can be a great way to deploy Docker based containers on the cheap [link](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-quickstart?WT.mc_id=UI_empg). It's agreat way to run images without any overhead and without the costs of maintanance. Now the GUI of Azure is always a bit lagging in terms of funconality compared to the CLI/Powershell options. And the one thing that I want to do (deploy a set of containers) isn't possible at the moment when using the GUI. So go go CLI!

## Objectiefs
* Create compose file with all necesery settings
* Deploy containergroup to Azure using CLI
* ?????????
* Profit

For this case I'm using WordPress as an example. Now deploying WordPress to Azure Web Apps might be easier but the cost's of Azure MySql (as a service) are (in my opinion) not worth it for this testing enviorment. 

## Setting up the containers
For this tutorial I'm using the info [availabile on MS Docs](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-multi-container-yaml). I'm also using parts of the template they provide. So let's get this started. We are going to create a container group of 2 containers. The first one will be running WordPress and the second one will be running MySql

~~~YAML
apiVersion: 2018-10-01
location: westeurope
name: WordPressGroup
properties:
  containers: 
  - name: WordPressWeb
    properties:
      image: wordpress
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
	  - port: 8080


  - name: WordPressDB
    properties:
      image: mysql:5.7
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
  osType: Linux
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
    - protocol: tcp
	  port: '8080'
	args:
	- MYSQL_ROOT_PASSWORD: my-secret-pw

tags: "WordPress"
type: Microsoft.ContainerInstance/containerGroups
~~~