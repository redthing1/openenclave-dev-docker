# openenclave-dev-docker

docker image for openenclave development

+ based on the [offiical openenclave base image](https://github.com/openenclave/openenclave/blob/45280ac3134cfa8f74eb2fe9c911ec0dc2727dbb/.jenkins/infrastructure/docker/dockerfiles/linux/base/README.md)
+ builds the sdk within the container
+ can build openenclave projects to binaries
+ does not require working sgx, only simulation mode

# usage

## build
build the container image:
```sh
docker build --pull -t openenclave-dev -f ./openenclave-dev.docker .
```

## run container
run a container:
+ for running in **sgx hardware**, add `--device <sgx_device>`, with the correct device as described in [openenclave docker documentation](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/Contributors/BuildingInADockerContainer.md).
+ for running in **simulation mode**, no special device is necessary

run a container (sgx hardware support)
```sh
sudo docker run --device <sgx_device> --rm -it openenclave-dev
```

run a container (sgx simulation support)
```sh
docker run --rm -it openenclave-dev
```

## run enclave program

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
docker run --rm -it -v $(pwd):/prj openenclave-dev
# inside the container:
cd /prj/demo/helloworld
. /opt/openenclave/share/openenclave/openenclaverc
make -j
# run the helloworld sample in sgx simulation mode
./host/helloworld_host ./enclave/enclave.signed --simulate
```
