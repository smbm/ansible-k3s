This is based on the official k3s role but builds on it with a bunch of stuff that I needed for my setup.

My Raspberry Pis run Ubuntu 20.04, they use USB connected SSDs for `/` and `/boot` lives on an SD card.

I had a need for shared storage without any external hardware dependencies. I tried GlusterFS with the local path provisioner but found that my pods were not portable even though the data itself was available on both nodes.

I looked at setting up Heketi and the full on Gluster provisioner but it seemed needlessly complex for what I needed.

In the end I settled on a somewhat hacky approach of installing GlusterFS with Ganesha NFS exposing my Gluster share. Then I used the NFS Client Provisioner pointed at `127.0.0.1`. As both nodes had the Gluster volume available and exposed by Ganesha each of them was able to connect to the NFS share but without a dependency on the other one staying up. It is very ugly, but it works.

It relies on GeerlingGuys superb GlusterFS role whish is listed as a dependency and can be installed using:

`ansible-galaxy install -r requirements.yml`
