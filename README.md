# mruby-packer
(Experimental) Pack mruby source code and headers in a buildable format.

## Download
[Releases](https://github.com/ongaeshi/mruby-packer/releases) page.

Copy the unzipped files to your project.
- Add `mruby/src` to build target
- Pass the include path to `mruby/include` (-I)

## Build
- First, build [mruby](https://github.com/mruby/mruby)
- Clone mruby-packer next to mruby
```
$ pwd
/path/to/mruby
$ cd ../
$ git clone https://github.com/ongaeshi/mruby-packer.git
$ cd mruby-packer
```
- rake
```
$ rake
mkdir -p mruby
mkdir -p mruby/src
mkdir -p mruby/include
cp ../mruby/src/array.c ../mruby/src/backtrace.c...
.
.
```

## TIPS
- Build error on VS2019
  - Add define of `_CRT_SECURE_NO_WARNINGS` to vcxproj