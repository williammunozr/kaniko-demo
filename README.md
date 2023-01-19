# Kaniko Demo

> Build Docker Images with Docker Daemon

### First create a kubernetes secret with docker registry credentials
```sh
kubectl create secret docker-registry registry-credentials --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-password> --docker-email=<your-email>
```
```sh
kubectl create secret docker-registry registry-credentials --docker-server=https://index.docker.io/v1/ --docker-username=hachikoapp --docker-password=<your-password> --docker-email=<your-email>
```
### Using the Executor: Various command-line flags
    Usage:
      executor [flags]
      executor [command]
    
    Available Commands:
      completion  Generate the autocompletion script for the specified shell
      help        Help about any command
      version     Print the version number of kaniko
    
    Flags:
          --build-arg multi-arg type                  This flag allows you to pass in ARG values at build time. Set it repeatedly for multiple values.
          --cache                                     Use cache when building image
          --cache-copy-layers                         Caches copy layers
          --cache-dir string                          Specify a local directory to use as a cache. (default "/cache")
          --cache-repo string                         Specify a repository to use as a cache, otherwise one will be inferred from the destination provided
          --cache-ttl duration                        Cache timeout in hours. Defaults to two weeks. (default 336h0m0s)
          --cleanup                                   Clean the filesystem at the end
          --compressed-caching                        Compress the cached layers. Decreases build time, but increases memory usage. (default true)
      -c, --context string                            Path to the dockerfile build context. (default "/workspace/")
          --context-sub-path string                   Sub path within the given context.
          --customPlatform string                     Specify the build platform if different from the current host
      -d, --destination multi-arg type                Registry the final image should be pushed to. Set it repeatedly for multiple destinations.
          --digest-file string                        Specify a file to save the digest of the built image to.
      -f, --dockerfile string                         Path to the dockerfile to be built. (default "Dockerfile")
          --force                                     Force building outside of a container
          --force-build-metadata                      Force add metadata layers to build image
          --git gitoptions                            Branch to clone if build context is a git repository (default branch=,single-branch=false,recurse-submodules=false)
      -h, --help                                      help for executor
          --ignore-path multi-arg type                Ignore these paths when taking a snapshot. Set it repeatedly for multiple paths.
          --ignore-var-run                            Ignore /var/run directory when taking image snapshot. Set it to false to preserve /var/run/ in destination image. (default true)
          --image-fs-extract-retry int                Number of retries for image FS extraction
          --image-name-tag-with-digest-file string    Specify a file to save the image name w/ image tag w/ digest of the built image to.
          --image-name-with-digest-file string        Specify a file to save the image name w/ digest of the built image to.
          --insecure                                  Push to insecure registry using plain HTTP
          --insecure-pull                             Pull from insecure registry using plain HTTP
          --insecure-registry multi-arg type          Insecure registry using plain HTTP to push and pull. Set it repeatedly for multiple registries.
          --label multi-arg type                      Set metadata for an image. Set it repeatedly for multiple labels.
          --log-format string                         Log format (text, color, json) (default "color")
          --log-timestamp                             Timestamp in log output
          --no-push                                   Do not push the image to the registry
          --oci-layout-path string                    Path to save the OCI image layout of the built image.
          --push-retry int                            Number of retries for the push operation
          --registry-certificate key-value-arg type   Use the provided certificate for TLS communication with the given registry. Expected format is 'my.registry.url=/path/to/the/server/certificate'.
          --registry-mirror multi-arg type            Registry mirror to use as pull-through cache instead of docker.io. Set it repeatedly for multiple mirrors.
          --reproducible                              Strip timestamps out of the image to make it reproducible
          --single-snapshot                           Take a single snapshot at the end of the build.
          --skip-tls-verify                           Push to insecure registry ignoring TLS verify
          --skip-tls-verify-pull                      Pull from insecure registry ignoring TLS verify
          --skip-tls-verify-registry multi-arg type   Insecure registry ignoring TLS verify to push and pull. Set it repeatedly for multiple registries.
          --skip-unused-stages                        Build only used stages if defined to true. Otherwise it builds by default all stages, even the unnecessaries ones until it reaches the target stage / end of Dockerfile
          --snapshotMode string                       Change the file attributes inspected during snapshotting (default "full")
          --tarPath string                            Path to save the image in as a tarball instead of pushing
          --target string                             Set the target build stage to build
          --use-new-run                               Use the experimental run implementation for detecting changes without requiring file system snapshots.
      -v, --verbosity string                          Log level (trace, debug, info, warn, error, fatal, panic) (default "info")
    
    Use "executor [command] --help" for more information about a command.

