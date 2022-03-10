
## Vagrant: HAProxy + Consul + Consul Template (scalable web application example)

- Vagrant for launching multiple VM instances and persistent storage
- Consul for health checking and service discovery
- Consul Template for automated load balancer management
- HAProxy for  HTTP server load balancing (or TCP : MySQL/etc. service LB)

### Prerequisites
Vagrant 1.8.1+: http://www.vagrantup.com/
VirtualBox: https://www.virtualbox.org/

### Usage
```
 $ vagrant up
 $ vagrant status
Current machine states:

consulserver1.local       running (virtualbox)
web-lb.local              running (virtualbox)
web1.local                running (virtualbox)
web2.local                running (virtualbox)
 
```   
After vagrant machines are running, you can connect instances to:

Consul WEB UI: 192.168.100.11:8500
Web Load Balancing Machine: 192.168.100.20

If having the error message 'The guest additions on this VM do not match the install version of VirtualBox!', then run following command before vagrant up
```
$ vagrant plugin install vagrant-vbguest
```

### Features
- Launch scalable architecture in single command
- Used Consul to manage server nodes, service discovery and health checking
- Configured web server load balancing with Consul Template + HAProxy

Once all vagrant instances up, you can access Consul Web UI by opening browser http://192.168.100.11:8500. Then you will see services like consul, web-lb, web. In Nodes section, you will see nodes like consul1, web-lb, web1 and so on. If you see services and nodes as following screenshot, then it is successfully up and running.

![picture](pictures/consul-services.png)
![picture](pictures/consul-nodes.png)

HAProxy: Open browser and access to http://192.168.100.20.

![picture](pictures/haproxy-stats.png)
