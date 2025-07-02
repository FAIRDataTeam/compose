## Compose files for FAIR Data Point development and testing

To run a FAIR Data Point (FDP), we need at least a database, such as MongoDB or PostgreSQL, depending on the FDP version, and the FDP (back-end) application.
Typically, this set-up is extended with an FDP-client (front-end) application.
The FDP uses an in-memory triple store by default, but an external triple store, such as GraphDB, can be added for persistence.
In addition, an FDP may communicate with other FDPs that are configured as FDP-index.

Re-usable Docker compose files for the components described above are defined in the [components] directory, for different major versions of the FDP.
Although this dir contains valid compose files, these are not intended for direct use. 
Instead, the intention is to use the compose files defined in the [ephemeral] and [persistent] directories.
As implied by the dir names, "ephemeral" configurations do not store data permanently, whereas the "persistent" configurations do.
So, if you tear down the "ephemeral" containers, their data is lost, whereas, if you tear down the "persistent" containers, their data remains in the corresponding Docker volumes and will be picked up on the next run.

To quickly set up a complete stack of database + FDP + FDP-client, just [run] the main compose file in either [ephemeral/v1] or [ephemeral/v2].

The [ephemeral/v1/dev] and [ephemeral/v2/dev] directories contain compose files for different development scenarios, for example:

- `fdp`: enables development of FDP source code by running database and FDP-client containers
- `fdp-ping`: enables development of FDP source code by running database, FDP-client, and FDP-index containers (for testing communication between FDP and FDP-index)
- `fdp-client`: enables development of FDP-client source code by running database and FDP containers

[components]: ./components
[ephemeral]: ./ephemeral
[ephemeral/v1]: ./ephemeral/v1
[ephemeral/v2]: ./ephemeral/v2
[ephemeral/v1/dev]: ./ephemeral/v1/dev
[ephemeral/v2/dev]: ./ephemeral/v2/dev
[persistent]: ./persistent
[run]: ../readme.md#quickstart
