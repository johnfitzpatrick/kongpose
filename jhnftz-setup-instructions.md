# Pseudo code

```
sudo apt update
sudo apt install docker-compose docker httpie jq -y
```

```
HOSTIP=$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4)
sudo cat << EOF >> /etc/hosts
$HOSTIP manager.kong.lan
$HOSTIP api.kong.lan
$HOSTIP portal.kong.lan
$HOSTIP portal-api.kong.lan
$HOSTIP proxy.kong.lan
EOF
```

# If permission denied, you can add manually

```
echo "
$HOSTIP manager.kong.lan
$HOSTIP api.kong.lan
$HOSTIP portal.kong.lan
$HOSTIP portal-api.kong.lan
$HOSTIP proxy.kong.lan
"
```

# SHould see something like this
```
tail -6 /etc/hosts
54.175.115.117 manager.kong.lan
54.175.115.117 api.kong.lan
54.175.115.117 portal.kong.lan
54.175.115.117 portal-api.kong.lan
54.175.115.117 proxy.kong.lan
```
# Clone the repo

git clone https://github.com/johnfitzpatrick/kongpose.git
cd kongpose
git checkout simple-jaf
sudo chmod -R 644 ./ssl-certs/*
sudo docker-compose up -d
```


# Add a licence
```
curl --http1.1 -k -X POST 'http://localhost:8001/licenses' -F "payload=@/home/ubuntu/kongpose/license.json"
```

# Test proxy
```
http --headers GET http://localhost:8001
http --headers GET http://api.kong.lan:8001
```

or

```
http --headers GET http://api.kong.lan:8001
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: https://manager.kong.lan:8445
Connection: keep-alive
Content-Length: 18396
Content-Type: application/json; charset=utf-8
Date: Tue, 15 Mar 2022 10:17:54 GMT
Server: kong/2.4.1.1-enterprise-edition
X-Kong-Admin-Latency: 2
X-Kong-Admin-Request-ID: 0AFGXpCoX5IYxY7dRGfuaOX0UFonYy0f
vary: Origin
```