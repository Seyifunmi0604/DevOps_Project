## PROJECT8:LOAD BALANCER SOLUTION WITH APACHE

After completing **Project-7** you might wonder how a user will be accessing each of the webservers using 3 different IP addresses or 3 different DNS names. You might also wonder what is the point of having 3 different servers doing exactly the same thing.

When we access a website on the Internet we use an [URL](https://en.wikipedia.org/wiki/URL) and we do not really know how many servers are out there serving our requests, this complexity is hidden from a regular user, but in the case of websites that are being visited by millions of users per day (like Google or Reddit) it is impossible to serve all the users from a single Web Server (it is also applicable to databases, but for now we will not focus on distributed DBs).

Each URL contains a [domain name](https://en.wikipedia.org/wiki/Domain_name) part, which is translated (resolved) to the IP address of a target server that will serve requests when opening a website on the Internet. Translation (resolution) of domain names is performed by [DNS servers](https://en.wikipedia.org/wiki/Domain_Name_System), the most commonly used one has a public IP address 8.8.8.8 and belongs to Google. You can try to query it with [nslookup](https://en.wikipedia.org/wiki/Nslookup) command:

```
nslookup 8.8.8.8
Server:  UnKnown
Address:  103.86.99.99

Name:    dns.google
Address:  8.8.8.8
```
When you have just one Web server and load increases – you want to serve more and more customers, you can add more CPU and RAM or completely replace the server with a more powerful one – this is called "vertical scaling". This approach has limitations – at some point, you reach the maximum capacity of CPU and RAM that can be installed into your server.

Another approach used to cater for increased traffic is "horizontal scaling" – distributing the load across multiple Web servers. This approach is much more common and can be applied almost seamlessly and almost infinitely (you can imagine how many server Google has to serve billions of search requests).

Horizontal scaling allows one to adapt to the current load by adding (scale out) or removing (scale in) Web servers. Adjustment of a number of servers can be done manually or automatically (for example, based on some monitored metrics like CPU and Memory load).

The property of a system (in our case it is Web tier) to be able to handle the growing load by adding resources, is called ["Scalability"](https://en.wikipedia.org/wiki/Scalability).
In our setup in Project-7, we had 3 Web Servers and each of them had its own public IP address and public DNS name. A client has to access them by using different URLs, which is not a nice user experience to remember the addresses/names of even 3 servers, let alone millions of [Google servers](https://en.wikipedia.org/wiki/Google_data_centers).

In order to hide all this complexity and to have a single point of access with a single public IP address/name, a Load Balancer can be used. A [Load Balancer](https://en.wikipedia.org/wiki/Load_balancing_(computing)) (LB) distributes clients’ requests among underlying Web Servers and makes sure that the load is distributed in an optimal way.

# Side Self-Study:
Read about different Load [Balancing](https://www.nginx.com/resources/glossary/load-balancing/) concepts and differences between [L4 Network LB](https://www.nginx.com/resources/glossary/layer-4-load-balancing/) and [L7 Application LB](https://www.nginx.com/resources/glossary/layer-7-load-balancing/).
Let us take a look at the updated solution architecture with an LB added on top of Web Servers (for simplicity let us assume it is a software L7 Application LB, for example – [Apache](https://httpd.apache.org/docs/2.4/mod/mod_proxy_balancer.html), [NGINX](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/) or [HAProxy](http://www.haproxy.org/)

![8 1](https://github.com/Seyifunmi0604/DevOps_Project/assets/130314772/d23d2f70-cff9-4faa-bb57-3761e91252ff)

































