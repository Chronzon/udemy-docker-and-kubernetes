# Section 5 — Docker Compose with Multiple Local Containers

[Back to the repository guide](../README.md)

## Exercise

[visits-starter](visits-starter) combines a Node.js visit counter with Redis.

```bash
cd visits-starter
docker compose up --build
```

Open **http://localhost:4001** with the current port mapping.

## Port mapping: host first, container second

These configurations both map host port 4001 to container port 8081:

```bash
docker run -p 4001:8081 IMAGE
```

```yaml
ports:
  - "4001:8081"
```

The browser uses `localhost:4001`; Node still listens on 8081 inside the container. With Compose, the mapping is already in the YAML file, so `docker compose up` does not need a `-p` publishing flag.

`EXPOSE` documents the image's intended port. It does not publish the port to the host.

## Image names

With `build:` and no explicit `image:`, Compose derives the image name from the project and service, such as `visits-starter-nodejs-app`. The project name normally comes from the Compose project directory and can be overridden.

`image: visits-app:v1` selects an explicit image name and tag for the build. `container_name` names the container, not its image.

## Background execution versus application errors

`docker compose up -d` runs the stack in the background. It does not keep a crashing process alive or fix errors. `Exited (1)` indicates an unsuccessful process exit.

```bash
docker logs --tail 80 nodejs-app
# A container ID works too:
docker logs CONTAINER_ID
# Or select the Compose service:
docker compose logs nodejs-app
```

## Dependency errors encountered

- **`Object.hasOwn is not a function`:** Node 14 was running a dependency that used an API introduced in Node 16.9. The runtime and installed packages were incompatible.
- **`client.connect is not a function`:** the code used the Redis v4 client API with the Redis v2 library. Redis v4 supports explicit `connect()`, the `socket` configuration, and promise-based commands; Redis v2 uses a different API.

The Redis Node library and the Redis server image are separate dependencies. Updating one does not automatically update the other.

The current exercise uses Node 22 and Redis client 4.7.0. Express is still specified as `"*"`, so fresh installations can resolve different versions. Pin dependencies for repeatable course exercises, and use `npm ci` when a matching lockfile exists.
