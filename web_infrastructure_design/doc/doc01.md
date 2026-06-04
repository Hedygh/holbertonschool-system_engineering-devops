# 1 - Distributed Web Infrastructure

## Diagram Summary

This infrastructure hosts `www.foobar.com` using three servers.

Components:

* 1 Load Balancer HAProxy
* 2 Web/Application Servers
* 1 Primary Database
* 1 Replica Database

Request flow:

1. The user enters `www.foobar.com`.
2. DNS resolves the domain name to the Load Balancer IP address.
3. HAProxy receives the request.
4. HAProxy forwards the request to Server 1 or Server 2.
5. Nginx receives the request.
6. The application server executes the application code.
7. The application queries the database.
8. The response is returned to the user.

## New Components Added

### Load Balancer HAProxy

Role:

* Distributes incoming requests across multiple servers.
* Improves performance.
* Improves availability.
* Prevents a single server from handling all traffic.

Why add it?

The single server architecture from Task 0 cannot efficiently handle high traffic.

### Second Web/Application Server

Role:

* Shares workload with Server 1.
* Provides redundancy.
* Improves scalability.

Why add it?

If one server becomes overloaded or fails, the second server can continue serving requests.

### Replica Database

Role:

* Maintains a copy of the Primary database.
* Can be used for read operations.
* Provides data redundancy.

Why add it?

To reduce database workload and improve availability.

## Load Balancing Algorithm

### Round Robin

HAProxy is commonly configured with the Round Robin algorithm.

How it works:

Request 1 → Server 1

Request 2 → Server 2

Request 3 → Server 1

Request 4 → Server 2

The load balancer distributes requests evenly among available servers.

## Active-Active vs Active-Passive

### Active-Active

Both servers actively handle incoming traffic.

Example:

* Server 1 handles requests.
* Server 2 handles requests.

Advantages:

* Better performance.
* Better resource utilization.

This infrastructure uses an Active-Active setup.

### Active-Passive

One server handles traffic while the other remains on standby.

Example:

* Server 1 handles requests.
* Server 2 waits for a failure.

Advantages:

* Simpler failover.

Disadvantage:

* One server remains unused most of the time.

## Primary-Replica Database Cluster

### Primary Database

Responsibilities:

* Handles READ operations.
* Handles WRITE operations.
* Receives all modifications.

Examples:

* INSERT
* UPDATE
* DELETE

### Replica Database

Responsibilities:

* Receives replicated data from the Primary.
* Usually handles READ operations only.

The Replica automatically synchronizes with the Primary database.

## Difference Between Primary and Replica

Primary:

* Read operations
* Write operations

Replica:

* Read operations only
* Receives updates from the Primary

Important:

Applications should write data only to the Primary database.

## Remaining SPOF

Even though the infrastructure is more resilient, some Single Points Of Failure still exist.

### Load Balancer

There is only one HAProxy instance.

If HAProxy fails:

* Users cannot reach the application servers.

### Primary Database

There is only one Primary database.

If the Primary fails:

* Write operations stop working.

## Security Issues

### No Firewall

Problem:

* All services may be exposed directly to the internet.
* Increased attack surface.

Risk:

* Unauthorized access.
* Exploitation of vulnerable services.

### No HTTPS

Problem:

Data is transmitted in clear text.

Risk:

* Credentials can be intercepted.
* Sensitive information can be exposed.

HTTPS should be used to encrypt communications between client and server.

## Monitoring Issues

There is no monitoring system.

Consequences:

* No CPU monitoring.
* No memory monitoring.
* No disk monitoring.
* No traffic monitoring.
* No automatic alerts.

Administrators may only discover problems after users report them.

## Key Terms

* HAProxy: Load Balancer that distributes requests.
* Round Robin: Distribution algorithm that alternates requests between servers.
* Active-Active: Multiple servers actively handle traffic.
* Active-Passive: One active server and one standby server.
* Primary Database: Handles reads and writes.
* Replica Database: Receives replicated data and usually handles reads.
* Redundancy: Duplication of components to improve availability.
* Scalability: Ability to handle increasing traffic.
* SPOF: Single Point Of Failure.
* HTTPS: Encrypted communication protocol.
* Firewall: Security system controlling network access.
* Monitoring: Continuous observation of system health and performance.

## Common Oral Questions

Q: Why add a load balancer?

A: To distribute traffic across multiple servers and improve availability.

Q: Why add a second server?

A: To provide redundancy and support more traffic.

Q: What algorithm does the load balancer use?

A: Round Robin.

Q: What is the difference between Active-Active and Active-Passive?

A: In Active-Active, all servers handle traffic. In Active-Passive, one server handles traffic while the other waits for a failure.

Q: What is the difference between Primary and Replica databases?

A: The Primary handles reads and writes, while the Replica receives replicated data and is mainly used for reads.

Q: Does this infrastructure still have SPOFs?

A: Yes. The Load Balancer and the Primary Database are still SPOFs.

Q: What security problems remain?

A: No firewall and no HTTPS.

Q: What operational problem remains?

A: There is no monitoring system.
