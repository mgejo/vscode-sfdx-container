# VSCode SFDX Container

VSCode SFDX Container is a set of configuration files for Docker images used for containerized developer environments, meant for Salesforce developers.

[VSCode reference](https://code.visualstudio.com/docs/remote/containers)

[Salesforce reference](https://developer.salesforce.com/tools/vscode/en/user-guide/remote-development/)

The files included are a Docker image to build the containers, and a devcontainer.json file with configurations.


## Programs installed

- Java 11
- npm
- SFDX
- Git
- fish shell

## npm packages installed

- prettier
- prettier-plugin-apex 

## SFDX plugins installed

- salesforce/lwc-dev-server

## VSCode extensions installed

- [Salesforce Extension Pack](https://marketplace.visualstudio.com/items?itemName=salesforce.salesforcedx-vscode)
- [Apex Log Analyzer](https://marketplace.visualstudio.com/items?itemName=financialforce.lana)
- [Apex PMD](https://marketplace.visualstudio.com/items?itemName=chuckjonas.apex-pmd)
- [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)


## Reasoning
When developing multiple projects, it's easy to mistakenly make a commit to the wrong org, because SFDX may preserve the configuration of another project.

Containerizing the development environment solves this issue, and makes the files from different projects invisible to each other while executing inside the container.

There is a [Salesforce official image](https://hub.docker.com/r/salesforce/salesforcedx)  for VSCode development, but at least in my computer it can't open the browser for authentication or browsing the org (with force:org:open), and that's why I decided to build my own.

## Usage

Follow the steps [here](https://code.visualstudio.com/docs/remote/containers)

Create a folder with a meaningful name for your project and place the .devcontainer folder there, then open the project folder with VSCode, wait for the prompt to suggest to reopen in container, and click the corresponding button. The first time you build the image can take some time. Check the extensions tab to install all the extensions you want in the container.

To change to another container, first disconnect the one that is runnning, then open the project folder with VSCode and click the button when prompted. A good alternative is to use the [Project Manager](https://marketplace.visualstudio.com/items?itemName=alefragnani.project-manager) extension.

If developing inside a repository, you may want to add *.devcontainer/\** to /.git/info/exclude.

## Optimization

When you open a SFDX project folder, the Salesforce Extension Pack extensions load before Remote - Container, thus delaying the "Reopen in container" prompt.

In order to mitigate this, you can uninstall the Salesforce Extension pack from VSCode when disconnected from any containers and they will be loaded only when VSCode attaches to a container.

## Cleaning the cache

It's a good idea to clean the Docker cache every once in a while, so new versions of packages get installed inside the containers. To do so, you can run *docker builder prune -a*.
