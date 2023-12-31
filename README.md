# build containers as root in a container without root, without exposing host's docker socket.

#### Run buildah in chroot mode inside rootless container with a region of shared storage to take advantage of caching. You can also do user namespace separation via --uidmap

    podman run --device /dev/fuse -v ./build:/build:z -v ./shared:/var/lib/shared/:Z -v ./targetdir:/var/lib/containers:Z quay.io/buildah/stable buildah -t my-image bud /build/

#### then run buildah push from that container to some registry

    podman run --device /dev/fuse -v ./build:/build:z -v ./shared:/var/lib/shared/:Z -v ./targetdir:/var/lib/containers:Z quay.io/buildah/stable buildah push localhost/my-image registry:5000

### the Dockerfile under /build is building Buildah itself but could be replaced with whatever you're building.

# refs
https://github.com/containers/buildah/tree/main/contrib/buildahimage

https://developers.redhat.com/blog/2019/08/14/best-practices-for-running-buildah-in-a-container

https://github.com/containers/Demos/blob/main/running/BuildahInPodman/building_ctr_wo_root.sh

https://github.com/containers/storage
