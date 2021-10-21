# Building Images For OpenShift

This file has some tips and tricks for understanding what OpenShift expects when building and running container images,
and how to write your `Dockerfile` and organize your directory layout to make your builds and deployments run
smoothly with OpenShift.

### Do not assume root user

OpenShift runs all its images with an anonymous and random UID, and group id 0:
```sh
$ id
uid=1000640000(1000640000) gid=0(root) groups=0(root),1000640000
```

This is the single most common source of problems when new OpenShift users try to run images they were running using `docker` or `kubernetes`.
Typically these images will fail in OpenShift because they were build to assume they would run as `root:0`.

Here are some tips for writing your `Dockerfile` to work on OpenShift:

#### Ensure group 0 can access your deps
Install your image dependencies into non-root directories, and ensure that they are universally accessible by `gid 0`.
One example of a common install pattern is the following:

```
RUN \
    mkdir -p /opt/app \
 && cd /opt/app \
 && install-my-stuff \
 && chgrp -R 0 /opt/app \
 && chmod g+rwX /opt/app
```

#### Locally test with a different uid

If you wish to test your images locally using `docker` or `podman`,
it is best to add a final line to your `Dockerfile`, like this:

```
# Emulate an anonymous uid
USER 9999:0
```

This will cause `docker` or `podman` (or `kubernetes`) to run your container as `uid 9999` and `gid 0`.
If your image runs this way locally, it is likely to run when OpenShift assigns it a different random uid.



### Directory layouts

Most OpenShift build-related commands (e.g. `oc new-build` or `oc new-app`) provide a way to specify the context directory to use from a repository.
The command argument is typically `--context-dir=your/context/path`, and is a relative path (not beginning with `/`) from the root directory of your repository.

These command line utilities assume that your dockerfile is named `Dockerfile`, and is in your context directory: `your/context/path/Dockerfile`
OpenShift's build systems further assume that *all* of your build dependencies are underneath `your/context/path/...`.

Deviating from this directory layout is possible, but it generally requires editing your `BuildConfig` as a YAML file and setting the `dockerfilePath` field.
See: https://docs.openshift.com/container-platform/3.4/dev_guide/builds/build_strategies.html#dockerfile-path

