# How to run Gluster Docker Containers

```bash
docker pull gluster/gluster-centos

docker rm -f -v glusterfs

docker run --name=glusterfs \
 -v $PWD/etc/glusterfs:/etc/glusterfs:z \
 -v $PWD/var/lib/glusterfs:/var/lib/glusterfs:z \
 -v $PWD/var/log/glusterfs:/var/log/glusterfs:z \
 -v /sys/fs/cgroup:/sys/fs/cgroup:ro \
 -v /dev:/dev:ro \
 -d --privileged=true --net=host \
 gluster/gluster-centos

docker exec -it glusterfs bash

systemctl status glusterd
...
   Active: active (running) since Sun 2017-09-10 19:06:43 UTC; 4min 18s ago
...

cat /etc/redhat-release
CentOS Linux release 7.3.1611 (Core)

glusterd --version
glusterfs 3.10.2

mount | grep gluster
osxfs on /etc/glusterfs type fuse.osxfs (rw,nosuid,nodev,relatime,user_id=0,group_id=0,allow_other,max_read=1048576)
osxfs on /var/lib/glusterfs type fuse.osxfs (rw,nosuid,nodev,relatime,user_id=0,group_id=0,allow_other,max_read=1048576)
osxfs on /var/log/glusterfs type fuse.osxfs (rw,nosuid,nodev,relatime,user_id=0,group_id=0,allow_other,max_read=1048576)

gluster v list
No volumes present in cluster

gluster peer status
Number of Peers: 0

exit

docker inspect glusterfs
...
            "Cmd": [
                "/usr/sbin/init"
            ],
            "Image": "gluster/gluster-centos",
            "Volumes": {
                "/sys/fs/cgroup": {}
            },
            "Labels": {
                "architecture": "x86_64",
                "build-date": "20170510",
                "description": "Gluster Image is based on CentOS Image which is a scalable network filesystem. Using common off-the-shelf hardware, you can create large, distributed storage solutions for media streaming, data analysis, and other data- and bandwidth-intensive tasks.",
                "io.k8s.description": "Gluster Image is based on CentOS Image which is a scalable network filesystem. Using common off-the-shelf hardware, you can create large, distributed storage solutions for media streaming, data analysis, and other data- and bandwidth-intensive tasks.",
                "io.k8s.display-name": "Gluster 3.10 based on CentOS 7",
                "io.openshift.tags": "gluster,glusterfs,glusterfs-centos",
                "license": "GPLv2",
                "name": "gluster/gluster-centos",
                "summary": "This image has a running glusterfs service ( CentOS 7 + Gluster 3.10)",
                "vendor": "Red Hat, Inc",
                "version": "3.10"
            }
...
```

links:

- [slides](https://www.slideshare.net/HumbleChirammal/gluster-containers)
- [youtube](https://www.youtube.com/watch?v=4Xf8pmDEZYw)
