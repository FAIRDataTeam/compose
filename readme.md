# FAIRDataTeam compose collection

This repository contains a collection of Docker compose files for development and testing of the [FAIR Data Point] reference implementation.

>[!IMPORTANT]
>These files are ***not*** intended for use in production, because they do not take into account important production aspects like security, data management, etc.
>Nevertheless, the "persistent" stack could be used as a *starting point* for developing your own production configuration.

>[!WARNING]
>This repository does ***not*** conform to any semantic versioning.
>Because the compose repo is a development tool, we reserve the right to make breaking changes without prior warning.
>If there *are* breaking changes, these will be described in the [release notes], so please keep an eye on those.

>[!NOTE]
>Release 2026.21 introduced a major change in project file paths:
>
>- removed `fdp` and `v1` subdirectories
>- renamed `ephemeral` to `transient`
>- removed all `v2` configurations
>- changed file extensions from `.yml` to `.yaml`
>
>For example, the old `fdp/ephemeral/v1/compose.yml` is now `transient/compose.yaml`.

## Quickstart

1. `cd` into the directory for the desired configuration, e.g. `cd transient` (this is important because we rely on autodetection of `compose.override.yaml` files)
2. set up: `docker compose up -d`
3. visit http://localhost in your browser to play around with the application
4. tear down: `docker compose down` (from the same directory)

Note: To remove *persistent* data, it is convenient to use `docker compose down --volumes` (or `-v`).
Do be careful though, because this is irreversible.

## Description

Re-usable Docker compose services are specified in separate directories such as [components](./components).
These components are used to construct compose files for various use cases.

For example, [transient/compose.yaml](transient/compose.yaml) defines a minimal setup, consisting of the following Docker containers:
 
- mongodb
- fdp (v1.x)
- fdp-client (v1.x)
- fdp-ui (dev)

This uses the FDP's default in-memory triplestore, and mongodb data is non-persistent (transient), because no Docker volumes are defined.
If you tear these containers down, the data are gone.

>[!NOTE]
>For now we include both the `fdp-client` and the new `fdp-ui` which is still under development.
>The `fdp-client` component is now deprecated and will be removed when a stable `fdp-ui` is released.  

If you want persistent storage, use one of the setups from the [persistent](./persistent) directory, which *do* use Docker volumes.

Similarly, if you're interested in the FDP index, run the setup from [transient/index](./transient/index), for example.

## Usage

Although it is possible to specify one or more Docker compose files on the command line, using the `-f` option, this repo relies on the default Docker compose behavior which is to automatically [merge] the following files:

- `compose.yaml`
- `compose.override.yaml` (optional)

This way, we can `cd` into the desired directory and then simply call `docker compose` commands, without having to specify files explicitly using `-f`. 
Moreover, some configurations rely on this method to override certain service attributes. 

For example, compare this approach to merge `compose.yaml` with `compose.override.yaml`:

```bash
cd transient/dev/fdp
docker compose up -d
```

with the alternative:

```bash
docker compose -f transient/dev/compose.yaml -f transient/dev/fdp/compose.override.yaml up -d
```

To check the actual result of such a [merge], we can use the [`compose config`] command:

```bash
docker compose config
``` 

If desired, specific Docker image versions can be specified using environment variables, before calling `compose up`.
For example:

```bash
export FDP_VERSION=1.22.0
export FDP_CLIENT_VERSION=1.18.1
export FDP_UI_VERSION=0.2.0
export MONGO_VERSION=8.2.6
export GRAPHDB_VERSION=10.8.13
```

Refer to the compose files in the `components` directories to see the environment variable names. 

If you want to persist the use of specific versions, create a [`.env` file] in the applicable directory with the desired environment variables.
This file will be picked up automatically when you run `docker compose`. See [docker environment variables] for more information.

## More details

See [readme.fdp.md](./readme.fdp.md) for more information about the FDP stacks.

[`compose config`]: https://docs.docker.com/reference/cli/docker/compose/config/
[merge]: https://docs.docker.com/compose/how-tos/multiple-compose-files/merge/
[FAIR Data Point]: https://github.com/FAIRDataTeam/FAIRDataPoint
[Docker hub]: https://hub.docker.com/r/fairdata/fairdatapoint-client/tags
[`.env` file]: https://docs.docker.com/compose/how-tos/environment-variables/variable-interpolation/#env-file
[docker environment variables]: https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/
[release notes]:  https://github.com/FAIRDataTeam/compose/releases
