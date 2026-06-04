# 2 - Secured and Monitored Web Infrastructure

## Diagram Summary

This infrastructure hosts `www.foobar.com` using:

* 1 Load Balancer HAProxy
* 2 Web/Application Servers
* 1 MySQL Primary Database
* 1 MySQL Replica Database
* 3 Firewalls
* 1 SSL Certificate
* 3 Monitoring Clients

The infrastructure is now:

* Distributed
* Secured
* Encrypted
* Monitored

## Request Flow

1. The user enters `www.foobar.com` in a web browser.
2. DNS resolves the domain name to the Load Balancer IP address.
3. The request is sent over HTTPS.
4. The firewall filters incoming traffic.
5. HAProxy receives the request.
6. HAProxy distributes the request to one of the web servers using Round Robin.
7. Nginx receives the request.
8. The application server executes the application code.
9. The application queries the MySQL database.
10. The response is sent back to the user.

## New Components Added

### Firewalls

Role:

* Filter network traffic.
* Allow authorized connections.
* Block unauthorized connections.

Why add them?

To improve security and reduce exposure to attacks.

Example:

Allow:

* Port 443 HTTPS

Block:

* Unnecessary services and ports

### SSL Certificate

Role:

Allows the website to use HTTPS.

Why add it?

To encrypt communications between users and the infrastructure.

Benefits:

* Protects passwords.
* Protects cookies.
* Protects personal information.
* Prevents eavesdropping.

### Monitoring Clients

Role:

Collect metrics and logs from servers.

Why add them?

To monitor infrastructure health and detect problems.

Examples of collected data:

* CPU usage
* Memory usage
* Disk usage
* Network traffic
* Service status
* Application logs

## Firewalls

A firewall is a security device or software that controls incoming and outgoing network traffic.

It acts as a security guard between systems.

Responsibilities:

* Allow legitimate traffic.
* Block malicious or unauthorized traffic.

Without a firewall:

* Services may be exposed directly to the Internet.
* Attackers have more opportunities to access systems.

## HTTPS

HTTPS is HTTP over SSL/TLS encryption.

Role:

Encrypts communication between the user's browser and the infrastructure.

Without HTTPS:

Data is transmitted in plain text.

Examples:

* Usernames
* Passwords
* Session cookies

With HTTPS:

Data is encrypted and cannot easily be read by attackers.

## Monitoring

Monitoring is used to observe the health and performance of the infrastructure.

Monitoring helps administrators:

* Detect failures.
* Detect performance issues.
* Detect resource exhaustion.
* Analyze trends.

Examples:

* Server down
* High CPU usage
* High memory consumption
* Large traffic spikes

## How Monitoring Collects Data

A monitoring agent is installed on each server.

The monitoring client:

1. Collects system metrics.
2. Collects service metrics.
3. Collects logs.
4. Sends data to a monitoring platform.

Examples:

* Sumologic
* Datadog
* Prometheus
* Grafana

## Monitoring Web Server QPS

QPS means:

Queries Per Second

To monitor QPS:

* Collect request metrics from Nginx or HAProxy.
* Count the number of incoming requests.
* Display the result in the monitoring platform.

Example:

If the server receives:

100 requests in 1 second

Then:

QPS = 100

QPS is useful for measuring traffic load.

## Remaining Issues

### SSL Termination at Load Balancer

Problem:

The SSL connection ends at HAProxy.

Example:

User → HAProxy = HTTPS

HAProxy → Backend Servers = HTTP

Consequences:

* Internal traffic is no longer encrypted.
* Data could be exposed if the internal network is compromised.

### Only One MySQL Server Accepts Writes

Problem:

Only the Primary database accepts write operations.

Examples:

* INSERT
* UPDATE
* DELETE

If the Primary fails:

* Write operations stop.
* Users cannot create or modify data.

The Replica cannot replace the Primary automatically.

The Primary remains a critical component.

### Same Components on Every Server

Problem:

Each server contains:

* Nginx
* Application Server
* Application Files
* Database

Consequences:

* Resource contention.
* More difficult maintenance.
* More difficult scaling.
* Larger attack surface.

Example:

A database consuming excessive RAM can negatively impact the web server and application server on the same machine.

## Key Terms

* Firewall: Filters network traffic.
* HTTPS: Encrypted HTTP communication.
* SSL Certificate: Enables HTTPS.
* Monitoring: Continuous observation of infrastructure health.
* Monitoring Client: Agent collecting metrics and logs.
* QPS: Queries Per Second.
* HAProxy: Load Balancer.
* Primary Database: Handles reads and writes.
* Replica Database: Receives replicated data and usually handles reads.
* SSL Termination: SSL encryption ends at the load balancer.

## Common Oral Questions

Q: Why add firewalls?

A: To control network access and block unauthorized traffic.

Q: Why use HTTPS?

A: To encrypt communications between users and servers.

Q: Why add monitoring?

A: To detect failures and monitor system performance.

Q: How does monitoring collect data?

A: Monitoring agents installed on servers collect metrics and logs and send them to a monitoring platform.

Q: What is QPS?

A: Queries Per Second, the number of requests handled each second.

Q: Why is SSL termination at the load balancer a problem?

A: Traffic may be unencrypted between the load balancer and backend servers.

Q: Why is the Primary database still a problem?

A: It is the only server that accepts writes, making it a critical point of failure.

Q: Why is having all components on every server a problem?

A: It makes maintenance, scaling, resource management, and security more difficult.

## Short Explanation

This infrastructure hosts `www.foobar.com` using a load balancer, two web/application servers, and a Primary-Replica MySQL database setup.

Traffic is protected by firewalls and encrypted using HTTPS with an SSL certificate.

HAProxy distributes incoming requests between the two servers using the Round Robin algorithm.

Monitoring agents are installed on all servers to collect metrics and logs and send them to a monitoring platform.

Although the infrastructure is more secure and reliable, some issues remain, including SSL termination at the load balancer, a single writable database, and the fact that each server contains multiple critical components.
