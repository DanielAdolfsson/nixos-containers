
# nixos-containers with nix-store-fs

This repository provides a modified version of the nixos-containers that makes use of `nix-store-fs` to isolate `/nix/store`.

## Differences

* `nix-store-fs` is used to mount `/nix/store` [privateStore].
* Other mounts in `/nix`, such as `db` and `daemon-socket` has been removed to further isolate `/nix` [privateStore]. 
* The container service is allowed access to `/dev/fuse rw`.
* Two new container options `privateUsers` and `privateStore`.
* Source has been formatted with `nix-fmt`.

## How to use

Since this is a modified version of the original `nixos-containers`, you need
to disable that module and then add this version to `imports`.

Like so:

```
  disabledModules = [ "virtualisation/nixos-containers.nix" ];

  imports = [
    "${
      builtins.fetchTarball {
        url = "https://github.com/DanielAdolfsson/nixos-containers/archive/refs/tags/v0.1.0-test.tar.gz";
        sha256 = "sha256:04250jimflzjbhb4d9cybz4hmm27vxc0ry5bq1mv8xxfp95nysi1";
      }
    }/nixos-containers.nix"
  ];

  container.my = {
    privateStore = true;
  };
```

## privateUsers

The new `privateUsers` option turns on user namespaces.
It's the same as passing `-U` to `systemd-nspawn`.

## Important

This modification, as well as `nix-store-fs` is a highly experimental hack. Not everything has been tested, and there are a few things missing. 

You have been warned!

## Acknowledgements

Original module source can be found at:
https://github.com/NixOS/nixpkgs/blob/master/nixos/modules/virtualisation/nixos-containers.nix

```
Copyright (c) 2003-2024 Eelco Dolstra and the Nixpkgs/NixOS contributors

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```
