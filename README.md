# Docker Hub Multi-Arch Hooks Example

[![](https://img.shields.io/badge/Github-Template-blue?style=flat&logo=github)](https://img.shields.io/badge/Github-Template-blue?style=flat&logo=github)

The scripts in this respository are intended to provide auto-updating builds based on tags and automatic building of multi-arch images

For best practices on Docker Hub, when you push a patch version of your container, you SHOULD be updating the major and minor versions of your release to point to this new build.  Additionally, when pushing a new minor release, you SHOULD be updating the major container version.

In theory, pulling the tag `:1` of your container, should always pull the most up-to-date release in the `v1` version, be it `v1.3.1` or `v1.99.99`.  If you wanted `v1.0.0` specifically, best practices means you should grab `:1.0.0`. However, if a user always wants `v1.2` and all patches of `v1.2` (e.g. `v1.2.4`, `v1.2.219`), but never `v1.3`, the user MUST download `:1.2`. If someone always wants the latest version, they can download `:latest`.

When you tag and push `v1.2.3` of your repository, the scripts will create a new tag, `:1.2.3`, for your container.  It will also automatically update `:1.2` and `:1` for you.  Keeping all releases up to date.

## Follow My Example

I used this template for my [jnovack/docker-autossh](https://www.github.com/jnovack/docker-autossh) repository, which has automatic builds on Docker Hub for my [jnovack/autossh](https://hub.docker.com/jnovack/autossh) image.

You can `docker pull jnovack/autossh:2.0.0` from a raspberry pi 3 (`arm32v7` architecture), a raspberry pi zero (`arm32v6` architecture), an Amazon AWS instance (`arm64v8` architecture), or a linux or mac desktop (`amd64` architecture).  Each image will find the correct architecture to download, without having to specify different tags.

[![](preview.png)](preview.png)

## How To Use

1. You MUST click "Use This Template" on Github. DO NOT "Fork" a *template* repository.
2. You SHOULD update and synchronize all your `Dockerfile`s, adding all your commands at the end. Please update the label section.
3. You MUST tag and push your commits with [semantic versioning standards](https://semver.org).

### Tagging Your Commits

In order to use these scripts, you **MUST** tag your commits following [semantic version standards](https://semver.org).

The format is `vMAJOR.MINOR.PATCH` (please note the lowercase `v`).  Examples: `v0.0.4`, `v1.0.0`, `v2.0.103`.  Yes, I'm aware the `v` is not a part of semantic versioning, use it in the git tags; it will not show up on your images.

If you wish to use lightweight tags within git, it is recommended to push your tag **PRIOR** to pushing the commit so that Docker Hub does not label your build incorrectly.  Yes, there is a delay prior to Docker Hub building your release, however, you should not rely on it.

To properly version your releases (according to semver) increment the:

1. **MAJOR** version when you make incompatible API changes,
1. **MINOR** version when you add functionality in a backwards-compatible manner, and
1. **PATCH** version when you make backwards-compatible bug fixes.

Additional labels for pre-release and build metadata are available as extensions to the `MAJOR.MINOR.PATCH` format.  Examples: `v1.0.0-rc10`, `v1.9.0-alpha`

### Shallow Pulls

When Docker Cloud pulls a branch from a source code repository, it performs a shallow clone (only the tip of the specified branch). This has the advantage of minimizing the amount of data transfer necessary from the repository and speeding up the build because it pulls only the minimal code necessary.

Because of this, in order to get the tags, a full "unshallow" clone needs to be fetched.  This can potentially take a very long time and transfers a lot of data depending on your repository.  It is not recommended to use these scripts on a large repository.  Sorry.

## Notes

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in [RFC 2119](http://tools.ietf.org/html/rfc2119).
