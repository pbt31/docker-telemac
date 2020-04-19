# What is TELEMAC-MASCARET

The TELEMAC-MASCARET solver suite is a high-performance Fortran based application for a range of free-surface flow problems.

This image provides a Debian-based precompiled packaging of the latest TELEMAC-MASCARET suite for immediate usage.

It includes Open MPI support for multi-CPU/multi-core parallelization.

The image is hosted as public [Dockerhub Repository](https://hub.docker.com/repository/docker/flussplan/telemac) and sources
can be found on the [flussplan Github](https://github.com/flussplan/docker-telemac).

# How to use this image

The image is designed for interactive usage in the shell:

```docker run --rm -it -v ./workdir:/opt/telemac-mascaret/latest/workdir flussplan/telemac
```

As you can see the TELEMAC-MASCARET suite is installed in the folder `/opt/telemac-mascaret/<version>` with a symbolic link being
provided via `/opt/telemac-mascaret/latest` for easy access from the host system.

In the example above a local folder named `workdir` is mapped into the `${HOMETEL}` root installation folder. This is where you
can put your simulation files and data for execution and fetch/visualize the results on the host machine if required.

The root installation folder is also where the image shell will drop you after startup.

You can run TELEMAC or MASCARET solvers from there like:

```# cd examples/telemac2d/gouttedo
# telemac2d.py t2d_gouttedo.cas --ncsize=4
```

Just copy your simulation files into the `workdir` folder on the host machine and run your it inside the Docker container, e.g.
in a subfolder named `my-simulation` like:

```# cd workdir/my-simulation
# telemac2d.py my-simulation.cas --ncsize=4
```

On the host machine you can browse the local folder `workdir/my-simulation` after the simulation has finished to check the results.  

# Docker compose file

In the github repository provided above you will find a Docker compose file for easy startup of the image. Just check out the
repository to start using it immediately as shown here:

```# docker-compose run --rm --service-ports telemac-mascaret
```

This compose file is prepared with an automated mapping of the subfolder `workdir` (as in the `docker run` example above) as well
as a port mapping for the included web-based file browser running at [http://localhost:8090] for easy access to the examples and docs in the current
distribution. The compose file also makes sure to map all available CPUs into the docker image for parallelization, please adapt your
`--ncsize` parameter accordingly.

Note that you can disable the apache service the file browser by passing the environment variable `DISABLE_APACHE=1` to `docker` or `docker-compose`
if you do not need to browse docs or examples. Since the file browser imposes a slight security risk because the TELEMAC-MASCARET installation
files are accessible in read/write manner there the mapped port `8090` should not be accessible on the public interface of the host machine
to prevent access outside of `localhost`. 

# Image tags

While the tag `latest` will provide access to the most current stable TELEMAC-MASCARET, you can also access versions directly with
the corresponsing TELEMAC-MASCARET Subversion repository tag.

This image started to build against `v8p1r1`, contact the maintainer if you require older versions in Docker format.

# License

This software is provided by [flussplan](http://www.flussplan.at) under the GNU GPL Version 3 license, similar to TELEMAC-MASCARET itself.

See the `LICENSE.txt` in the repository for details.