# Reproducibly Built APPC/ACI Baseimage

Ubuntu without apt within less than 25 MiB.

 * blitznote/baseimage [![](https://images.microbadger.com/badges/image/blitznote/baseimage.svg)](https://microbadger.com/images/blitznote/baseimage "Get your own image badge on microbadger.com")

This is a demo which shows how using my patches to **[dgr](https://github.com/wmark/dgr)** you can
create a minimal baseimage for APPC/ACI container runtimes, such as (but not limited to) CoreOS' **rkt**.

Any two builds will result in identical image files.

## Usage
### Docker

Please see [blitznote/ubuntu](https://github.com/Blitznote/docker-ubuntu-debootstrap) for a complete
README, including the advantages and system requirements for this container image.
The most important difference is that this one does not ship with **Perl** and **apt**.

Example usage:
```Docker
FROM blitznote/baseimage

RUN curl …
```

### rkt

```bash
rkt image fetch blitznote.com/aci/base
```

You can find an extensive example, which uses this ACI as dependency and utilizes **dgr** for building,
[here](https://github.com/wmark/aci-avorion-server).

## Howto

1. Install [rkt](https://github.com/coreos/rkt/releases). Any version will do.
2. Download [dgr](https://github.com/wmark/dgr/releases), `chmod a+x dgr`, and copy it into your `PATH`.  
   If you like you can build it yourself, first. Just run `./gomake`
3. `git clone …` this repository. Then `chdir` into it, run `sudo dgr build`.

No matter how often or on what machine you run step (3),
the resulting container image file will be the same. Reproducibly.
`sha256sum target/image.aci` will show the same hash.

## Relevance

Given reproducibly built binaries, packaged in a way that retains that property,
you can create with my fork of **dgr** *reproducible* container images.
This is something which currently is not possible with *Docker*, the *Docker Hub*,
or any tools for *ACI/APPC* container formats, except by manual tinkering.

The big picture regarding *reproducible builds* is to enable any observer to trust
*software binaries* if he/she trusted its finite set of (ideally human-readable) *source files*.
(Given the same applies for the tools used to achieve this.)
