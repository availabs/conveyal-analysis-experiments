# Running Conveyal Analysis UI

## System Dependencies

### Installing Node.js

See the official instructions [here](https://nodejs.org/en/download/package-manager/)

For Ubuntu, form the NodeSource [instructions](https://github.com/nodesource/distributions/blob/master/README.md#installation-instructions)

```sh
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get install -y nodejs
```

The Conveyal Analysis UI requires a specific Node.js version to run.
For conveyal/analysis-ui version 4.12.0, that Node.js version is 10.x.

We can use the **n** npm package to manage Node.js versions.

```sh
sudo npm install -g n
sudo n 10
```

Install the yarn package manager.
(_npm_ failed to install project dependencies for analysis-ui v4.12.0)

```sh
sudo npm install -g yarn
```

## Get the source code

```sh
git clone https://github.com/conveyal/analysis-ui.git
```

## Checkout a stable release (c66be9e = v4.12.0)

```sh
cd analysis-ui
git checkout c66be9e
```

## Create the Configuration File

Create a file named _.env_ with the following

```sh
# Private
AUTH0_BASE_URL=http://localhost:3000
AUTH0_CLIENT_ID=
AUTH0_CLIENT_SECRET=
AUTH0_HTTP_TIMEOUT=10000
AUTH0_ISSUER_BASE_URL=https://conveyal.eu.auth0.com
AUTH0_SECRET=
AUTH0_SESSION_ABSOLUTE_DURATION=false
AUTH0_SESSION_ROLLING_DURATION=2592000
MONGODB_URL=mongodb://127.0.0.1:27017/analysis

# Public. No Auth0 creds needed if AUTH_DISABLED = true
NEXT_PUBLIC_ADMIN_ACCESS_GROUP=local
NEXT_PUBLIC_API_URL=http://localhost:7070
NEXT_PUBLIC_AUTH_DISABLED=true
NEXT_PUBLIC_BASEMAP_DISABLED=false
NEXT_PUBLIC_CYPRESS=false
NEXT_PUBLIC_MAPBOX_ACCESS_TOKEN=false
NEXT_PUBLIC_MAPBOX_STYLE=mapbox/light-v10
```

You must set **NEXT_PUBLIC_MAPBOX_ACCESS_TOKEN**
to your Mapbox public [access token](https://docs.mapbox.com/help/getting-started/access-tokens/).

Note: If you are running MongoDB with authentication enabled, you also will need
to set **MONGODB_URL** appropriately. For example:

`MONGODB_URL=mongodb://root:password@127.0.0.1:27017/analysis?authSource=admin`

## Install the project dependencies

```sh
yarn install --frozen-lockfile
```

## Build the project

```sh
yarn build
```

## Start the server

```sh
yarn start
```
