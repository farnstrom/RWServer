## Introduction

This repository houses code originally developed by Luca Chiarabini run by an external server to provide additional functionality to the RopeWiki site.

## Changes

Before beginning the repository, these changes were made to the original code recovered from Luca's computer:
* Moved sensitive information in code (passwords, etc) into sensitive.h which is not included in this repository
* Identified close to the minimum number of files for the build to succeed, out of the thousands of files in the original folder

## Development environment setup

The RopeWiki executables can be built with Visual Studio 2010, but not the free Express edition.

## HTTP server

'rw.exe' listens for FastCGI commands on port 9001. In order to use it from a web browser, a HTTP server is needed. For local testing purposes, on Windows this can be set up using nginx. Instructions are in the 'Installation steps' below.

## Notes

This repo does not include data files that are necessary for RopeWiki Explorer.  Documentation will have to be added for how to acquire this data.

It's likely that there will be more bugs due to the differences between compilers. Specifically, the change from 'char' types defaulting to 'signed' now instead of 'unsigned' is likely to be an issue.

## Installation steps

* download nginx:  http://nginx.org/en/download.html
* unzip and place in c:\ folder
* edit conf\nginx.conf file
* add following lines under 'location /' tag:
```
            fastcgi_pass 127.0.0.1:9001;
            include fastcgi_params; # need this line, or query string will be empty
```

* copy nginx.bat file to the AWS server from the RopeWiki development folder
* edit nginx.bat file and modify the path of the nginx.exe to where it was placed above
* run the nginx.bat file and make sure the nginx.exe process starts. You may have to enable permissions

* install [Microsoft Visual C++ 2010 Redistributable Package (x86) package](https://www.microsoft.com/en-us/download/details.aspx?id=5555)
* edit environment variable PATH (for some reason the files above are installed in folder not in the PATH):
  * add new: C:\Windows\SysWOW64 (note: this step may not be necessary)
* open windows firewall: create 'new rule...', open Port 80, give some name (e.g. 'Web Access')
* create new folder c:\rw
* copy following files inside:
  * rwr.exe  (rwr.exe is release version; rw.exe is debug version)
  * GPSBabel.exe
  * gdal111.dll
  * geos.dll
  * geos_c.dll
  * libcurl.dll
  * libexpat.dll
  * libmysql.dll
  * libpq.dll
  * openjp2.dll
  * spatialite.dll
  * xerces-c_2_8.dll
  * zlib1.dll
  * libeay32.dll
  * ssleay32.dll

* run rwr.exe and make sure it starts up ok. You should see the line 'Actively Listening'
* test from a KML/GPX download link from RopeWiki.com.
* if that does not succeed, you can test from a local web browser on the server, with something like this URL: "http://localhost/rwr?gpx=on&filename=Behunin_Canyon&url=http://ropewiki.com/images/c/cb/Behunin_Canyon.kml"
* You should see in the rw.exe output that it received the command, and the web browser should give you a "Behunin_Canyon.gpx" file to download.

## Tests

- KML->GPX conversion commands are working. Make sure all links in the RopeWiki.com templates point to the luca.ropewiki.com server address.
- ZIP/PDF download:
  - PDF: Page (http://localhost/rwr?filename=Behunin_Canyon.pdf&pdfx=Behunin_Canyon&summary=off&docwidth=1903&ext=.rw) - seems to work
  - PDF: Map (http://localhost/rwr?filename=Behunin_Canyon_MAP.pdf&pdfx=Map?pagename=Behunin_Canyon&summary=off&docwidth=1903&ext=.rw)
  - ZIP: Page+Maps (http://localhost/rwr?filename=Behunin_Canyon.zip&zipx=Behunin_Canyon&bslinks=off&trlinks=off&summary=off&docwidth=1903&ext=.rw) - seems to work
  - ZIP: P+M+Links (http://localhost/rwr?filename=Behunin_Canyon+.zip&zipx=Behunin_Canyon&bslinks=on&summary=off&docwidth=1903&ext=.rw) - probably not working - took long time, produced truncated zip

Other commands have not been attempted yet. We hava not tried to run the 'httprobot' executable. Waterflow analysis broke when loading and is turned off in code.

## RopeWiki references

To have RopeWiki call a new RWServer, the server location must be updated in the ropewiki.com MediaWiki content.  References to luca.ropewiki.com (or old server) must be changed in the following [Templates](http://ropewiki.com/index.php?title=Special:Templates)

* KMLDisplay
* KMLDisplay2
* KMLLink
* KMLExtract

## Contributors

This original codebase was created by Luca Chiarabini. Michelle Nilles brought the code back online and installed to an AWS server provided by Dav, and also brought the code to a state that could be distributed publicly and published here to Github.  Some of this document attributed to Ben on GitHub was written by Fredrik.
