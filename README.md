# API-Caching-Redis-Python-ALB

## Create network

```
# docker network create mynetwork
```

## Creating container for caching using redis image

```
# docker container run --name redis -d --network mynetwork --restart always redis:latest

```

## Creating containers with flask applications

Here I am creating 3 containers with the same application to get the geoIP location of the input IP address and to map local systems ports 8081, 8082 and 8083 to the container port number 8080.

Your IPSTACK_KEY is your unique authentication key used to gain access to the ipstack API, https://ipstack.com/. 

```
# docker container run --name flask01 -d --network mynetwork --restart always -p 8081:8080  -e CACHING_SERVER=redis -e IPSTACK_KEY="IPSTACK_ACCESS_KEY"  betcybabu/ipstack:version1
# docker container run --name flask02 -d --network mynetwork --restart always -p 8082:8080  -e CACHING_SERVER=redis -e IPSTACK_KEY="IPSTACK_ACCESS_KEY"  betcybabu/ipstack:version1
# docker container run --name flask03 -d --network mynetwork --restart always -p 8083:8080  -e CACHING_SERVER=redis -e IPSTACK_KEY="IPSTACK_ACCESS_KEY"  betcybabu/ipstack:version1

```

Now your applicaton will be availble on IPv4 Public IP:8081, IPv4 Public IP:8082, and IPv4 Public IP:8083

## Load balancing containers with ALB

Create a target group and register the targets as shown below,

I am registering the same instance as different targets on port 8081, 8082 and 8083.

![image](https://user-images.githubusercontent.com/23291976/146956663-fc0ca9a7-6f4b-4deb-afbd-80abc13567de.png)

Create ALB using this target group

## Output

![image](https://user-images.githubusercontent.com/23291976/146958202-0f869806-e476-4dc8-9b1e-b45c240310ea.png)
![image](https://user-images.githubusercontent.com/23291976/146958308-84f9a234-e532-4af5-ad2e-557042a614b5.png)
![image](https://user-images.githubusercontent.com/23291976/146958363-1fb87691-1cf9-4c78-8e5c-8f70ef65082f.png)


Instead of setting up a container for caching (Redis) you can use ``ElastiCache``, then replace ``CACHING_SERVER=redis`` with ElastiCache primary end point.






