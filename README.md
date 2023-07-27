# RouterProxy
A proxy on openwrt

``` bash
docker build . -f /opt/source/RouterProxy/src/RouterProxy/Dockerfile -t routerproxy
docker run --name routerproxy -p 44330:443 -p 44330:443/udp -v /opt/haproxy/pengwei.name.pem:/root/pem -e Kestrel:Certificates:Default:Path=/root/pem -e Kestrel:Certificates:Default:KeyPath=/root/pem -d routerproxy
```
