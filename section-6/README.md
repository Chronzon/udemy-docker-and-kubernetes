# Section 6 — Creating a Production-Grade Workflow

[Back to the repository guide](../README.md)

## Exercises

- [frontend](frontend): React with Create React App (CRA).
- [frontend-vite](frontend-vite): an alternative using Vite with different dependencies and scripts.

For the CRA exercise:

```bash
cd frontend
docker compose up --build
```

The development app is available at **http://localhost:3000**. Its Compose file also defines a test service.

## Choosing a Node version

The CRA project uses React 18.2 and `react-scripts` 5.0.1, whose declared minimum Node version is 14. Node 14 fits the historical course environment but is end-of-life.

Do not apply that version choice automatically to the Vite exercise: its tooling has different requirements. `node:lts-alpine` follows the current LTS tag rather than fixing a Node major version.

With a matching `package-lock.json`, `npm ci` installs locked versions and checks consistency with `package.json`. Avoid `"*"` dependencies when repeatability matters.

## Single-stage versus multi-stage

Each `FROM` starts a stage. Several `RUN` and `COPY` instructions under one `FROM` are still a single stage.

A production React image can build with Node, then copy only the output into Nginx:

```dockerfile
FROM node:14-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:1.28-alpine
COPY --from=builder /app/build /usr/share/nginx/html
```

The final image omits Node and the builder's `node_modules`, usually reducing size. Multiple stages alone do not guarantee a smaller image; keeping unnecessary files out of the final stage does.

Development still needs Node, dependencies, and the React dev server. Nginx does not need an old version just because the build uses an old Node runtime: it serves the generated files.

## Test commands in Compose

These are equivalent YAML lists:

```yaml
command: ["npm", "run", "test"]
```

```yaml
command:
  - npm
  - run
  - test
```

`command: npm run test` is also valid. A list containing the single item `- npm run test` instead identifies one executable name, including spaces.

Compose `command` overrides the image's `CMD`. For comparison, Kubernetes can split the executable and arguments into `command: ["npm"]` and `args: ["run", "test"]`.

## Source mounts and dependency volumes

```yaml
services:
  frontend:
    volumes:
      - .:/app
      - frontend-deps:/app/node_modules

volumes:
  frontend-deps:
```

`.:/app` bind-mounts local source. For these single-file Compose projects, relative paths are resolved from the Compose file's directory. Use `./frontend:/app` if the Compose file is one directory above the frontend.

`$(pwd)` is shell syntax, suitable for `docker run -v "$(pwd):/app" IMAGE`; Compose YAML does not execute it as a shell command.

A separate `/app/node_modules` mount prevents the source bind mount from replacing the container's dependencies with host files.

- `/app/node_modules` alone creates an anonymous volume with a generated name.
- `frontend-deps:/app/node_modules` uses a named volume declared at the top level.
- Without a volume `name:` override, Compose prefixes its project name.
- A volume `name: frontend-node-modules` sets the exact Docker volume name.

Existing dependency volumes can retain old packages after an image rebuild. Investigate the dependency volume when package changes do not appear at runtime; avoid deleting database volumes as part of that cleanup.
