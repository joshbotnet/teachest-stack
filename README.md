# TeaChest Stack

<p><img src="./static/teachest.png" width="220px" /></p>

The TeaChest Stack contains 3 related projects:

- `Express Server`
- - http://teachest.io
- - https://github.com/joshbotnet/teachest-express.git
- `VueJS App`
- - http://tealight.io
- - https://github.com/joshbotnet/teachest-vuejs.git
- `GraphQL Server`
- - http://mainbox.io/graphiql
- - - eg. http://mainbox.io/graphiql?query=%7B%0A%20%20posts%20%7B%0A%20%20%20%20id%0A%20%20%7D%0A%7D
- - https://github.com/joshbotnet/teachest-graphql.git

Example nginx config for all 3 projects proxied on port 80 locally:

```
server {
    listen 80;
    server_name teachest.io;
    location / {
        proxy_pass http://teachest.io:2000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}

server {
    listen 80;
    server_name tealight.io;
    location / {
        proxy_pass http://tealight.io:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}

server {
    listen 80;
    server_name mainbox.io;
    location / {
        proxy_pass http://mainbox.io:4000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

*** Switch between local and remote via /etc/hosts

Use local instances:

`127.0.0.1 tealight.io`
`127.0.0.1 teachest.io`
`127.0.0.1 mainbox.io`

Use remote instances:

`# 127.0.0.1 tealight.io`
`# 127.0.0.1 teachest.io`
`# 127.0.0.1 mainbox.io`

*** loadtest

``` bash
# Load test the Express server
$ loadtest -n 100 -c 10 --rps 5 -k http://teachest.io
```
