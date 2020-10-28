# The Thycotic DevOps Secrets Vault Azure DevOps Task
This repository contains the code for an Azure DevOps pipeline task which is used to read secrets from Thycotic DevOps Secrets Vault.

## Prerequisites
* [Visual Studio Code](https://code.visualstudio.com/)
* [Node.js](https://nodejs.org)
* [TypeScript Compiler](https://www.npmjs.com/package/typescript)
* CLI for Azure DevOps (tfx-cli) to package the extension. You can install *tfx-cli* by running *npm i -g tfx-cli*.

## General
The task code can be found in the **DSVV1** directory. The entry point for the task is *index.ts* and most of the core code can be found in *operations/Vault.ts*.

## Compiling
From the task directory **DSVV1**, first install the task dependencies:
```
DSVV1> npm install
```

Then to compile the task:
```
DSVV1> tsc
```

## Debugging
Create a *launch.json* in your **.vscode** directory:
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "skipFiles": [
                "<node_internals>/**"
            ],
            "program": "${workspaceFolder}\\DSVV1\\index.ts",
            "outFiles": [
                "${workspaceFolder}/**/*.js"
            ],
            "env": {
                "INPUT_CLIENTID": "93d866d4-635f-4d4e-9ce3-0ef7f879f319",
                "INPUT_CLIENTSECRET": "xxxxxxxxxxxxxxxxxxxxxxxxx-xxxxxxxxxxx-xxxxx",
                "INPUT_SERVERURL": "https://mytenent.secretsvaultcloud.com/v1/",
                "INPUT_SECRETPATH": "/valid/secret",
                "INPUT_DATAFILTER": "*",
                "INPUT_VARIABLEPREFIX": "DSV_"
            }
        }
    ]
}
```
From the 'Run' menu, select 'Start Debugging' OR F5.

## Unit Tests
Create a *success_config.json* in the **DSVV1\tests** directory:
```json
{
    "credentials": {
        "clientId": "93d866d4-635f-4d4e-9ce3-0ef7f879f319",
        "clientSecret": "xxxxxxxxxxxxxxxxxxxxxxxxx-xxxxxxxxxxx-xxxxx"
    },
    "serverUrl": "https://mytenant.secretsvaultcloud.com/v1/",
    "secretPath": "/valid/secret",
    "dataFilter": "*",
    "variablePrefix": "DSV_"
}
```
Create a *failure_config.json* in the **DSVV1\tests** directory:
```json
{
    "credentials": {
        "clientId": "93d866d4-635f-4d4e-9ce3-0ef7f879f319",
        "clientSecret": "xxxxxxxxxxxxxxxxxxxxxxxxx-xxxxxxxxxxx-xxxxx"
    },
    "serverUrl": "https://mytenant.secretsvaultcloud.com/v1/",
    "secretPath": "/invalid/secret",
    "dataFilter": "*",
    "variablePrefix": "DSV_"
}
```
From the task directory **DSVV1**, run the following:
```
DSVV1> mocha .\tests\_suite.js
```

# Packaging the extension
Package the extension into a .vsix file using the following command from the repository root:
```
> tfx extension create --manifest-globs vss-extension.json
```
Note, the version in *vss-extension.json* must match the one in *DSVV1/task.json*.
