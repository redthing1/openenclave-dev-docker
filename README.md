# openenclave-dev-docker

docker image for openenclave development

# usage

build the container image:
```sh
docker build --pull -t openenclave-dev -f ./openenclave-dev.docker .
```

run a container (replace `<sgx_device>` with the correct device as described in [openenclave docker documentation](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/Contributors/BuildingInADockerContainer.md))
```sh
sudo docker run --device <sgx_device> --rm -it openenclave-dev
```

demonstrate that it works:
```sh
# source openenclave paths (pkg-config, etc.)
. /opt/openenclave/share/openenclave/openenclaverc
# go to helloworld sample and build
cd ~/openenclave/samples/helloworld
make -j
# run the helloworld sample in sgx simulation mode
./host/helloworld_host ./enclave/enclave.signed --simulate
```

to run the demo directly in this repo:
```sh
# mount this dir into the container
sudo docker run --device <sgx_device> --rm -it -v $(pwd):/prj openenclave-dev
# inside the container:
cd /prj/demo/helloworld
. /opt/openenclave/share/openenclave/openenclaverc
make -j
# run the helloworld sample in sgx simulation mode
./host/helloworld_host ./enclave/enclave.signed --simulate
```
