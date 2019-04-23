# ska-makefiles
A central place to keep and manage all of the SKA-telescope organisation's Makefiles.

The following sections will detail the instructions needed to be followed to make use of this repository.

**How to use this repository**

Each project Makefile lives in a directory with the name matching with the git repository, e.g. `tango_example`. To use this repo in your own git project, you will need to add it as a submodule in your project. To do that, run the following command in the root directory:

```git submodule add ska-makefiles```

This will create a file `.gitsubmodules` and a subdirectory named `ska-makefiles` in your project. Commit and push those changes upstream.

**Cloning your own repo**

Cloning your git project does not clone the submodule's contents. To have it cloned along with your repository, you need to add a flag, `--recurse-submodules`. For example:

```git clone --recurse-submodules <repo>```

**Updating the ska-makefiles submodule**

To pull in new changes from upstream run the following command:

```git submodule update --remote```
