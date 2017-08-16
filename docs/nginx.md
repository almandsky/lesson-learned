# How to redirect the www domain name to non-www domain name in SSL

## When there is no SSL redirect in Nginx

If there is no SSL setup in your Nginx server rules, the change is easy.  First, need to make sure you have both www and non-www DNS A record setup in your DNS provider.

Then you can just add the following rules to redirect the www domain to the non-www domain.  I am refering the example in [How To Redirect www to Non-www with Nginx on CentOS 7 ](https://www.digitalocean.com/community/tutorials/how-to-redirect-www-to-non-www-with-nginx-on-centos-7)

```
server {
    server_name www.example.com;
    return 301 $scheme://example.com$request_uri;
}
```


## When there is SSL redirect in Nginx

Because I have already setup the SSL cert for my Nginx server, so the above rule does not work.  

As per the [StackOverflow post](https://stackoverflow.com/questions/7947030/nginx-no-www-to-www-and-www-to-no-www), we need to redirect both, non-SSL and SSL to their non-www counterpart:

```
server {
    listen               80;
    listen               443 ssl;
    server_name          www.example.com;
    ssl_certificate      path/to/cert;
    ssl_certificate_key  path/to/key;

    return 301 $scheme://example.com$request_uri;
}

server {
    listen               80;
    listen               443 ssl;
    server_name          example.com;
    ssl_certificate      path/to/cert;
    ssl_certificate_key  path/to/key;

    # rest goes here...
}

```

For my case, because I have the http2 enabled, so the rules will be:

```
server {
    server_name          www.example.com;
    ssl_certificate      path/to/cert;
    ssl_certificate_key  path/to/key;
    listen               *:80;
    listen               *:443 ssl http2;
    listen               [::]:80;
    listen               [::]:443 ssl http2;

    return 301 https://example.com$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name          example.com;
    ssl_certificate      path/to/cert;
    ssl_certificate_key  path/to/key;

    # rest goes here...
}

server {
        listen 80;
        listen [::]:80;
        server_name    example.com;
        return         301 https://example.com$request_uri;
}

```