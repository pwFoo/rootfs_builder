Rootfs Builder
======
Rootfs builder pulls an image from a Docker registry and extracts the rootfs.
This is equivalent to the command:

`mkdir rootfs && docker export $(docker create busybox) | tar -C rootfs -xvf -`

The rootfs generated is OCI compliant and can be run with RunC.  The user can
specify the user to chown the files to and whether or not to use a subuid mapping
in case they want to unshare user namespaces.

Installation
=====
Install Go 1.12

On debian:sid
`apt-get install -y golang-1.12-go`.

From source:
```
sudo apt-get update
wget https://dl.google.com/go/go1.12.7.linux-amd64.tar.gz
sudo tar -xvf go1.12.7.linux-amd64.tar.gz
sudo mv go /usr/local
sudo mv /usr/local/go/bin/go /bin
```

Rootfs builder can be statically built.  This statically compiles rootfs builder in a container:
`make in_container`

Or if you want to develop Rootfs Builder in a container, run:
`make dev`

Usage
=====
Rootfs builder can be run with:
`./rootfs_builder <config.json>`

An example config.json looks like:
```
{
    "Name": "debian:buster",
    "Cert": /workdir/cert,
    "Retries": 3,
    "HTTPS": true,
    "Spec":
        {
            "Dest": "/tmp/rootfs",
            "User": "fas",
            "UseSubuid": True
        }
}
```
* **`Name`** (string, REQUIRED) Name of image to pull.
* **`Cert`** (string, OPTIONAL) Path to cert to add to root CAs for the registry.
* **`Retries`** (int, OPTIONAL) Number of attempts to connect to registry.
* **`HTTPS`** (bool, OPTIONAL) Whether or not connection should be over HTTPS.
* **`Spec`** (dict, OPTIONAL) Spec for the rootfs.
* **`Dest`** (string, OPTIONAL) Destination to extract rootfs to.
* **`User`** (string, OPTIONAL) User to chown files to.
* **`UseSubuid`** (bool, OPTIONAL) Look up subuid mapping for giving user and chown to that uid.

Credits
=====
Code recycled from Google's Kaniko.
