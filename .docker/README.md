# About the Docker container

We use a Docker container to render the `xaringan` presentation via continuous integration on GitLab.

## Gitlab container registry

You can push container images to any namespace or repo that you wish.
It makes sense to re-use the namespace for a project for containers that are used in this project.

To push images we have to login (once).
Note that the credentials will be stored unencrypted in your home. Only do this on trusted hosts.

You must be inside `.docker` when running the command:

```bash
docker login registry.git.mpib-berlin.mpg.de
```

## Build and publish

In order to build the Docker container from the [Dockerfile](Dockerfile) recipe, we need to run:

```bash
docker build -t registry.git.mpib-berlin.mpg.de/neurocode/git-workshop/xaringan:latest .
```

Note, that the version tag `latest` my be replaced by something else, e.g., `0.1` etc.
This make sense, if several different versions of the Docker container are produced over time.

Finally, we need to push the Docker container to the registry:

```bash
docker push registry.git.mpib-berlin.mpg.de/neurocode/git-workshop/xaringan:latest
```


If on two consecutive builds the Dockerfile didn't change up to a specific line then docker can re-use previously built layers for each RUN-line.
This is extremely useful when developing a container and the reason the large install.packages() list is split into multiple calls.
Also we want to stop after a RUN if it fails and that requires a little wrapper around install.packages().
Lastly, by setting the Ncpus option to 4 we can resolve up to 4 packages in parallel.

## Acknowledgement

Thanks to Michael Krause!
