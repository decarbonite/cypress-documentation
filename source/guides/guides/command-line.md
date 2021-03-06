---
title: Command Line
---

{% note info %}
# {% fa fa-graduation-cap %} What You'll Learn

- How to run Cypress from the command line
- How to specify which spec files to run
- How to launch other browsers
- How to record your tests
{% endnote %}

# Installation

This guide assumes you've already read our {% url 'Installing Cypress' installing-cypress %} guide and installed Cypress as an `npm` module. After installing you'll be able to execute all of the commands below.

You can alternatively require and run Cypress as a node module using our {% url "Module API" module-api %}.

{% note warning %}
For brevity we've omitted the full path to the cypress executable in each command.

You'll need to prefix each command with:

- `$(npm bin)/cypress`
- ...or...
- `./node_modules/.bin/cypress`

Or just add cypress commands to the `scripts` field in your `package.json` file.
{% endnote %}

# Commands

## `cypress run`

Runs Cypress to completion. By default will run all tests headlessly in the `Electron` browser.

### Run tests

```shell
cypress run [options]
```

**Options**

Option | Description
------ |  ---------
`-b`, `--browser`  | Specify different browser to run tests in
`-c`, `--config`  | Specify configuration
`-e`, `--env`  | Specify environment variables
`-h`, `--help`  | Output usage information
`-k`, `--key`  | Specify your secret record key
`-p`, `--port`  | Override default port
`-P`, `--project` | Path to a specific project
`-r`, `--reporter`  | Specify a mocha reporter
`-o`, `--reporter-options`  | Specify mocha reporter options
`-s`, `--spec`  | Specify the specs to run
`--no-exit` | Keep Cypress open after all tests run
`--record`  | Whether to record the test run
`--headed`  | Display the Electron browser instead of running headlessly

### Run tests specifying browser

```shell
cypress run --browser chrome
```

{% note warning %}
Cypress will attempt to find all supported browsers available on your system. If Cypress cannot find the browser you should turn on debugging for additional output.
{% endnote %}

```shell
DEBUG=cypress:launcher cypress run --browser chrome
```

### Run tests in Electron in headed mode

By default, Cypress will run tests in Electron headlessly.

Passing `--headed` will force Electron to be shown. This matches how you run Electron in interactive mode.

```shell
cypress run --headed
```

### Run tests specifying configuration

Read more about {% url 'environment variables' environment-variables %} and {% url 'configuration' configuration %}.

```shell
cypress run --config pageLoadTimeout=100000,watchForFileChanges=false
```

### Run tests specifying environment variables

```shell
cypress run --env host=api.dev.local
```

### Run tests specifying a port

```shell
cypress run --port 8080
```

### Run tests specifying a mocha reporter

```shell
cypress run --reporter json
```

### Run tests specifying mochas reporter options

```shell
cypress run --reporter-options mochaFile=result.xml,toConsole=true
```

### Run tests specifying a single test file to run instead of all tests

```shell
cypress run --spec 'cypress/integration/examples/actions.spec.js'
```

### Run tests specifying a glob of where to look for test files

```shell
cypress run --spec 'cypress/integration/login/**/*'  ## Note: quotes required
```

### Run tests specifying multiple test files to run

```shell
cypress run --spec 'cypress/integration/examples/actions.spec.js,cypress/integration/examples/files.spec.js'
```

### Run tests specifying a project

By default, Cypress expects your `cypress.json` to be found where your `package.json` is. However, you can point Cypress to run in a different location.

This enables you to install Cypress in a top level `node_modules` folder but run Cypress in a nested folder. This is also helpful when you have multiple Cypress projects in your repo.

To see this in action we've set up an {% url 'example repo to demonstrate this here' https://github.com/cypress-io/cypress-test-nested-projects %}.

```shell
cypress run --project ./some/nested/folder
```

### Run and record video of tests

