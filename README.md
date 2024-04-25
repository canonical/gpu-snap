GPU userspace helpers for snaps.
===

This repository contains some helper items for working with the interfaces supplying GPU userspace for snaps.

## `base: core24` snaps

1. `bin/gpu-2404-wrapper`
   You should prime this script in your snap and use it in your `command-chain:` for all
   apps requiring GPU support:
   ```yaml
   apps:
     app:
       plugs:
         - opengl
         - wayland
       command-chain:
         - bin/gpu-2404-wrapper
       command: usr/bin/eglinfo
   ```

   **NB**: the script assumes that `gpu-2404.target:` is set to `$SNAP/gpu-2404`.

2. `bin/gpu-2404-cleanup`
   This is a script to help avoid priming any libraries provided by the content providers.
   You should run it in `override-prime:` of the part whose source is this repository, `after:`
   any parts that stage any libraries, passing the names of the providers you want to make sure
   you're compatible with:

   ```yaml
   parts:
     my-app:
       stage-packages:
     # ...

     gpu-2404:
       after: [my-app]
       source: https://github.com/canonical/gpu-snap.git
       plugin: dump
       override-prime: |
         craftctl default
         ${CRAFT_PART_SRC}/bin/gpu-2404-cleanup mesa-2404 nvidia-2404
       prime:
         - bin/gpu-2404-wrapper
   ```

## `base: core22` snaps

1. `bin/graphics-core22-wrapper`
   You should prime this script in your snap and use it in your `command-chain:` for all
   apps requiring GPU support:
   ```yaml
   apps:
     app:
       plugs:
         - opengl
         - wayland
       command-chain:
         - bin/graphics-core22-wrapper
       command: usr/bin/eglinfo
   ```

   **NB**: the script assumes that `graphics-core22.target:` is set to `$SNAP/graphics`.

2. `bin/graphics-core22-cleanup`
   This is a script to help avoid priming any libraries provided by the content providers.
   You should run it in `override-prime:` of the part whose source is this repository, `after:`
   any parts that stage any libraries, passing the names of the providers you want to make sure
   you're compatible with:

   ```yaml
   parts:
     my-app:
       stage-packages:
     # ...

     graphics-core22:
       after: [my-app]
       source: https://github.com/canonical/gpu-snap.git
       plugin: dump
       override-prime: |
         craftctl default
         ${CRAFT_PART_SRC}/bin/graphics-core22-cleanup mesa-core22 nvidia-core22
       prime:
         - bin/graphics-core22-wrapper
   ```

## Documentation

For more information about those interfaces, see:
- [The graphics-core22 snap interface](https://mir-server.io/docs/the-graphics-core22-snap-interface) documentation.
- [The gpu-2404 snap interface](https://mir-server.io/docs/the-gpu-2404-snap-interface) documentation.
