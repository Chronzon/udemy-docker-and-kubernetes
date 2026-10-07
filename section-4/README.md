# Section 4 — Making Real Projects with Docker

[Back to the repository guide](../README.md)

## Exercise

[simpleweb](simpleweb) packages a small Node.js application into a Docker image.

```bash
cd simpleweb
docker build -t simpleweb .
```

## Images, containers, and build contexts

An image packages the application and its runtime. A container is an instance running from that image. Building an image does not run the application's startup command or prove that it can connect to other services.

In `docker build -f Dockerfile.dev .`, `-f` selects the Dockerfile and the final `.` selects the build context. Files referenced by `COPY` must be inside that context. Their paths are relative to the context, not necessarily to the Dockerfile's directory.

`--rm` is the correct build flag, rather than `-rm`, and is already enabled by default. It controls removal of intermediate containers where applicable. `docker run --rm` instead removes the application container when it exits.

## Runtime compatibility

The current Dockerfile uses `node:14-alpine` to follow the older exercise. Node 14 is end-of-life. Check the installed dependencies' Node requirements before assuming an older runtime will work with newly resolved packages.

A successful build is the first check. Starting the container and inspecting its logs is the next check.
