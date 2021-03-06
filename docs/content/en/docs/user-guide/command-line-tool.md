---
title: "Command-line tool: pipectl"
linkTitle: "Command-line tool: pipectl"
weight: 22
description: >
  This page describes how to install and use pipectl to manage PipeCD's resources.
---

Besides using web UI, PipeCD also provides a command-line tool, pipectl, which allows you to run commands against your project's resources.
You can use pipectl to add and sync applications, wait for a deployment status.

## Installation

### Binary

1. Download the appropriate version for your platform from [PipeCD Releases](https://github.com/pipe-cd/pipe/releases).

    We recommend using the latest version of pipectl to avoid unforeseen issues.

2. Make the pipectl binary executable.

    ``` console
    chmod +x ./pipectl
    ```

3. Move the binary to your PATH.

    ``` console
    sudo mv ./pipectl /usr/local/bin/pipectl
    ```

4. Test to ensure the version you installed is up-to-date.

    ``` console
    pipectl version
    ```

### Docker

We are storing every version of docker image for pipectl on Google Cloud Container Registry. You can use it with the following address:

```
gcr.io/pipecd/pipectl:VERSION
```

## Authentication

In order for pipectl to authenticate with PipeCD's control-plane, it needs an API key, which can be created from `Settings/API Key` tab on the web UI.
There are two kinds of key role: `READ_ONLY` and `READ_WRITE`. Depending on the command, it might require an appropriate role to execute.

![](/images/settings-api-key.png)
<p style="text-align: center;">
Adding a new API key from Settings tab
</p>

When executing a command of pipectl you have to specify either a string of API key via `--api-key` flag or a path to the API key file via `--api-key-file` flag. 

## Usage

### Help

Run `help` to know the available commands:

``` console
$ pipectl --help

The command line tool for PipeCD.

Usage:
  pipectl [command]

Available Commands:
  application Manage application resources.
  deployment  Manage deployment resources.
  help        Help about any command
  version     Print the information of current binary.
```

### Adding a new application

Add a new application into the project:

``` console
pipectl application add \
    --address=CONTROL_PLANE_API_ADDRESS \
    --api-key=API_KEY \
    --app-name=simple \
    --app-kind=KUBERNETES \
    --env-id=ENV_ID \
    --piped-id=PIPED_ID \
    --cloud-provider=kubernetes-default \
    --repo-id=examples \
    --app-dir=kubernetes/simple \
```

Run `help` to know what command flags should be specified:

``` console
$ pipectl application add --help

Add a new application.

Usage:
  pipectl application add [flags]

Flags:
      --app-dir string            The relative path from the root of repository to the application directory.
      --app-kind string           The kind of application. (KUBERNETES|TERRAFORM|LAMBDA|CLOUDRUN)
      --app-name string           The application name.
      --cloud-provider string     The cloud provider name. One of the registered providers in the piped configuration.
      --config-file-name string   The configuration file name. Default is .pipe.yaml (default ".pipe.yaml")
      --env-id string             The ID of environment where this application should belong to.
  -h, --help                      help for add
      --piped-id string           The ID of piped that should handle this applicaiton.
      --repo-id string            The repository ID. One the registered repositories in the piped configuration.

Global Flags:
      --address string                     The address to control-plane api.
      --api-key string                     The API key used while authenticating with control-plane.
      --api-key-file string                Path to the file containing API key used while authenticating with control-plane.
      --cert-file string                   The path to the TLS certificate file.
      --insecure                           Whether disabling transport security while connecting to control-plane.
      --log-encoding string                The encoding type for logger [json|console|humanize]. (default "humanize")
      --log-level string                   The minimum enabled logging level. (default "info")
```

### Syncing an application

- Send a request to sync an application and exit immediately when the deployment is triggered:

``` console
pipectl application sync \
    --address=CONTROL_PLANE_API_ADDRESS \
    --api-key=API_KEY \
    --app-id=APPLICATION_ID
```

- Send a request to sync an application and wait until the triggered deployment reaches one of the specified statuses:

``` console
pipectl application sync \
    --address=CONTROL_PLANE_API_ADDRESS \
    --api-key=API_KEY \
    --app-id=APPLICATION_ID \
    --wait-status=DEPLOYMENT_SUCCESS,DEPLOYMENT_FAILURE
```

### Waiting a deployment status

Wait until a given deployment reaches one of the specified statuses:

``` console
pipectl deployment wait-status \
    --address=CONTROL_PLANE_API_ADDRESS \
    --api-key=API_KEY \
    --deployment-id=DEPLOYMENT_ID \
    --status=DEPLOYMENT_SUCCESS
```

### You want more?

We always want to add more needed commands into pipectl. Please let us know what command do you want to add by creating issues in the [pipe-cd/pipe ](https://github.com/pipe-cd/pipe/issues) repository. We also welcome your pull request to add the command.
