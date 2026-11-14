The compose configurations in the `dev` folder are to be used when you're running one of the components from source (on the Docker host machine).

The name of the subfolder corresponds with the component that is running from source.

For example, if you're running the `fdp` from source, you can use the compose file from `dev/fdp` to run `mongo` and `fdp-client` services.

Similarly, if you're running an `fdp-index` from source, you can use `dev/fdp-index` to run `mongo`, `fdp-index-client`, and an additional `fdp` that sends frequent pings to your index.

The `dev/fdp-ping` is a special case. You can use this when running `fdp` from source. It provides an additional `fdp-index` service, useful for testing your `fdp`'s ping functionality.
