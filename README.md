# Saleforce App

There are two types of developer processes or models supported in Visual Studio Code. These models are explained below. Each model offers pros and cons and is fully supported.

### Org Development Model

The first supported model is the model most developers are already familiar with today - called Org Development Model. This model allows you to connect directly to you Sandbox, Developer Edition, Trailhead, or even Production orgs and retrieve and deploy code directly. This is the type of development you have done in the past using tools such as the Force.com IDE or MavensMate.

In order to start developing with this model, see the guide [Develop Against Any Org in Visual Studio Code](https://github.com/forcedotcom/salesforcedx-vscode/wiki/Develop-Against-Any-Org-in-Visual-Studio-Code).

If you are developing against sandboxes, developer edition, or trailhead orgs you should have used the command `SFDX: Create Project with Manifest` to create this project. If you used another command you may want to start over with that command.

When working with source against non-scratch orgs you will use the commands `SFDX: Deploy Source to Org` and `SFDX: Retrieve Source from Org`. The `Push` and `Pull` commands will only work on scratch orgs.

### Package Development Model

The second supported model is called Package Development Model. This model allows you to create self-contained applications or libraries that are deployed to your org as a single package. You can also use this model with the new type of org called Scratch Orgs. This model of development is geared toward a more modern type of software development process that uses org source tracking, source control, and continuous integration/deployment.

If you are starting a new project we would recommend you consider the Package Development Model.

If you are developing against scratch orgs you should have used the command `SFDX: Create Project` to create this project. If you used another command you may want to start over with that command.

When working with source against scratch orgs you will use the commands `SFDX: Push Source to Org` and `SFDX: Pull Source from Org`. Do not use the `Retrieve` and `Deploy` version of the commands on scratch orgs.

## Getting Started

### Understanding the `sfdx-project.json` File

The `sfdx-project.json` file contains useful configuration information for your project. See [the documentation](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_ws_config.htm) for the full details about this file.

The most important aspects of this file for getting start are the `packageDirectories` and `sfdcLoginUrl` settings.

The `sfdcLoginUrl` is used to specify which login URL to use when authorizing an org. This is explained more in the next section.

The `packageDirectories` file tells VS Code and the Salesforce CLI where metadata files are stored. You will need at least one package directory set in your file. The default setting is shown below. This means that by default your metadata will go in the `src` folder. If you want to change that folder to something else like `lib` simply change the config and make sure the folder exists.

```json
"packageDirectories" : [
    {
      "path": "src",
      "default": true
    }
]
```

### Authorize an Org

Now that you have your project setup you need to authorize an org to work against. Depending on if you are working with. You can authorize any type of org with the Visual Studio Code extensions.

To start the authorization process, run the command `SFDX: Authorize an Org`.

![Authorize an Org](https://github.com/forcedotcom/salesforcedx-vscode/wiki/images/authorize-org-command.png)

You will be shown the Salesforce login page. Enter your credentials and click allow to authorize VS Code.

### Working With Source Using a Package.xml File

> NOTICE: This section applies only when working against Sandbox, Developer Edition, or Trailhead orgs. Not Scratch Orgs.

If you are connected to a sandbox, developer edition, or trailhead org you will probably want to retrieve metadata from your org using a `package.xml` file. If you don't already have one, create a `package.xml` file in the `manifests` folder.

You will need to add the various metadata you want to retrieve to this file. You can read more about the `package.xml` file in [the documenation](https://developer.salesforce.com/docs/atlas.en-us.api_meta.meta/api_meta/manifest_samples.htm).

With your `package.xml` file created, you can right click on the file in the file tree or inside the contents of the file and select the command `SFDX: Retrieve Source from Org`. This will download your metadata from the org and place it in the location specified in the `sfdx-project.json` file as discussed above.

Once you retrieve your metadata your project structure will look something like the following.

```text
my-app
  ├── README.md
  ├── config
  │   └── project-scratch-def.json
  ├── force-app
  │   └── main
  │       └── default
  │           ├── applications
  │           ├── aura
  │           ├── classes
  │           ├── contentassets
  │           ├── flexipages
  │           ├── layouts
  │           ├── lwc
  │           ├── objects
  │           ├── permissionsets
  │           ├── staticresources
  │           ├── tabs
  │           └── triggers
  ├─── manifests
  │    └── package.xml
  └── sfdx-project.json
```

Similarly, you can deploy source by right-clicking on the `manifest.xml` file in the file tree or on its contents and selecting the command `SFDX: Deploy Source to Org`.

### Retrieving or Deploying Individual Files or Folders

> NOTICE: This section applies only when working against Sandbox, Developer Edition, or Trailhead orgs. Not Scratch Orgs.

In addition to retrieving or deploying metadata using `package.xml` files you can also deploy or retrieve individual folders or files. You can right click on either a folder or file and click `SFDX: Retrieve Source from Org` or `SFDX: Deploy Source to Org`.

The behaviour is such that the operation will occur on any metadata below the path you click on. For example, if you right click on the `classes` folder all apex classes that **currently exist** in that folder will be retrieved or deployed. This is important to note, running a retrieve operation on a folder like `classes` will **NOT** retrieve ALL apex classes on the org, it will only retrieve updates to classes that exist in the folder already. If you want to retrieve a new apex file you will need to add that file to a `package.xml` file and retrieve it that way.

![Deploy Source to Org](https://github.com/forcedotcom/salesforcedx-vscode/wiki/images/deploy-source-to-org.png)

### Working with Source Against Scratch Orgs

Scratch orgs support a feature called source tracking. This means that VS Code will only push and pull metadata to or from and or if there are changes. You don't need to run deploy or retrieve operations on individual files or folders or even use a `package.xml` file. Just run two commands `SFDX: Pull Source from Org` and `SFDX: Push Source to Org` and any changed metadata files will be pulled or pushed.

## Troubleshooting

Salesforce Extensions for VS Code functions properly only if the root directory of your open project contains an `sfdx-project.json` file.

If you’re not seeing the Apex completion suggestions that you expect, your Apex database might need to be rebuilt. Quit VS Code, and then delete the `.sfdx/tools/apex.db` file from your project. Then relaunch VS Code, and open an Apex class or trigger. The Apex database rebuilds within about 5 seconds (up to 30 seconds for very large code bases).

## Bugs and Feedback

To report issues with Salesforce Extensions for VS Code, open a [bug on GitHub](https://github.com/forcedotcom/salesforcedx-vscode/issues/new?template=Bug_report.md). If you would like to suggest a feature, create a [feature request on Github](https://github.com/forcedotcom/salesforcedx-vscode/issues/new?template=Feature_request.md).

## Resources

- [Getting Started Guide](https://github.com/forcedotcom/salesforcedx-vscode/wiki/Getting-Started-with-Visual-Studio-Code-for-Salesforce-Developers)
- [Develop Against Any Org](https://github.com/forcedotcom/salesforcedx-vscode/wiki/Develop-Against-Any-Org-in-Visual-Studio-Code)
- Trailhead: [Get Started with Salesforce DX](https://trailhead.salesforce.com/trails/sfdx_get_started)
- [Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev)
- [Apex Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode)
