# VSCode SFDX Container

VSCode SFDX Container is a set of configuration files for Docker images used for containerized developer environments, meant for Salesforce developers.

[VSCode reference](https://code.visualstudio.com/docs/remote/containers)

[Salesforce reference](https://developer.salesforce.com/tools/vscode/en/user-guide/remote-development/)

The files included are a Docker image to build the containers, and a devcontainer.json file with configurations.


The containers have npm, sfdx, and git installed

## Reasoning
When developing multiple projects, it's easy to mistakenly make a commit to the wrong org, because SFDX may preserve the configuration of another project.

Containerizing the development environment solves this issue, and makes the files from different projects invisible to each other while executing inside the container.

There is a [Salesforce official image](https://hub.docker.com/r/salesforce/salesforcedx)  for VSCode development, but at least in my computer it can't open the browser for authentication or browsing the org (with force:org:open), and that's why I decided to build my own.


## Usage

Follow the steps [here](https://code.visualstudio.com/docs/remote/containers)

Create a folder with a meaningful name for your project and place the .devcontainer folder there, then open the project folder with VSCode, wait for the prompt to suggest to reopen in container, and click the corresponding button. Check the extensions tab to install all the extensions you want in the container, and after they finish installing you may authorize an org to start development.
