There is a [plugin repository](https://github.com/endless-sky/endless-sky-plugins) consisting of community created plugins. If you'd like to add your plugin to the repository, you can follow the instructions below.

## Prerequisites

Once you have your plugin you will need someplace to host it on the internet. Most commonly this is done as a GitHub repository. If you are on Windows you can use the [GitHub Desktop](https://desktop.github.com/) application to create and manage your repository.

## How to add your plugin to the central repository

Instead of uploading a zip file to the plugins list, you upload a [manifest file](https://github.com/endless-sky/rfcs/blob/main/rfcs/0001-plugin-index.md). This is a file that describes various properties of your plugin, like name, description, icon, and most importantly, how the auto update works.

To help simplify creating such a manifest file, you can use the [following page](https://endless-sky.github.io/plugin-manifest-gen.html). Once you have your manifest file, go to the [repository](https://github.com/endless-sky/endless-sky-plugins), enter the `manifests/` directory and click on the + button at the top and then "Upload files" and select your manifest file. The filename should be `your-plugin-name.yaml`.

As soon as your plugin gets accepted, it will appear on the plugin repository. You can now use your repository to create new releases, which will get automatically reflected in the central repository after a while.