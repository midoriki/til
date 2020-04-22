# Yarn offline mirror in local

This config help you use `yarn` to install and download npm packages to local folder for offline usage.
Very helpful if you're working in secured environment that does not allow development machine to connect to internet.

Create `.yarnrc` file in root directory, put in two line below then run `yarn install`

```sh
yarn-offline-mirror "./npm_packages"
yarn-offline-mirror-pruning true
```

NPM packages will be downloaded to `npm_packages` directory.

Then copy this directory to internal machine.

When install run `yarn install --offline`.