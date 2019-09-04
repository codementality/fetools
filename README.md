# Front End tools in a Docker Container (gulp, grunt, etc.)

## Build Status

[![Build Status](https://travis-ci.org/codementality/fetools.svg?branch=develop)](https://travis-ci.org/codementality/fetools)

## Overview

This project contains the source files for building the Docker image codementality/fetools.

FETools is a container that contains gulp and grunt, and can be used for front end development on any Drupal development project.

The container is located on Docker Hub at:  https://hub.docker.com/r/codementality/fetools

## Adding this toolset to your project

To add this toolset to your project, modify your docker-compose file to include the following:

```
version: '3.4'
...
services:
  fetools:
    build: docker/fetools
    environment:
      TZ: 'EST5EDT'
    depends_on:
      - <place the service name of your PHP container here>
    volumes:
      - ./themesrc/themes:/data/themesrc/themes
      - ./docroot:/data/docroot
...
volumes:
  ## persistent data volume for node_modules
  node_modules:
    driver: local
```

### Explanation of the 'volumes' used by the project

The volume mounts located in the /themesrc directory are explained in detail in the documents contained in the /docs folder.

The following volumes are used by this project:

#### ./themesrc/themes
```
   - ./themesrc/themes:/data/themesrc/themes
```

This host directory should contain your project specific files, and should mirror the `./docroot/themes` directory for any Drupal theme you want to build.  So each custom theme would have a foler at themesrc/themes/custom/THEMENAME.


Of note:

* You can declare a seperate .eslintrc file to change configurations between themes.
* Any partial file starting with an underscore (i.e. "_imports.scss") will not be copied to the theme, and is used for creating a .scss file that imports those files.
* Any Image inside the `images` folder will be minified and copied to the theme `/img` folder.
* This is typically where your theme.js and theme.scsss files will live. Those files will be handled depending on the command and built to your theme's `/js` and `/css` directories, respectively.
* Any additional .scss or .js files that are added inside this folder will be built using the same structure to the `/css` and `/js` directories of the theme. For example: `/themesrc/themes/custom/THEMENAME/libraries/nodes` and `/themesrc/themes/custom/THEMENAME/libraries/paragraphs` folders to house the specific library files that are attached to nodes/paragraphs through a base theme. These files will be built to the `/docroot/themes/custom/THEMENAME/css/libraries/nodes` and `/docroot/themes/custom/THEMENAME/js/libraries/nodes` folders for use with your custom library definitions.

#### ./docroot:/data/docroot
```
   - ./docroot:/data/docroot
```
This volume is the location of your drupal root installation.  The host mount can be changed to suite your project's specific directory structure, but the mapping inside the container should always map to `/data/docroot`.

Please note that "./docroot" may change based on your project's directory structure.