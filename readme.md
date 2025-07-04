# FAIRDataTeam compose collection

This repository contains a collection of Docker compose files for development and testing of FAIRDataTeam applications 
like [FAIR Data Point], [FAIR Data Train], etc.

>[!IMPORTANT]
>These files are ***not*** intended for use in production.

## Quickstart

1. `cd` into the directory for the desired configuration, e.g. `cd fdp/ephemeral/v1` (this important because we rely on autodetection of `compose.override.yml` files)
2. set up: `docker compose up -d`
3. visit http://localhost in your browser to play around with the application
4. tear down: `docker compose down` (from the same directory)

## Description

Re-usable Docker compose services are specified in separate directories such as [fdp/components](./fdp/components).
These components are used to construct compose files for various use cases.

For example, [fdp/ephemeral/v1/compose.yml](fdp/ephemeral/v1/compose.yml) defines a minimal setup, consisting of the following Docker containers:
 
- mongodb
- fdp v1.x
- fdp-client v1.x

This uses the FDP's default in-memory triplestore, and mongodb data is non-persistent (ephemeral), because no Docker volumes are defined. If you tear these containers down, the data are gone.

If you want persistent storage, use one of the setups from the [fdp/persistent](./fdp/persistent) directory, which *do* use Docker volumes.

Similarly, if you're interested in the FDP index, run the setup from [fdp/ephemeral/v1/index](./fdp/ephemeral/v1/index), for example.

## Usage

Although it is possible to specify one or more Docker compose files on the command line, using the `-f` option, this repo relies on the default Docker compose behavior which is to automatically [merge] the following files:

- `compose.yml`
- `compose.override.yml` (optional)

This way, we can `cd` into the desired directory and then simply call `docker compose` commands, without having to specify files explicitly using `-f`. 
Moreover, some configurations rely on this method to override certain service attributes. 

For example, compare this approach to merge `compose.yml` with `compose.override.yml`:

```bash
cd fdp/ephemeral/v1/dev/fdp
docker compose up -d
```

with the alternative:

```bash
docker compose -f fdp/ephemeral/v1/dev/fdp/compose.yml -f fdp/ephemeral/v1/dev/fdp/compose.override.yml up -d
```

To check the actual result of such a [merge], we can use the [`compose config`] command:

```bash
docker compose config
``` 

If desired, specific Docker image versions can be specified using environment variables, before calling `compose up`.
For example:

```bash
export FDP_CLIENT_VERSION=2.0.0-alpha.2
```

Refer to the compose files in the `components` directories to see the environment variable names. 

## Troubleshooting

### Compose up fails with "no matching manifest" on macOS

On a mac with Apple silicon (or on other computers with arm64 processor architecture), `docker compose up -d` may produce an error like the following:

```none
no matching manifest for linux/arm64/v8 in the manifest list entries
``` 
 
This is because the latest versions of the fdp-client (`>1.17.0`)  don't have Docker images for arm64 architectures, for reasons explained in FAIRDataTeam/FAIRDataPoint-client#188.

While FAIRDataTeam/FAIRDataPoint-client#188 remains open, here are some temporary workarounds:

- roll back to an older version of the client, using env variables, e.g. `export FDP_CLIENT_VERSION=1.17.0`
- (not tested) force emulation by specifying `platform: linux/amd64` for the `fdp-client` service in the compose file

Check the `OS/ARCH` column on [Docker hub] to see which client versions do support `linux/arm64`. 

## Work in progress...

- Compose files for fdt need to be added (fair data train etc.)

[`compose config`]: https://docs.docker.com/reference/cli/docker/compose/config/
[merge]: https://docs.docker.com/compose/how-tos/multiple-compose-files/merge/
[FAIR Data Point]: https://github.com/FAIRDataTeam/FAIRDataPoint
[FAIR Data Train]: https://github.com/FAIRDataTeam/FAIRDataTrain
[Docker hub]: https://hub.docker.com/r/fairdata/fairdatapoint-client/tags
