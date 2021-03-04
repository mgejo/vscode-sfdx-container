# VSCode SFDX Container

VSCode SFDX Container is a set of configuration files for Docker images used for containerized developer environments, meant for Salesforce developers.

[VSCode reference](https://code.visualstudio.com/docs/remote/containers)

[Salesforce reference](https://developer.salesforce.com/tools/vscode/en/user-guide/remote-development/)

The files included are a Docker image to build the containers, and a devcontainer.json file with configurations.


The containers have Java 11, npm, SFDX, and Git installed

## Reasoning
When developing multiple projects, it's easy to mistakenly make a commit to the wrong org, because SFDX may preserve the configuration of another project.

Containerizing the development environment solves this issue, and makes the files from different projects invisible to each other while executing inside the container.

There is a [Salesforce official image](https://hub.docker.com/r/salesforce/salesforcedx)  for VSCode development, but at least in my computer it can't open the browser for authentication or browsing the org (with force:org:open), and that's why I decided to build my own.


## Usage

Follow the steps [here](https://code.visualstudio.com/docs/remote/containers)

Create a folder with a meaningful name for your project and place the .devcontainer folder there, then open the project folder with VSCode, wait for the prompt to suggest to reopen in container, and click the corresponding button. The first time you build the image can take some time. Check the extensions tab to install all the extensions you want in the container, and after they finish installing you may authorize an org to start development.

To change to another container, first disconnect the one that is runnning, then open the project folder with VSCode and click the button when prompted.

## Optimization

When you open a SFDX project folder, the Salesforce Extension Pack extensions load before Remote - Container, thus delaying the "Reopen in container" prompt.

In order to mitigate this, you can uninstall the Salesforce Extension pack from VSCode outside the containers and they will be loaded only once, inside the containers.