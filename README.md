graphics-core22 helpers
===

This repository contains some helper items for working with the graphics-core22 snap interface:

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
   This is a script to help avoid priming any libraries provided by the graphics-core22 providers.
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
       source: https://github.com/MirServer/graphics-core22.git
       plugin: dump
       override-prime: |
         craftctl default
         ${CRAFT_PART_SRC}/bin/graphics-core22-cleanup mesa-core22 nvidia-core22
       prime:
         - bin/graphics-core22-wrapper
   ```

For more information about the `graphics-core22` interface, see: [The graphics-core22 snap interface](https://mir-server.io/docs/the-graphics-core22-snap-interface) documentation.
