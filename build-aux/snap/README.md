# Package as a Snap

To build the snap, you'll need to have `snapd`, `snapcraft`, and `multipass` installed.

-   `snapd` is typically provided by your distro. For more info, see [installing snapd](https://snapcraft.io/docs/installing-snapd).
-   Both `snapcraft` and `multipass` are best to be installed from the snap store (will be more up to date than what distros provide):

         $ sudo snap install snapcraft --classic  
         $ sudo snap install multipass --classic

Once `snapd`, `snapcraft`, and `multipass` are installed, you are ready to build the glimpse snap (from glimpse's root directory):

         $ snapcraft

The build will take ~30 min and once completed, you'll have a message similar to:

         Snapped glimpse-editor_0+git.30a4913df8_amd64.snap

You can install this snap and then run it by either clicking the application icon installed or run it from the command line: 

         $ sudo snap install glimpse-editor_0+git.30a4913df8_amd64.snap --dangerous  
         $ snap run glimpse-editor
