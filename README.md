# VSCode SFDX Container

VSCode SFDX Container is a set of configuration files for Docker images used for containerized developer environments, meant for Salesforce developers.

- [VSCode reference](https://code.visualstudio.com/docs/remote/containers)

- [Salesforce reference](https://developer.salesforce.com/tools/vscode/en/user-guide/remote-development/)

The files included are a Docker image to build the containers, and a devcontainer.json file with configurations.

sfdx is installed using the installation tarball instead of npm, so updating is done using the `sfdx update` command.

npm is also present in the image, so npm scripts can be run.

The `devcontainer.json` file includes the VS Code extensions that are present in the [official Salesforce Docker image](https://hub.docker.com/r/salesforce/salesforcedx).

## Reasoning
Using containers for development makes it possible to keep a clean computing environment by minimizing the amount of software installed.

In the case of Salesforce development, it also has the benefits of persisting the default org between sessions on a same project, and uncluttering the list of all authorized orgs, keeping only the connected orgs relevant to each project.

Using containers also allows for the development of software in the same environment it will be deployed on, although this part is not relevant for Salesforce development.

While there is an official Salesforce Docker image, it has a couple issues:
- sfdx is installed from npm, which may cause issues when updating
- The containers using that image are incapable of opening the browser for authorization or as a result of force:org:open
- The prompt is too basic out of the box (refer to the image below)

<p align="center">
	<img src="https://user-images.githubusercontent.com/66442848/147389933-370c1921-f567-4789-abf5-9dd0ef0bdf2f.png" width="390" >
</p>

This project modifies the image from Microsoft's [vscode-remote-try-java](https://github.com/microsoft/vscode-remote-try-java) which comes with Java preinstalled and has a nice prompt out of the box  (refer to the image below).

<p align="center">
    <img src="https://user-images.githubusercontent.com/66442848/147389380-2d4e88bc-70de-4ac1-8de7-01fcef5eb98e.png" width="400" >
</p>


## Usage

Follow the steps [here](https://code.visualstudio.com/docs/remote/containers), using the `.devcontainers` folder from this project.

### Note for Windows users

If you are using Docker Desktop, it's a bad idea to reopen a folder in a container, since that will greatly reduce performance for any operation you do. An alternative is to build a container from the Dockerfile and attach the VS Code window to it.

The solution that worked best for me was to setup Docker Engine in a WSL distro and reopen the folders in containers from inside that, but there may be a more elegant solution involving Docker Volumes.

Also, WSL is a memory hog. Consider limiting how much RAM it can consume by using a [.wslconfig file](https://docs.microsoft.com/en-us/windows/wsl/wsl-config)


## Optimization

When you open a SFDX project folder, the Salesforce Extension Pack extensions may load before Remote - Container, thus increasing the time before being able to begin working.

If you don't have sfdx installed in your host, you will never face this issue. If you do have it installed, uninstalling the Salesforce Extension Pack from the host will solve it.

## Known problems and workarounds

- **Authorization callback fails** <br/>
Use auth:device:login. If authorizing a dev hub, use the flag -d.
- **force:source:retrieve fails** <br />
Retrieve from a manifest instead.
