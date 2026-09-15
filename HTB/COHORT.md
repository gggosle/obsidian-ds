SSRF Exploit - was found by random path enumeration on the website
Marimo Pre-Auth RCE (CVE-2026–39987)
SSRF bypass formatting
The subdomain name, which was responsible for the Marimo connection was found by looking at the /status path

The script used to connect via sockets is: 
SNI (Server Name Indication)
- Not sending the request to the DNS server, as both the ip and the domain name are hardcoded SNI (Server Name Indication)
- Not sending the request to get the public id of the SSL certificate in order to check it: sends the initial HTTP handshake request for the websocket connection right away
- Ignores the HTTP response and assuming the "Happy path" starts to send frames right away. 