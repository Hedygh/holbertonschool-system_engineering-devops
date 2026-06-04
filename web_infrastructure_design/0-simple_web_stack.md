# 0 - Simple Web Stack

## Diagram Summary

This infrastructure hosts the website `www.foobar.com` on a single server with the IP address `8.8.8.8`.

When a user enters `www.foobar.com` in a browser:

1. The DNS resolves `www.foobar.com` to `8.8.8.8`.
2. The request reaches the server.
3. Nginx receives the HTTP request.
4. Nginx forwards the request to the application server.
5. The application server executes the application code.
6. If data is needed, the application server queries the MySQL database.
7. The response is returned to the user.

## Infrastructure Components

### Server

A server is a computer that provides services or resources to other computers over a network.

In this infrastructure, the server hosts:

* Nginx web server
* Application server
* Application files
* MySQL database

### Domain Name

The domain name is `foobar.com`.

Its role is to provide a human-readable name instead of requiring users to remember an IP address.

Example:

`www.foobar.com` → `8.8.8.8`

### DNS Record

The `www` record is an **A Record**.

Its role is to map a domain name to an IPv4 address.

Example:

`www.foobar.com` → `8.8.8.8`

### Web Server Nginx

The web server receives incoming HTTP requests from users.

Its responsibilities include:

* Accepting client connections
* Serving static files
* Forwarding requests to the application server

### Application Server

The application server executes the application logic.

Its responsibilities include:

* Processing requests
* Running application code
* Communicating with the database
* Generating responses

### Application Files

The application files contain the website source code and business logic executed by the application server.

### Database MySQL

The database stores and manages application data.

Examples:

* User accounts
* Products
* Articles
* Comments

The application server queries the database when information is needed.

### Communication Protocol

The server communicates with the user's computer using the HTTP/HTTPS protocol over TCP/IP.

## Infrastructure Issues

### SPOF Single Point Of Failure

This infrastructure contains several SPOFs.

Because there is only one server:

* If the server fails, the entire website becomes unavailable.
* If Nginx fails, the website becomes unavailable.
* If the application server fails, the website becomes unavailable.
* If MySQL fails, the website becomes unavailable.

### Downtime During Maintenance

When deploying new code or restarting services:

* The application server may need to restart.
* Nginx may need to restart.
* Users may temporarily lose access to the website.

This causes downtime.

### Cannot Scale

The infrastructure uses only one server.

If traffic increases significantly:

* CPU usage increases
* Memory usage increases
* Response times become slower
* The server may become overloaded

The infrastructure cannot handle a large amount of traffic without adding additional servers and load balancing.

## Key Terms

* Server: A computer that provides services to clients.
* Domain Name: Human-readable address of a website.
* A Record: DNS record that maps a domain name to an IPv4 address.
* Nginx: Web server.
* Application Server: Executes application logic.
* MySQL: Database management system.
* SPOF: Single Point Of Failure.
* HTTP/HTTPS: Communication protocols used between client and server.