Record video of tests running after {% url 'setting up your project to record' dashboard-service#Setup %}. After setting up your project you will be given a **Record Key**.

```shell
cypress run --record --key <record_key>
```

If you set the **Record Key** as the environment variable `CYPRESS_RECORD_KEY`, you can omit the `--key` flag.

You'd typically set this environment variable when running in {% url 'Continuous Integration' continuous-integration %}.

```shell
export CYPRESS_RECORD_KEY=abc-key-123
```

Now you can omit the `--key` flag.

```shell
cypress run --record
```

You can {% url 'read more about recording runs here' dashboard-service#Setup %}.

### Run without exit

To prevent Cypress from exiting after running tests with `cypress run`, use `--no-exit`.

You can pass `--headed --no-exit` in order to view the **command log** or have access to **developer tools** after a `spec` has run.

```shell
cypress run --headed --no-exit
```

## `cypress open`

Opens the Cypress Test Runner in interactive mode.

### Open Cypress

```shell
cypress open [options]
```

**Options**

Options passed to `cypress open` will automatically be applied to the project you open. These persist on all projects until you quit the Cypress Test Runner. These options will also override values in `cypress.json`

Option | Description
------ | ---------
`-c`, `--config`  | Specify configuration
`-d`, `--detached` | Open Cypress in detached mode
`-e`, `--env`  | Specify environment variables
`-h`, `--help`  | Output usage information
`-p`, `--port`  | Override default port
`-P`, `--project` | Path to a specific project
`--global` | Run in global mode

### Open Cypress projects specifying port

```shell
cypress open --port 8080
```

### Open Cypress projects specifying configuration

```shell
cypress open --config pageLoadTimeout=100000,watchForFileChanges=false
```

### Open Cypress projects specifying environment variables

```shell
cypress open --env host=api.dev.local
```

### Open Cypress in global mode

Opening Cypress in global mode is useful if you have multiple nested projects but want to share a single global installation of Cypress. In this case you can add each nested project to the Cypress in global mode, thus giving you a nice UI to switch between them.

```shell
cypress open --global
```

## `cypress verify`

Verify that Cypress is installed correctly and is executable.

```shell
cypress verify
✔  Verified Cypress! /Users/jane/Library/Caches/Cypress/3.0.0/Cypress.app
```

## `cypress version`

Equivalent: `cypress --version`, `cypress -v`

Output both the versions of the installed Cypress binary application and NPM module.
In most cases they will be the same, but could be different if you have installed a different version of the NPM package and for some reason could not install the matching binary.

```shell
cypress version
Cypress package version: 3.0.0
Cypress binary version: 3.0.0
```

## `cypress cache [command]`

Commands for managing the global Cypress cache. The Cypress cache applies to all installs of Cypress across your machine, global or not.

### `cypress cache path`

Print the `path` to the Cypress cache folder.

```shell
cypress cache path
/Users/jane/Library/Caches/Cypress
```

### `cypress cache list`

Print all existing installed versions of Cypress. The output will be a **space delimited** list of version numbers.

```shell
cypress cache list
3.0.0 3.0.1 3.0.2
```

### `cypress cache clear`

Clear the contents of the Cypress cache. This is useful when you want Cypress to clear out all installed versions of Cypress that may be cached on your machine. After running this command, you will need to run `cypress install` before running Cypress again.

```shell
cypress cache clear
```

# Debugging Commands

Cypress is built using the {% url 'debug' https://github.com/visionmedia/debug %} module. That means you can receive helpful debugging output by running Cypress with this turned on prior to running `cypress open` or `cypress run`.

**On Mac or Linux:**
```shell
DEBUG=cypress:* cypress open
```

```shell
DEBUG=cypress:* cypress run
```
**On Windows:**

```shell
set DEBUG=cypress:*
```

```shell
set DEBUG=cypress:*
```

Cypress is a rather large and complex project involving a dozen or more submodules, and the default output can be overwhelming.

**To filter debug output to a specific module**

```shell
DEBUG=cypress:cli cypress run
```

```shell
DEBUG=cypress:launcher cypress run
```

...or even a 3rd level deep submodule

```shell
DEBUG=cypress:server:project cypress run
```
