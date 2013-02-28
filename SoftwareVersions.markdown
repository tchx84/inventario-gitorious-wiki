You can maintain a registry of XO software versions in inventario.

The primary use for this functionality is to be able to easily identify the software version running on laptops in the laptop connectivity log. The laptops communicate their long, ugly version hash, but if you save a software version that associates this hash to a particular name and description, inventario will show the human-readable software version name in the connectivity log.

To determine the hash of a particular software release that you have booted on an XO, run this script from the Terminal activity:
    http://dev.laptop.org/~dsd/20090521/versionhash.py

Now you can add a "software version" in inventario for this particular build.