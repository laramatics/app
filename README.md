<div align="center">

# Laravel App

![GitHub](https://img.shields.io/github/license/laramatics/app)
![Docker Image Size (tag)](https://img.shields.io/docker/image-size/laramatics/app/latest)
![Docker Pulls](https://img.shields.io/docker/pulls/laramatics/app)
![Docker Image Version (latest semver)](https://img.shields.io/docker/v/laramatics/app)
![Docker Cloud Automated build](https://img.shields.io/docker/cloud/automated/laramatics/app)
![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/laramatics/app)

</div>

## About

This repository contains a `Dockerfile` which builds an [image](https://hub.docker.com/r/laramatics/app)
for serving your Laravel app.

### Table of Contents

- [Folder Structure](#folder-structure)
- [Packages and Services](#packages-and-services)
    - [Customizing build versions](#customizing-build-versions)
    - [Adding more PHP extensions](#adding-more-php-extensions)
    - [Adding more packages](#adding-more-packages)
    - [Testing](#testing)
- [References](#references)

## Folder Structure

Although folder structure is self-explanatory, description is as below:

```
.
├── Dockerfile
├── LICENSE
├── readme.md
├── scripts
│   ├── cleanup.sh            # removes build dependencies for lighter image size.
│   ├── install-packages.sh   # OS packages will be installed by this file.
│   └── install-php.sh        # php extensions and installation.
└── tests
    └── goss.yaml             # see "testing" section
```

## Packages and Services

We created the `Dockerfile` with image size in mind, so only packages and PHP extensions which is absolutely necessary
are installed.

|Service|Version|Argument|
|---|:---:|:---:|
|PHP|8.0.1|`PHP_VERSION`|
|Composer|2.0.9|`COMPOSER_VERSION`|

### Customizing build versions

As you can see in the table above, some services has an argument in `Dockerfile` for you to modify installation version,
to do that, you need to clone the repo and build the image yourself:

```bash
git clone https://github.com/laramatics/app.git
cd app
# modify files...
docker build \
  --build-arg PHP_VERSION=8.0.1 \
  --build-arg COMPOSER_VERSION=2 \
  -t <image_name> .
```

### Adding more PHP extensions

If you want to add more extensions to the PHP installation, you will have to build your own image based on the one
already built or modify the `Dockerfile` and `scripts/*` to your liking and build your own image from that as
described [here](#adding-more-packages).

see [Docker PHP Extension Installer](https://github.com/mlocati/docker-php-extension-installer)
for available extensions, although you can install them fom source if you will.

```dockerfile
FROM laramatics/app:latest
# add your extentions here...
RUN docker-php-ext-install -j "$(nproc)" <package_name>
```

### Adding more packages

Sometimes you need a specific package for your pipeline; as described in the previous section, you can build your own
image from `laramatics/app` or clone this repo and modify files to suit your needs.

```shell
git clone https://github.com/laramatics/app.git
cd app
# modify files...
docker build -t <image_name> .
```

### Testing

Tests are written using [GOSS](https://github.com/aelsabbahy/goss/tree/master/extras/dcgoss), to test your changes after
modifying source files and building your own image, run:

```shell
GOSS_FILES_PATH=tests dgoss run -it <image_name> /bin/ash -l
```

# References

- [Useful issue](https://github.com/docker-library/php/issues/1049)