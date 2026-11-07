# VPN lab

## Part 1
My HTTP server used port 43885 and listened on 0.0.0.0.

Firefox: Opening http://212.34.250.14:43885/ displayed my webpage
and username.

Server curl to 127.0.0.1:43885: returned 200 OK and my HTML.
Server curl to 212.34.250.14:43885: returned 200 OK and my HTML.
Windows curl to 212.34.250.14:43885: returned 200 OK and my HTML.
## Part 2
After binding the HTTP server to 127.0.0.1, the server-side curl
request to 127.0.0.1:43885 returned 200 OK and my webpage.
The request to 212.34.250.14:43885 failed to connect.
The ss command showed that the server listened on 127.0.0.1:43885.

This failure was casued because the server was listening only on its
loopback address, not its public IP address.

Binding to 0.0.0.0 means listening on all local IPv4 interfaces.
Binding to 127.0.0.1 allows connections only through the server's
loopback interface.

This does not restrict access to my Unix account. Other users
logged into the same server can also connect to this service.
## Part 3
Before starting the SSH tunnel, http://127.0.0.1:8080/ did
not load. While the tunnel was running, it displayed my webpage.
After stopping the tunnel and forcing a reload, it failed again.

Port 8080 listened on my Windows computer. Python's HTTP port
43885 listened on the course server.

The second 127.0.0.1 in the -L argument refers to the course
server's loopback address.

The browser connected to local port 8080. SSH carried the
connection through the encrypted tunnel, and the remote SSH
server connected to 127.0.0.1:43885 on the course server.
This allowed access to the loopback-only HTTP service.
## Part 4
With the SOCKS proxy enabled in Firefox, ip.me displayed
2001:470:22c2:250:7468:6568:6c6c:1. This is an IPv6 address;
the proxy's outgoing address perhaps does not have to match the
course server's IPv4 address, 212.34.250.14.

My og IP was 217.113.17.247


Port 1080 listened on my Windows computer. Firefox connected
to this local SOCKS proxy, and SSH forwarded its connections
through the course server.

Firefox's proxy settings affect Firefox, not every application
on my computer. Other applications must separately use the proxy.

SSH encrypts traffic between my computer and the course server.
From the course server to the website, HTTPS provides encryption;
plain HTTP does not.

SOCKS forwards connections for applications configured to use it.
WireGuard carries IP packets through a virtual network interface,
with routing determining which traffic uses the VPN.

Yggdrasil provides an encrypted IPv6 overlay between participating
nodes. It does not automatically provide a public-web exit proxy.
