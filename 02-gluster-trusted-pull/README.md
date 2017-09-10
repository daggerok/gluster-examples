# Gluster Trusted Pull using Docker Containers

```bash
docker pull gluster/gluster-centos

docker rm -f -v glusterfs1 glusterfs2

docker run \
 --name=glusterfs1 \
 --hostname=glusterfs1 \
 -v $PWD/etc/glusterfs1:/etc/glusterfs:z \
 -v $PWD/var/lib/glusterfs1:/var/lib/glusterfs:z \
 -v $PWD/var/log/glusterfs1:/var/log/glusterfs:z \
 -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
 -d --privileged=true \
 gluster/gluster-centos

docker run \
 --name=glusterfs2 \
 --hostname=glusterfs2 \
 -v $PWD/etc/glusterfs2:/etc/glusterfs:z \
 -v $PWD/var/lib/glusterfs2:/var/lib/glusterfs:z \
 -v $PWD/var/log/glusterfs2:/var/log/glusterfs:z \
 -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
 -d --privileged=true \
 gluster/gluster-centos

docker ps
... PORTS                                                                                                                                                   NAMES
... 111/tcp, 245/tcp, 443/tcp, 2049/tcp, 2222/tcp, 6010-6012/tcp, 8080/tcp, 24007/tcp, 38465-38466/tcp, 38468-38469/tcp, 49152-49154/tcp, 49156-49162/tcp   glusterfs2
... 111/tcp, 245/tcp, 443/tcp, 2049/tcp, 2222/tcp, 6010-6012/tcp, 8080/tcp, 24007/tcp, 38465-38466/tcp, 38468-38469/tcp, 49152-49154/tcp, 49156-49162/tcp   glusterfs1

# go insde containers each in diferent shell:
docker exec -it glusterfs1 bash
docker exec -it glusterfs2 bash

# check gluster demon status
[root@glusterfs1 /]# systemctl status glusterd
... Active: active (running) since Sun 2017-09-10 20:04:11 UTC; 5min ago

[root@glusterfs2 /]# systemctl status glusterd
... Active: active (running) since Sun 2017-09-10 20:04:21 UTC; 4min 27s ago

# ip addr
[root@glusterfs1 /]# ip a|grep inet|grep eth0
    inet 172.17.0.2/16 scope global eth0

[root@glusterfs2 /]# ip a|grep inet|grep eth0
    inet 172.17.0.3/16 scope global eth0

# peers status
[root@glusterfs1 /]# gluster peer status
Number of Peers: 0

[root@glusterfs2 /]# gluster peer status
Number of Peers: 0

# peer probe
[root@glusterfs1 /]# gluster peer probe 172.17.0.3
peer probe: success.
[root@glusterfs1 /]# gluster peer status
Number of Peers: 1

Hostname: 172.17.0.3
Uuid: 5f8374f3-d2a1-433d-95ed-9a77f140eaa5
State: Peer in Cluster (Connected)

[root@glusterfs2 /]# gluster peer status
Number of Peers: 1

Hostname: 172.17.0.2
Uuid: 0fd752d8-46be-46aa-90d8-d0bd6077a15f
State: Peer in Cluster (Connected)

[root@glusterfs2 /]# exit
docker stop glusterfs2
glusterfs2

[root@glusterfs1 /]# gluster peer status
Number of Peers: 1

Hostname: 172.17.0.3
Uuid: 5f8374f3-d2a1-433d-95ed-9a77f140eaa5
State: Peer in Cluster (Disconnected)

docker start glusterfs2
glusterfs2

[root@glusterfs1 /]# gluster peer status
Number of Peers: 1

Hostname: 172.17.0.3
Uuid: 5f8374f3-d2a1-433d-95ed-9a77f140eaa5
State: Peer in Cluster (Disconnected)

# wait some time

[root@glusterfs1 /]# gluster peer status
Number of Peers: 1

Hostname: 172.17.0.3
Uuid: 5f8374f3-d2a1-433d-95ed-9a77f140eaa5
State: Peer in Cluster (Connected)

# done
```

links:

- [youtube](https://www.youtube.com/watch?v=y8HgXBEKC0I&index=2&list=PLjokYq9Sm90mGnLvC71nL8eQQrZZIadye)
