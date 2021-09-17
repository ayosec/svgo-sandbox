# svgo-sandbox

Script to launch [svgo] in a [bubblewrap] sandbox. An executable file for
[svgo] is built using [pkg].

[bubblewrap]: https://github.com/containers/bubblewrap
[pkg]: https://github.com/vercel/pkg
[svgo]: https://github.com/svg/svgo

## Usage

1. Create a [svgo] package in the `vendor` directory, as described below.

2. Add an alias to the `./run` script in this repository.

3. Use the alias to invoke [svgo].

    The current directory at the moment of executing `./run` is the only
    directory writable in the sandbox. If you want to use the `-o` option in
    svgo, the path has to be relative to the current directory.

### Example

```console
$ alias svgo=~/path/to/svgo-sandbox/run

$ svgo --help
Usage: svgo [options] [INPUT...]

Nodejs-based tool for optimizing SVG vector graphics files

Arguments:
  INPUT                      Alias to --input

Options:
  -v, --version              output the version number
  -i, --input <INPUT...>     Input files, "-" for STDIN
  -s, --string <STRING>      Input SVG data string
[…]

$ svgo -i a.svg -o b.svg

a.svg:
Done in 211 ms!
134.196 KiB - 36.3% = 85.438 KiB

$ ls -1sh a.svg  b.svg
136K a.svg
 88K b.svg
```

## Build svgo package

```console
host$ cd svgo-sandbox

host$ mkdir vendor

host$ docker run --rm -ti -v "$PWD/vendor:/h" node bash

$ npm install -g svgo

$ npm install -g pkg

$ cd /h

$ pkg --compress GZ -t node16-linux-x64  /usr/local/lib/node_modules/svgo/bin/svgo

$ chown 1000:1000 svgo
```
