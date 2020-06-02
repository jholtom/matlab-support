# matlab-support

Docker image with enough dependencies to support a mounted-in Matlab.

This docker image gives you a Fedora (Latest) environment capable of supporting Matlab.  For licensing reasons, you must already have your own Matlab installed on the Docker host.

To run Matlab inside the container, you have to provide three things at launch time:

- Mount in a Matlab installation at `/usr/local/MATLAB/from-host`.
- Mount in a folder to receive the Matlab execution log at `/var/log/matlab`.
- Spoof the container MAC address to match you own Matlab license file.
  - This can be done with either --net=host (just use your host computers network config)
  - Can also be done with --mac-address="MAC_ADDR_HERE"

These expect you to define some local environment variables:

- `MATLAB_ROOT` is your matlab installation on the Docker host, perhaps `/usr/local/MATLAB/R2016a`.
- `MATLAB_LOGS` is optional path on the Docker host to receive Matlab logs, perhaps `~/matlab-logs`.
- `MATLAB_MAC_ADDRESS` is the MAC address associated with your own Matlab License, of the form `00:00:00:00:00:00`.

## Build the image

Specify the username you use MATLAB with `--build-arg=MATLAB_USER=${YOUR USERNAME HERE}`
```
sudo podman build --rm -t jholtom/matlab-support matlab-support
```

## Running

Then go ahead and run it something like this...(substitute jholtom for your username)
Also can substitute --net=host for the --mac-address argument if you are using rootfull podman

```
sudo podman run --rm -v "/usr/local/MATLAB/R2020a":/usr/local/MATLAB/from-host -v "/var/tmp":/var/log/matlab -u=jholtom --net=host -it jholtom/matlab-support -r" "
```
