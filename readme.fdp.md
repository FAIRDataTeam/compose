# Compose files for FAIR Data Point development and testing

>[NOTE]
> The development branch of the fdp (working title "v2") has been discontinued.
> For this reason, the `*/v2/*` compose files are now deprecated.

## Background 

To run a FAIR Data Point (FDP), we need at least a MongoDB database and the FDP (back-end) application.
Typically, this set-up is extended with an front-end application such as FDP-UI or FDP-client. 
The FDP uses an in-memory triple store by default, but an external triple store, such as GraphDB, can be added for persistence.
In addition, an FDP may communicate with other FDPs that are configured as FDP-index.

## Compose files

Re-usable Docker compose files for the components described above are defined in the [components] directory, for different major versions of the FDP.
Although this dir contains valid compose files, these are not intended for direct use. 
Instead, the intention is to use the compose files defined in the [ephemeral] and [persistent] directories.
As implied by the dir names, "ephemeral" configurations do not store data permanently, whereas the "persistent" configurations do.
So, if you tear down the "ephemeral" containers, their data is lost, whereas, if you tear down the "persistent" containers, their data remains in the corresponding Docker volumes and will be picked up on the next run.

To quickly set up a complete stack of database + FDP + FDP-client, just [run] the main compose file in [ephemeral/v1].

The [dev/*] subdirectories contain compose files for different development scenarios, for example:

- `fdp`: enables development of FDP source code by running database and FDP-client containers
- `fdp-ping`: enables development of FDP source code by running database, FDP-client, and FDP-index containers (for testing communication between FDP and FDP-index)
- `fdp-client`: enables development of FDP-client source code by running database and FDP containers

>[!NOTE]
>Re-use of components is achieved by merging files with the help of the [include] top-level element, in combination with [compose.override.yml] files.
>To check the actual compose configuration resulting from this merge, use the [config] command.
>For example:
>```bash
>cd fdp/persistent/v1
>docker compose config
>```

[components]: ./components
[compose.override.yml]: https://docs.docker.com/compose/how-tos/multiple-compose-files/merge/
[config]: https://docs.docker.com/reference/cli/docker/compose/config/
[ephemeral]: ./ephemeral
[ephemeral/v1]: ./ephemeral/v1
[ephemeral/v1/dev]: ./ephemeral/v1/dev
[include]: https://docs.docker.com/reference/compose-file/include/
[persistent]: ./persistent
[run]: ../readme.md#quickstart
