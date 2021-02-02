# References

1. flasksocketio project - https://codeburst.io/building-your-first-chat-application-using-flask-in-7-minutes-f98de4adfa5d

# How to install

```bash
sudo apt update
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.6
sudo apt install python3-pip
sudo apt install python-is-python3
pip3 install -r requirements.txt

# https://flask-socketio.readthedocs.io/en/latest/#using-nginx-as-a-websocket-reverse-proxy

sudo apt install nginx
sudo unlink /etc/nginx/sites-enabled/default
sudo su 
cat > /etc/nginx/sites-available/socketio << 'EOF'
server {
    listen 80;
    server_name _;

    location / {
        include proxy_params;
        proxy_pass http://127.0.0.1:5000;
    }

    location /socket.io {
        include proxy_params;
        proxy_http_version 1.1;
        proxy_buffering off;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_pass http://127.0.0.1:5000/socket.io;
    }
}
EOF
ln -s /etc/nginx/sites-available/socketio /etc/nginx/sites-enabled/
nginx -t # Check if the configuration is correct
service nginx reload
```

# How to configure cloudfront

When you configure cloudfront cache Behavior, 2 settngs are very important.

1. Configure cache such a way that it forwards **`Host`** Header. To make sure it forwards *`Host`* Header, I used `Use legacy cache settings` under `Cache and origin request settings` and whitelisted *`Host`* Header.

![Cache and origin request settings](cache-and-origin-request-settings.png "Cache and Origin Request Settings")

2. Make sure `Query String Forwarding and Caching` forwards all query and caches based on all query string.

![Query String Forwarding and Caching Settings](Query-String-Forwarding-and-Caching.png "Query String Forwarding and Caching Settings.")