This page describes how to prepare the Couchapp Installer for the Mac OS X operating system. (based on [bazaar page](http://wiki.bazaar.canonical.com/MacOSXBundle/Prepare))

## Prerequities

* Mac OSX 10.6
* [PackageMaker](http://developer.apple.com/mac/library/documentation/DeveloperTools/Conceptual/PackageMakerUserGuide/Introduction/Introduction.html) installed with XCode
* Disk Utility
* bdist\_mpkg

## Building single packages :

* [Restkit](http://pypi.python.org/pypi/restkit/2.1.4) : version tested 2.1.4
* [CouchApp](http://pypi.python.org/pypi/couchapp/0.7.0) : version tested 0.7.0

Every package should be built the same way (issue the command in the root of the source trees):
```sh
    python setup.py bdist_mpkg
```

This will generate a .mpkg file in the `dist` subdirectory of the source tree. This file is actually a directory that contains some metadata, and further package files (.pkg). You should copy those packages out of the .mpkg structure (you can either use the Terminal for that, or just control-click the .mpkg file in Finder, and select _Show Package Contents_). The package files are located in the `Contents/Packages subdirectory`.

## PackageMaker project preferences

### Distribution

Distribution

* Title: Couchapp VERSION (e.g. Couchapp 0.7.0)
* User Sees: Easy and Custom Install
* Install Destination: System volume
* Description: Utilities to make standalone CouchDB application development simple

Add each packages extracted above to the project.

Location for `*-script` packages is /usr/bin and for `*-purelib` /Library/Python/2.6/site-packages .

## Disk image generation

1.  Download the attached template from here: [couchapp-template.dmg.zip](http://github.com/downloads/couchapp/couchapp/couchapp-template.dmg.zip) Double-click it if it doesn't mount the volume automatically.
2.  Place the generated Installer file into the volume (remove the old one, and don't forget to Empty the Trash!). You could replace the documentation as well.
3.  Unmount the volume.
4.  Open the template DMG file in Disk Utility. Click on Convert and save it as Couchapp-VERSION-OSX10.6.dmg