### Example 01 - Build the image from Git Dockerfile and Push to Docker Hub

```
kubectl apply -f example-01.yml

kubectl logs -f kaniko

kubectl delete po kaniko
```

### Example 02 - Build the image from Git Dockerfile and Push to Docker Hub

Create your Token first:

```
kubectl create secret generic git-token --from-literal='GIT_TOKEN=<your-token>'
```

```
kubectl apply -f example-02.yml

kubectl logs -f kaniko

kubectl delete po kaniko
```

### Example 03 - Build the image from local directory and push to Docker Hub

The local directory must exist in the Node Group that the container is running.

```
kubectl apply -f example-03.yml

kubectl logs -f kaniko

kubectl delete po kaniko
```

### Example 03 - Build the image from local directory and without pushing to registry

The local directory must exist in the Node Group that the container is running.

```
kubectl apply -f example-03.yml

kubectl logs -f kaniko

kubectl delete po kaniko
```

### Build Log

```
$ kubectl logs -f kaniko
Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Compressing objects: 100% (8/8), done.
Total 11 (delta 2), reused 11 (delta 2), pack-reused 0
INFO[0001] Using dockerignore file: /kaniko/buildcontext/.dockerignore
INFO[0001] Retrieving image manifest python:3.9.7-alpine3.14
INFO[0001] Retrieving image python:3.9.7-alpine3.14 from registry index.docker.io
INFO[0001] Built cross stage deps: map[]
INFO[0001] Retrieving image manifest python:3.9.7-alpine3.14
INFO[0001] Returning cached image manifest
INFO[0001] Executing 0 build triggers
INFO[0001] Building stage 'python:3.9.7-alpine3.14' [idx: '0', base-idx: '-1']
INFO[0001] Unpacking rootfs as cmd RUN pip install flask requires it.
INFO[0004] RUN pip install flask
INFO[0004] Initializing snapshotter ...
INFO[0004] Taking snapshot of full filesystem...
INFO[0005] Cmd: /bin/sh
INFO[0005] Args: [-c pip install flask]
INFO[0005] Running: [/bin/sh -c pip install flask]
Collecting flask
  Downloading Flask-2.2.2-py3-none-any.whl (101 kB)
Collecting click>=8.0
  Downloading click-8.1.3-py3-none-any.whl (96 kB)
Collecting Jinja2>=3.0
  Downloading Jinja2-3.1.2-py3-none-any.whl (133 kB)
Collecting Werkzeug>=2.2.2
  Downloading Werkzeug-2.2.2-py3-none-any.whl (232 kB)
Collecting importlib-metadata>=3.6.0
  Downloading importlib_metadata-6.0.0-py3-none-any.whl (21 kB)
Collecting itsdangerous>=2.0
  Downloading itsdangerous-2.1.2-py3-none-any.whl (15 kB)
Collecting zipp>=0.5
  Downloading zipp-3.11.0-py3-none-any.whl (6.6 kB)
Collecting MarkupSafe>=2.0
  Downloading MarkupSafe-2.1.2-cp39-cp39-musllinux_1_1_x86_64.whl (29 kB)
Installing collected packages: zipp, MarkupSafe, Werkzeug, Jinja2, itsdangerous, importlib-metadata, click, flask
Successfully installed Jinja2-3.1.2 MarkupSafe-2.1.2 Werkzeug-2.2.2 click-8.1.3 flask-2.2.2 importlib-metadata-6.0.0 itsdangerous-2.1.2 zipp-3.11.0
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
WARNING: You are using pip version 21.2.4; however, version 22.3.1 is available.
You should consider upgrading via the '/usr/local/bin/python -m pip install --upgrade pip' command.
INFO[0012] Taking snapshot of full filesystem...
INFO[0013] WORKDIR /app
INFO[0013] Cmd: workdir
INFO[0013] Changed working directory to /app
INFO[0013] Creating directory /app
INFO[0013] Taking snapshot of files...
INFO[0013] COPY app.py .
INFO[0013] Taking snapshot of files...
INFO[0013] ENTRYPOINT ["python", "app.py"]
INFO[0013] Pushing image to hachikoapp/kaniko-demo-image:latest
```
