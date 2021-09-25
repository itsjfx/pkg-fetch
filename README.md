# pkg-fetch

## Changes

This fork contains [Valetudo](https://github.com/Hypfer/Valetudo)-specific changes to the upstream `pkg-fetch`

### No ICU

Since space is limited in embedded devices, this fork is built with `'--without-intl'`

### Relative Payload Offsets

To be able to utilize UPX compression, this fork uses offsets relative to the EOF to read prelude and payload.

Be advised that this might break if you sign your binary or append anything else apart from prelude and payload to the
binary. Maybe there will be a better solution in the future. For now this shall suffice.

### Static-linking only

Since the whole point of using `pkg` is to have a binary that runs everywhere, we're doing static linking only.

### Linux-only

Unless some Windows armv7 embedded machine magically appears somewhere, this will be linux-only.

### Skip bundled payload

This actually isn't a feature of this fork but just mainline pkg.

By setting the `PKG_EXECPATH` environment variable to `PKG_INVOKE_NODEJS`, the payload gets ignored and you'll end up
with the nodejs runtime which can then be used for other purposes

## How to Use

Fortunately, you can vendor your own pkg nodejs base binaries by using the `PKG_CACHE_PATH` environment variable to point
to a directory that is part of your codebase. e.g. `cross-env PKG_CACHE_PATH=./build_dependencies/pkg pkg --targets node16-linuxstatic-arm64 .`
