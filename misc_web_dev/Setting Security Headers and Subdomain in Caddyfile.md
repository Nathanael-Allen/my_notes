After some frustration with Caddy documentation I was able to set the required headers for security and also redirect the www. subdomain to the main site. 
This is what the Caddyfile looks like for coreassocations.com
```yml
coreassociations.com {
        reverse_proxy localhost:PORT
        header {
                +Strict-Transport-Security max-age=31536000; includeSubDomains
                +Permissions-Policy geolocation(self https://coreassociations.com/), camera=()
				+Content-Security-Policy "default-src 'self'; script-src 'self' https://unpkg.com/; font-src 'self' https://fonts.googleapis.com/ https://fonts.gstatic.com/; style-src 'self' https://fonts.googleapis.com/ https://fonts.gstatic.com/; style-src-elem 'self' https://fonts.googleapis.com/ https://fonts.gstatic.com/"
                X-Frame-Options SAMEORIGIN
                X-Content-Type-Options nosniff
                Referrer-Policy same-origin
        }
}
www.coreassociations.com {
        redir https://coreassociations.com{uri}
}
```
In the header setting you need to append a `+` to the response headers that are not sent automatically. The `+` ensures they are added. The second block of the file handles redirecting from the `www.example.com` subdomain to our actual domain name.

## CSP
Handling the Content-Security-Policy headers was a bit of nightmare at first but it's not as complicated as I initially thought. For me I found most things should just be 'self'. When set to self anything coming from your own URL will be allowed. However if you are using a google font or a library from a cdn you need to pass those urls into your csp header.