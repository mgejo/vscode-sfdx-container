# VSCode SFDX Container

VSCode SFDX Container is a set of configuration files for Docker images used for containerized developer environments, meant for Salesforce developers.

-   [VSCode reference](https://code.visualstudio.com/docs/remote/containers)

-   [Salesforce reference](https://developer.salesforce.com/tools/vscode/en/user-guide/remote-development/)

The files included are a `Dockerfile`, and a `devcontainer.json` file with configurations.

SFDX is installed using the installation tarball instead of npm, so updating is done using the `sfdx update` command.

npm is also present in the resulting image, so npm scripts can be run.

The following extensions are included:

-   [Salesforce Extension Pack](https://marketplace.visualstudio.com/items?itemName=salesforce.salesforcedx-vscode)
-   [XML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-xml)
-   [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
-   [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

The included extensions are the ones that are present in the official [VS Code Container Definition for SFDX](https://github.com/microsoft/vscode-dev-containers/blob/main/containers/sfdx-project/.devcontainer/devcontainer.json). You may modify this list by editing `devcontainer.json`.

## Reasoning

Using containers for development makes it possible to keep a clean computing environment by minimizing the amount of software installed.

In the case of Salesforce development, it also has the benefits of persisting the default org between sessions on a same project, and uncluttering the list of all authorized orgs, keeping only the connected orgs relevant to each project.

Using containers also allows for the development of software in the same environment it will be deployed on, although this part is not relevant for Salesforce development.

While there is an official [Salesforce Docker image](https://hub.docker.com/r/salesforce/salesforcedx), it has a couple issues:

-   SFDX is installed from npm, which may cause issues when updating
-   The containers using that image are incapable of opening the browser for authorization or as a result of `sfdx force:org:open`
-   The prompt is too basic out of the box

<p align="center">
	<img src="https://user-images.githubusercontent.com/66442848/147389982-cb7da232-072f-4772-87d0-0284132b88b3.png" width="400" >
</p>

This project modifies the `Dockerfile` from Microsoft's [vscode-remote-try-java](https://github.com/microsoft/vscode-remote-try-java) which comes with Java preinstalled and has a nicer prompt out of the box.

<p align="center">
    <img src="https://user-images.githubusercontent.com/66442848/147389380-2d4e88bc-70de-4ac1-8de7-01fcef5eb98e.png" width="400" >
</p>

An additional consideration is that the official Container Definition for SFDX runs containers as root by default. If using a Linux host, this causes files or folders created in the container to be created as root on the local side as well.

## Usage

Follow the steps [here](https://code.visualstudio.com/docs/remote/containers), using the `.devcontainer` folder from this project.

### Note for Windows users

If you are using Docker Desktop, it's a bad idea to reopen a folder in a container, since that will greatly reduce performance for any operation you do. An alternative is to build a container from the Dockerfile and attach the VS Code window to it.

Although there may be a more elegant solution, what worked best for me was to setup Docker Engine in a WSL distro and reopen the folders in containers from inside that, without using Docker Desktop at all.

Also, WSL is a memory hog. Consider limiting how much RAM it can consume by using a [.wslconfig file](https://docs.microsoft.com/en-us/windows/wsl/wsl-config).

## Optimization

When you open a SFDX project folder, the Salesforce Extension Pack extensions may load before Remote - Container, thus increasing the time before being able to begin working.

If you don't have SFDX installed in your host, you will never face this issue. If you do have it installed, uninstalling the Salesforce Extension Pack from the host will solve it.

## Known problems and workarounds

-   **Authorization callback fails** <br/>
    Use `sfdx auth:device:login`. If authorizing a dev hub, use the flag -d.
-   **sfdx force:source:retrieve fails** <br />
    Retrieve from a manifest instead.
