First I install Caddy using Caddy's install method for Ubuntu [here](https://caddyserver.com/docs/install#debian-ubuntu-raspbian) then I can create a Caddyfile and run Caddy as a reverse proxy. A simple Caddyfile for that would be like this:
```yml
example.com {
	reverse_proxy localhost:PORT_NUMBER
}
```
Then set up [[Setting Security Headers and Subdomain in Caddyfile |security headers and subdomains]] 