# Package as a Flatpak

## Build from Git

The Glimpse source code comes with a Flatpak manifest (at `build-aux/flatpak/org.glimpse_editor.Glimpse.json`).

Before building Glimpse, you need to install the required `Platform` and `Sdk` runtimes. You can find the required versions in the manifest. As of today, it says:

     "runtime": "org.gnome.Platform",
     "runtime-version": "3.36",
     "sdk": "org.gnome.Sdk",

This means you need to install the runtimes with:

      $ flatpak install flathub org.gnome.Platform//3.36 org.gnome.Sdk//3.36

From a Git clone of Glimpse, you can trivially build a Git snapshot with:

      $ flatpak-builder --force-clean --ccache _build build-aux/flatpak/org.glimpse_editor.Glimpse.json

Note that Flatpak will build Glimpse as well as all its dependencies which aren't already in the Sdk. This could take a long time. Fortunately Flatpak is smart enough to cache builds and only rebuild what changed, so subsequent builds should be much faster.

## Stable Releases

To make a stable release (e.g for [Flathub](https://flathub.org/)), you need to first edit the manifest to build from a stable point.

Find the Glimpse section in the manifest:

       {
         "name": "glimpse",
         "sources": [
           {
             "type": "dir",
             "path": "../.."
           }
         ],
       }

And replace it by something like the following (adapt to the actual release):

       {
         "name": "glimpse",
         "sources": [
           {
             "type": "git",
             "url": "<https://github.com/glimpse-editor/glimpse>",
             "tag": "0.1.2",
             "commit": "30a4913df8e11635bfdb16029da2237e29e4053a"
           }
         ],
       }

At this point, you can build the Glimpse stable release you chose with the same command as above:

      $ flatpak-builder --force-clean --ccache _build build-aux/flatpak/org.glimpse_editor.Glimpse.json

## Publish

To publish this build, you need to commit it to a repository, ideally signing it with a GPG key:

      $ flatpak-builder --force-clean --ccache --repo=path/to/repository --gpg-sign=${GPG_KEY_ID} _build build-aux/flatpak/org.glimpse_editor.Glimpse.json
      $ flatpak build-update-repo --title="Glimpse" --prune --prune-depth=4 --gpg-sign=${GPG_KEYID} --generate-static-deltas path/to/repository

You can then `rsync` the repository to a server somewhere, behind something like Nginx serving its contents statically.
