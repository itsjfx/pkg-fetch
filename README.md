# pkg-fetch

## Changes

This fork contains [Valetudo](https://github.com/Hypfer/Valetudo)-specific changes to the upstream `pkg-fetch`

### No ICU

Since space is limited in embedded devices, this fork is built with `'--without-intl'`

### UPX Support

UPX is supported

### Static-linking only

Since the whole point of using `pkg` is to have a binary that runs everywhere, we're doing static linking only.

### Linux-only

Unless some Windows armv7 embedded machine magically appears somewhere, this will be linux-only.

### Optimized for size

`-Os`

### Skip bundled payload

This actually isn't a feature of this fork but just mainline pkg.

By setting the `PKG_EXECPATH` environment variable to `PKG_INVOKE_NODEJS`, the payload gets ignored and you'll end up
with the nodejs runtime which can then be used for other purposes

## How to Use

Fortunately, you can vendor your own pkg nodejs base binaries by using the `PKG_CACHE_PATH` environment variable to point
to a directory that is part of your codebase. e.g. `cross-env PKG_CACHE_PATH=./build_dependencies/pkg pkg --targets node16-linuxstatic-arm64 .`

## How to build

Building the docker images will also build the nodejs binary.

### Build the runtime

armv7
`docker build -t valetudo-pkg-fetch-armv7 --build-arg HOST_ARCH=i686 --build-arg TARGET_TRIPLE=armv7l-linux-musleabihf --build-arg PKG_FETCH_OPTION_a=armv7 --build-arg PKG_FETCH_OPTION_n=node18 --build-arg PKG_FETCH_OPTION_p=linuxstatic -f .\Dockerfile.alpine .`

aarch64
`docker build -t valetudo-pkg-fetch-aarch64 --build-arg HOST_ARCH=x86_64 --build-arg TARGET_TRIPLE=aarch64-linux-musl --build-arg PKG_FETCH_OPTION_a=arm64 --build-arg PKG_FETCH_OPTION_n=node18 --build-arg PKG_FETCH_OPTION_p=linuxstatic -f .\Dockerfile.alpine .`

### Create a container based on that image

`docker create valetudo-pkg-fetch-armv7:latest`

### Pull the files

`docker cp id_from_previous_command:/root/pkg-fetch/dist/ .`
