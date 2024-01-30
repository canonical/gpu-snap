graphics-core helpers
===

This repository contains some helper items for working with the graphics-core* snap interfaces:

**NB**: replace "XX" with either "22" or "24", corresponding to your `base: coreXX` version.

1. `bin/graphics-coreXX-wrapper`
   You should prime this script in your snap and use it in your `command-chain:` for all
   apps requiring GPU support:
   ```yaml
   apps:
     app:
       plugs:
         - opengl
         - wayland
       command-chain:
         - bin/graphics-coreXX-wrapper
       command: usr/bin/eglinfo
   ```

   **NB**: the script assumes that `graphics-coreXX.target:` is set to `$SNAP/graphics`.

2. `bin/graphics-coreXX-cleanup`
   This is a script to help avoid priming any libraries provided by the graphics-coreXX providers.
   You should run it in `override-prime:` of the part whose source is this repository, `after:`
   any parts that stage any libraries, passing the names of the providers you want to make sure
   you're compatible with:

   ```yaml
   parts:
     my-app:
       stage-packages:
     # ...

     graphics-coreXX:
       after: [my-app]
       source: https://github.com/MirServer/graphics-core.git
       plugin: dump
       override-prime: |
         craftctl default
         ${CRAFT_PART_SRC}/bin/graphics-coreXX-cleanup mesa-coreXX nvidia-coreXX
       prime:
         - bin/graphics-coreXX-wrapper
   ```

For more information about the graphics interfaces, see:
- [The graphics-core22 snap interface](https://mir-server.io/docs/the-graphics-core22-snap-interface) documentation.
- [The graphics-core24 snap interface](https://mir-server.io/docs/the-graphics-core24-snap-interface) documentation.
