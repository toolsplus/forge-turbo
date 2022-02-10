# Forge Monorepo Example

Simple example of a Forge app with Custom UI, based on a monorepo structure using [Turborepo](https://turborepo.org/).

## Goals

* Simple build and dev workflow
* Allows extracting shared logic to separate packages
* Easily use shared logic in Forge backend (FAAS) and custom UI frontend

## Getting started

Make sure you have the required dependencies to develop Forge apps installed. See [Getting started](https://developer.atlassian.com/platform/forge/getting-started/) for instructions.

After that, run `npm install` from the project root. This will install dependencies for the root project and all sub-projects (custom UIs and Forge app).

Register the Forge app by running the following command from the `apps/forge` folder 

    forge register

Build the app from the project root directory by running

    npm run build

This will build shared dependencies (`packages`), custom UI project and finally deploy the Forge app.

Finally, install the app, again by running the command from the `apps/forge` folder

    forge install

## Usage

### Development

> The development process does not work yet as expected (see Known Issues below).

    npm run dev

To work around this issue do the following:

1. Remove the `dev` script from `apps/forge/package.json`
2. Run `npm run dev` from the project root
3. Run `forge tunnel` from `apps/forge` directory

### Build

Simply run the following command from the project root:

    npm run build

## Known issues

### Development workflow does not work as expected
 
Running `turbo run dev` (or `npm run dev`) from the project root does not work as expected. Forge tunnel complains `the input device is not a TTY` which seems to have to do with the [Docker process startup](https://stackoverflow.com/questions/43099116/error-the-input-device-is-not-a-tty). 

Additionally, the Forge linter produces the errors, but they are not fatal. Linting errors seem to be an issue with relative paths in the `manifest.yml`:

```
=== Running forge lint...

/app/manifest.yml
23:10   error    missing index.html file in directory (../issue-panel/build) is being referenced by a custom UI resource in jira:issuePanel module  valid-resource-required

23:10   error    missing directory '../issue-panel/build' is being referenced by 'issue-panel-resource' in resources  valid-resource-required

27:10   error    missing index.html file in directory (../issue-glance/build) is being referenced by a custom UI resource in jira:issueGlance module  valid-resource-required

27:10   error    missing directory '../issue-glance/build' is being referenced by 'issue-glance-resource' in resources  valid-resource-required

```

### Running specific Forge CLI commands

To run specific Forge CLI commands (`forge register`, `forge install`, etc.), you should run them directly from the `apps/forge` directory. However, it should not be too difficult to tweak Turborepo to allow running them from the project root as well.