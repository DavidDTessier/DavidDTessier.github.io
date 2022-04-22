---
layout: post
title:  "Infrastructure-As-Code (IaC) Tooling Options Comparison - Part 3"
draft: true
date:   2021-12-03 15:17:00
tags: [IaC, devops, cloud, terraform, pulumi, arm, cloudformation, aws, gcloud, azure, draft]
---

<h1>THIS IS IN DRAFT....</h1>
In [part 1](/2021/05/31/Infras-As-Code-P1.html) and [part 2](/_site/2021/05/31/Infras-As-Code-P2.html) we reviewed what infrastructure-as-code tools are as well as the following tools: [Terraform](https://www.terraform.io/), [Terraform CDK](https://github.com/hashicorp/terraform-cdk),[Azure Resource Manager (ARM)](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview), and [Azure Bicep](https://devblogs.microsoft.com/devops/project-bicep-next-generation-arm-templates/).

In this third volume we will continue our disection of IaC tools; this time reviewing will will be focusing on AWS's two native solutions:
* [Cloud Formation](https://aws.amazon.com/cloudformation/)
* [AWS CDK](https://aws.amazon.com/cdk/)

# AWS CloudFormation

## What is CloudFromation

_CloudFormation_ or typically referred to as _CF_ for short is AWS's native tooling for deploying and managing of services/infrastructure within its cloud environment.

It provides a centralized management layer that allows users to create, update, and delete resources in your Azure account using Azure tools, APIs, or SDKs.

For those unfamiliar with _Azure Resource Manager_ let's breakdown some of the key terms:

* _resource_
  * A manageable item that is available through Azure. Virtual machines, storage accounts, web apps, databases, and virtual networks are examples of resources. Resource groups, subscriptions, management groups, and tags are also examples of resources.
* _resource group_
  * A container that holds related resources for an Azure solution. The resource group includes those resources that you want to manage as a group. You decide which resources belong in a resource group based on what makes the most sense for your organization.
* _resource provider_
  * A service that supplies Azure resources. For example, a common resource provider is Microsoft.Compute, which supplies the virtual machine resource. Microsoft.Storage is another common resource provider.
* _Resource Manager template_
  * A JavaScript Object Notation (JSON) file that defines one or more resources to deploy to a resource group, subscription, management group, or tenant. The template can be used to deploy the resources consistently and repeatedly.
* _declarative syntax_
  * Syntax that lets you state "Here is what I intend to create" without having to write the sequence of programming commands to create it. The Resource Manager template is an example of declarative syntax. In the file, you define the properties for the infrastructure to deploy to Azure.


From an IaC perspective the typical approach to leverage _ARM_ is using _ARM Templates_ which are JavaScript Object Notation (JSON) based files that define the resources that make up your environment.

## What are the benefits of Azure Resource Manager

Azure Resource Manager Templates offers the same benefits as Terraform when it comes to infrastructure-as-code, i.e. resuablitity, consistent state of your environment, maintainability using templates over scripts.

Another benefit of using ARM for deploying infrastructure in Azure is that any new updates to the underlying APIs for provisioning resources is automatically available in _ARM_ do it is native capabilities.

Native state management without the need to instantiate any additional component to keep track of your environment is also a nice thing ;).

## ARM Templates

The following is a sample format of the JSON template that is used.

```
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "",
    "apiProfile": "",
    "parameters": {  },
    "variables": {  },
    "functions": [  ],
    "resources": [  ],
    "outputs": {  }
}

```

The file is split into six sections:

* _schema_:
  * This section defines the location of the JavaScript Object Notation (JSON) schema file that describes the version of the template language. The version number you use depends on the scope of the deployment and your JSON editor.
* _contentVersion":
  * This attribute defines the version of the template (such as 1.0.0.0). You can provide any value for this element. Use this value to document significant changes in your template. When deploying resources using the template, this value can be used to make sure that the right template is being used. **This attribute is required.**
* _apiProfile_:
  * This attribute is used to avoid having to specify API versions for each resource in the template. When you specify an API profile version and don't specify an API version for the resource type, Resource Manager uses the API version for that resource type that is defined in the profile. The API profile property is especially helpful when deploying a template to different environments, such as Azure Stack and global Azure. Use the API profile version to make sure your template automatically uses versions that are supported in both environments. For a list of the current API profile versions and the resources API versions defined in the profile. **This attribute is not required.**
* _parameters_:
  * This sections defines the values that are provided when deployment is executed to customize resource deployment.
* _variables_:
  * This section defines the values that are used as JSON fragments in the template to simplify template language expressions.
* _functions_:
  * This section defines user-defined functions that are available within the template.
* _resources_:
  * This section defines the resource types that are deployed or updated in a resource group or subscription.
* _outputs_:
  * This section defines the values that are returned after deployment.

Unlike Terraform, as described in the previous article,  which use remote state to manage the environment; Azure manages the deployments internally and no additional steps are required.

Now that we have defined what the template consists of, let's take a look at a sample template that will deploy a webserver in Azure.

To keep this concise, the full sample can be found [here](https://github.com/DavidDTessier/iac-tools-samples/tree/main/Azure_Native/Azure_Resource_Manager/azuredeploy.json), the follow will outline the VM specific resource

```
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "",
    "apiProfile": "",
    "parameters": { .... },
    "variables": { ... },
    "functions": [],
    "resources": [
        ...
        {
        "type": "Microsoft.Compute/virtualMachines",
        "apiVersion": "2021-03-01",
        "name": "[parameters('vmName')]",
        "location": "[parameters('location')]",
        "properties": {
          "hardwareProfile": {
            "vmSize": "[parameters('vmSize')]"
          },
          "osProfile": {
            "computerName": "[parameters('vmName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
          },
          "storageProfile": {
            "imageReference": {
              "publisher": "MicrosoftWindowsServer",
              "offer": "WindowsServer",
              "sku": "[parameters('OSVersion')]",
              "version": "latest"
            },
            "osDisk": {
              "createOption": "FromImage",
              "managedDisk": {
                "storageAccountType": "StandardSSD_LRS"
              }
            },
            "dataDisks": [
              {
                "diskSizeGB": 1023,
                "lun": 0,
                "createOption": "Empty"
              }
            ]
          },
          "networkProfile": {
            "networkInterfaces": [
              {
                "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('nicName'))]"
              }
            ]
          },
          "diagnosticsProfile": {
            "bootDiagnostics": {
              "enabled": true,
              "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))).primaryEndpoints.blob]"
            }
          }
        },
        "dependsOn": [
          "[resourceId('Microsoft.Network/networkInterfaces', variables('nicName'))]",
          "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
        ]
      },
      {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "apiVersion": "2021-04-01",
        "name": "[concat(parameters('vmName'),'/', 'InstallWebServer')]",
        "location": "[parameters('location')]",
        "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"
        ],
        "properties": {
          "publisher": "Microsoft.Compute",
          "type": "CustomScriptExtension",
          "typeHandlerVersion": "1.7",
          "autoUpgradeMinorVersion": true,
          "settings": {
            "fileUris": [
              "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-vm-extension/installWebServer.ps1"
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File installWebServer.ps1"
          }
        }
      }
    ],
    "outputs": {
        "hostname": {
            "type": "string",
            "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIpName'))).dnsSettings.fqdn]"
      }
    }
}
```

Once the template is fully defined you can leverage [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) or [Azure Powershell]
(https://docs.microsoft.com/en-us/powershell/azure/?view=azps-6.6.0) to deploy the template.

For our example we will use Azure CLI. First you must create a resource group that will contain the resources in the template:

`az group create --name mydemorg --location "Central US"`

Once the resource group is created, run the following command to deploy the template:

```
az deployment group create \
    --name $deploymentName \
    --resource-group my-demo-rg \
    --template-file _templatefilename_
```

Replace the _templatefilename_ with the template file.

Once the deployment has complete, the hostname of the webserver will appear in the output:

```
 "outputs": {
      "hostname": {
        "type": "String",
        "value": "simple-vm-bvmnkllaw5pva.centralus.cloudapp.azure.com"
      }
```

Navigating to the hostname in your browser it should show the following page:

![Azure Web Server](/images/azure-web-server.png)

If you navigate to the Azure Portal and go to the newly create resource group you will see that this process has generated a successful deployment as shown in the image below. This is the how Azure manages the difference between changes to the template and what is deployed in the environment.

![Azure RG Deployments](/images/azure-deployments.png)

## Potential downsides for the user

JSON notation can become unreadable depending on the complexity of your templates.

# Azure Bicep

## What is Azure Bicep?

Bicep is a domain-specific language, it is a replacement for developing Azure Resource Manager templates in JSON. Bicep reduces the complexity of developing in JSON and provides efficient scale deployments of Azure resources.

## What are the benefits of Azure Bicep

1. Simple syntax:
 Compared to writing in JSON, Bicep is concise and easy to read. It is easy to pick up and understand even from a user with little to no coding experience.

2. First party support:
 Bicep supports all preview and GA versions of Azure services right out of the box. As Azure resource providers push out new versions of their product, Bicep is able to use them in file right away.

3. No state or state files to manage:
 Using a what-if operation, users can manage and update their deployments. (More elaboration on "what-if" later in the article -How to run Bicep code) State files allow users to have more predictable  deployments, however they must be updated on every update to the code. A lack of state files allows for more fluidity albeit with more uncertainity in the success of the deployment.

4. Modular:
 "Modules" are files that can be built outside of main project files and then consumed in order to quickly and efficiently deploy resources across multiple projects.

5. Efficiency:
 When building resources, users are prompted with a "required properties" [When coding in VS code with the Azure Bicep extension]. This will autofill the required fields to launch the specified resources, this does not limit the user to the customization of said resource either.

6. Familiarity // ease of use:
 The structure and flow of Bicep can be similar to that of Terraform. Veteran users will have no issue transitioning beetween the two languages. First time users will also be quick to learn as the language is very conventional.


## Potential downsides for the user

1.  Bicep can be new line sensitive. This can be a hassle in terms of code organization and clarity.

2. No support for single line arrays or objects. [An inconvenience and might cause clutter but still functional]

3. Lacking documentation in comparison to terraform. This might not be a problem for those well versed in the various resources.

4. No managing state files, Bicep documentation sites this as an upside, however some users might not like this out of preference.

5. Deployment error messages. Unlike Terraform, when there is a deployment error in Bicep, the user must navigate through the Azure portal in order to find the source of error.

## Sample code

The full sample that I am highlighting here can be found on my [github repo](https://github.com/DavidDTessier/iac-tools-samples/blob/main/Azure_Native/Bicep/azuredeploy.bicep).

The snipet below indicates how to deploy a vm using _Bicep_:

```
resource vm 'Microsoft.Compute/virtualMachines@2021-07-01' = {
  name: vmName
  location: location
  properties: {
    hardwareProfile: {
      vmSize: vmSize
    }
    osProfile: {
      computerName: vmName
      adminPassword: adminPassword
      adminUsername: adminUser
    }
    storageProfile: {
     imageReference: {
       publisher: 'MicrosoftWindowsServer'
       offer: 'WindowsServer'
       sku: osVersion
       version: 'latest'
     }
     osDisk: {
      createOption: 'FromImage'
      managedDisk: {
        storageAccountType: 'StandardSSD_LRS'
      }
     }
     dataDisks: [
      {
        diskSizeGB: 1023
        lun: 0
        createOption: 'Empty'
      }
    ]
    }
    networkProfile: {
      networkInterfaces: [
        {
          id: nic.id
        }
      ]
    }
    diagnosticsProfile: {
      bootDiagnostics: {
        enabled: true
        storageUri: storageAccount.properties.primaryEndpoints.blob
      }
    }
  }

  dependsOn: [
    storageAccount
    nic
  ]
}
```

As you can see the structure is very similar to _Terraform_'s declarative syntax but leverages the same schema mechanism as its _ARM_ alternative.

## How to run Bicep code

Instead of the traditional terraform plan and state files, bicep uses a what-if operation.

~~~~
az deployment group what-if --mode Incremental --name rollout01 --resource-group testrg --template-file demotemplate.json
~~~~

This command format can be used in the azure CLI. An example of the output for this operation as provided by Microsoft is as follows:

~~~~
Resource and property changes are indicated with these symbols:
  - Delete
  + Create
  ~ Modify

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
    - tags.Owner: "Team A"
    ~ properties.addressSpace.addressPrefixes: [
      - 0: "10.0.0.0/16"
      + 0: "10.0.0.0/15"
      ]
    ~ properties.subnets: [
      - 0:

          name:                     "subnet001"
          properties.addressPrefix: "10.0.0.0/24"

      ]

Resource changes: 1 to modify.
~~~~

This format of deployment preview Bicep unlike Terraform, does not save this preview/plan to a state file. Similar to running terraform plan without using "-out" to save the plan and then deploying your code.

There are multiple ways to launch resources in Bicep. Since the project presented is heavily parameterized it must be launched in the following format:

az deployment group create \
--resource-group ResG \
--template-file <path-to-bicep> \
--parameters location="eastus2" dataFactoryName="exampledf" ... etc \

These parameters can be filled within the respective modules/main file; if a parameter is used across multiple modules or separate workspaces/deployments are needed, they can be passed throught the azure cli as shown.

# Terraform Vs Bicep

Is there a reason to use Bicep over Terraform? From a beginners persepective, Bicep has a larger learning curve and was made much easier due to having learned Terraform first. The skills that are learned from whatever domain you first learn in Terraform are transferrable to other cloud domains. Whereas generally speaking, due to the domain specific nature of Bicep, the skills are not necessarily transferrable.

In terms of which language is more useful for the long term. Investing time in to Terraform seems to be the better option. Keep in mind that after learning the basics of Terraform and the fundamentals of cloud, you essentially have learned Bicep (at the very least it becomes easier to pick up).

Bicep is a powerful language that enables the user to launch Azure resources quickly and efficiently. However, it is hard to say that the time investment for Bicep is more valuable than putting time in to Terrafom. It just is not the powerful multi domain tool that Terraform is.


# Azure Bicep vs Azure ARM Templates

In final thoughts when we compare ARM templates JSON notation to Bicep's declarative (Terraform like) syntax I would lean more towards using Bicep. The reason behind this is that Microsoft is investing time to make Bicep more user friendly and the backing tooling like [VS Code Bicep Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-bicep) make build bicep code so much easier that understanding the JSON schema to build ARM templates.

When it comes to _Bicep_ over _Terraform_ it is still an interesting debate as to which is best suited. If your organization is only interested in Azure and are not looking at other cloud providers than the clear winner is Bicep; however if your looking for a muti-cloud approach than I would go with Terraform as you can build you multi-cloud environment with a single code base.
