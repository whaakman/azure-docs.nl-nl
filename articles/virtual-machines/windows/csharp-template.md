---
title: Een virtuele machine met C# en Resource Manager-sjabloon implementeren | Microsoft Docs
description: Informatie over het gebruik van C# en Resource Manager-sjabloon voor het implementeren van een virtuele machine in Azure.
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: bfba66e8-c923-4df2-900a-0c2643b81240
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: davidmu
ms.openlocfilehash: bd1c860db026f948202cd1f3aa763b4547c597b4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Een Azure-virtuele Machine met C# en Resource Manager-sjabloon implementeren
In dit artikel laat zien hoe een Azure Resource Manager-sjabloon met C# implementeren. De sjabloon die u maakt implementeert een enkele virtuele machine met Windows Server in een nieuw virtueel netwerk met één subnet.

Zie voor een gedetailleerde beschrijving van de bron van de virtuele machine, [virtuele machines in een Azure Resource Manager-sjabloon](template-description.md). Zie voor meer informatie over de resources in een sjabloon [overzicht van Azure Resource Manager-sjabloon](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Het duurt ongeveer 10 minuten aan deze stappen uit te voeren.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

In deze stap maakt ervoor u zorgen dat Visual Studio is geïnstalleerd en u een consoletoepassing die wordt gebruikt maakt voor het implementeren van de sjabloon.

1. Als u nog niet gedaan hebt, installeert u [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Selecteer **.NET-ontwikkeling voor bureaubladen** op de pagina werkbelastingen en klik vervolgens op **installeren**. In de samenvatting, kunt u zien dat **.NET Framework 4 4.6 ontwikkelingsprogramma's** automatisch voor u is geselecteerd. Als u Visual Studio al hebt geïnstalleerd, kunt u de .NET-werkbelasting met de Visual Studio starten toevoegen.
2. Klik in Visual Studio **bestand** > **nieuw** > **Project**.
3. In **sjablonen** > **Visual C#**, selecteer **Console-App (.NET Framework)**, voer *myDotnetProject* voor de naam van het project, selecteer de locatie van het project en klik vervolgens op **OK**.

## <a name="install-the-packages"></a>De pakketten installeren

NuGet-pakketten zijn de eenvoudigste manier om de bibliotheken die u moet deze stappen hebt geïnstalleerd. Als u de bibliotheken die u nodig hebt in Visual Studio, voer deze stappen uit:

1. Klik op **extra** > **Nuget Package Manager**, en klik vervolgens op **Package Manager Console**.
2. Typ deze opdrachten in de console:

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    Install-Package WindowsAzure.Storage
    ```

## <a name="create-the-files"></a>De bestanden maken

In deze stap maakt u een sjabloonbestand dat de bronnen implementeert en een parameterbestand dat parameterwaarden voor de sjabloon. U wordt ook een bestand met autorisatieregels die wordt gebruikt voor het Azure Resource Manager-bewerkingen uitvoeren.

### <a name="create-the-template-file"></a>Het sjabloonbestand maken

1. Klik in Solution Explorer met de rechtermuisknop op *myDotnetProject* > **toevoegen** > **Nieuw Item**, en selecteer vervolgens **tekstbestand** in *Visual C# Items*. Noem het bestand *CreateVMTemplate.json*, en klik vervolgens op **toevoegen**.
2. Voeg deze JSON-code naar het bestand dat u hebt gemaakt:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" }
      },
      "variables": {
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks','myVNet')]", 
        "subnetRef": "[concat(variables('vnetID'),'/subnets/mySubnet')]", 
      },
      "resources": [
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "myPublicIPAddress",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "myresourcegroupdns1"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "myVNet",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "mySubnet",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "myNic",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/publicIPAddresses/', 'myPublicIPAddress')]",
            "[resourceId('Microsoft.Network/virtualNetworks/', 'myVNet')]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": { "id": "[resourceId('Microsoft.Network/publicIPAddresses','myPublicIPAddress')]" },
                  "subnet": { "id": "[variables('subnetRef')]" }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-04-30-preview",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "myVM",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkInterfaces/', 'myNic')]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_DS1" },
            "osProfile": {
              "computerName": "myVM",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "myManagedOSDisk",
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces','myNic')]"
                }
              ]
            }
          }
        }
      ]
    }
    ```

3. Sla het bestand CreateVMTemplate.json.

### <a name="create-the-parameters-file"></a>De parameterbestand maken

Waarden voor de resourceparameters die zijn gedefinieerd in de sjabloon wilt opgeven, moet u een parameterbestand met de waarden maken.

1. Klik in Solution Explorer met de rechtermuisknop op *myDotnetProject* > **toevoegen** > **Nieuw Item**, en selecteer vervolgens **tekstbestand** in *Visual C# Items*. Noem het bestand *Parameters.json*, en klik vervolgens op **toevoegen**.
2. Voeg deze JSON-code naar het bestand dat u hebt gemaakt:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUserName": { "value": "azureuser" },
        "adminPassword": { "value": "Azure12345678" }
      }
    }
    ```

4. Sla het bestand Parameters.json.

### <a name="create-the-authorization-file"></a>Maken van het bestand met autorisatieregels

Voordat u een sjabloon implementeren kunt, zorg ervoor dat u toegang tot hebben een [Active Directory-service-principal](../../resource-group-authenticate-service-principal.md). Van de service-principal moet u een token voor het verifiëren van aanvragen in Azure Resource Manager verkrijgen. U moet ook de toepassings-ID, de verificatiesleutel en de tenant-ID die u nodig hebt in het bestand met autorisatieregels vastleggen.

1. Klik in Solution Explorer met de rechtermuisknop op *myDotnetProject* > **toevoegen** > **Nieuw Item**, en selecteer vervolgens **tekstbestand** in *Visual C# Items*. Noem het bestand *azureauth.properties*, en klik vervolgens op **toevoegen**.
2. Deze eigenschappen van autorisatie toevoegen:

    ```
    subscription=<subscription-id>
    client=<application-id>
    key=<authentication-key>
    tenant=<tenant-id>
    managementURI=https://management.core.windows.net/
    baseURL=https://management.azure.com/
    authURL=https://login.windows.net/
    graphURL=https://graph.windows.net/
    ```

    Vervang  **&lt;abonnement-id&gt;**  met uw abonnements-id  **&lt;toepassing-id&gt;**  met de toepassings-id van het Active Directory  **&lt;verificatiesleutel&gt;**  voor de Toepassingssleutel, en  **&lt;tenant-id&gt;**  met tenant-id.

3. Sla het bestand azureauth.properties.
4. Stel een omgevingsvariabele in Windows met de naam AZURE_AUTH_LOCATION met het volledige pad naar het bestand met autorisatieregels die u hebt gemaakt, bijvoorbeeld de volgende PowerShell-opdracht kan worden gebruikt:

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```
    
## <a name="create-the-management-client"></a>De management-client maken

1. Open het bestand Program.cs voor het project dat u hebt gemaakt en voeg vervolgens deze using-instructies aan de bestaande instructies aan de bovenkant van het bestand:

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

2. Voeg deze code aan de Main-methode voor het maken van de management-client:

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Waarden voor de toepassing wilt opgeven, moet u code toevoegen aan de Main-methode:

```
var groupName = "myResourceGroup";
var location = Region.USWest;

var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

## <a name="create-a-storage-account"></a>Een opslagaccount maken

De sjabloon en de parameters worden van een opslagaccount in Azure geïmplementeerd. In deze stap maakt u het account maken en de bestanden uploaden. 

Voeg deze code aan de Main-methode voor het maken van het account:

```
string storageAccountName = SdkContext.RandomResourceName("st", 10);

Console.WriteLine("Creating storage account...");
var storage = azure.StorageAccounts.Define(storageAccountName)
    .WithRegion(Region.USWest)
    .WithExistingResourceGroup(resourceGroup)
    .Create();

var storageKeys = storage.GetKeys();
string storageConnectionString = "DefaultEndpointsProtocol=https;"
    + "AccountName=" + storage.Name
    + ";AccountKey=" + storageKeys[0].Value
    + ";EndpointSuffix=core.windows.net";

var account = CloudStorageAccount.Parse(storageConnectionString);
var serviceClient = account.CreateCloudBlobClient();

Console.WriteLine("Creating container...");
var container = serviceClient.GetContainerReference("templates");
container.CreateIfNotExistsAsync().Wait();
var containerPermissions = new BlobContainerPermissions()
    { PublicAccess = BlobContainerPublicAccessType.Container };
container.SetPermissionsAsync(containerPermissions).Wait();

Console.WriteLine("Uploading template file...");
var templateblob = container.GetBlockBlobReference("CreateVMTemplate.json");
templateblob.UploadFromFile("..\\..\\CreateVMTemplate.json");

Console.WriteLine("Uploading parameters file...");
var paramblob = container.GetBlockBlobReference("Parameters.json");
paramblob.UploadFromFile("..\\..\\Parameters.json");
```

## <a name="deploy-the-template"></a>De sjabloon implementeren

Implementeer de sjabloon en de parameters van het opslagaccount dat is gemaakt. 

Voeg deze code aan de Main-methode voor het implementeren van de sjabloon:

```
var templatePath = "https://" + storageAccountName + ".blob.core.windows.net/templates/CreateVMTemplate.json";
var paramPath = "https://" + storageAccountName + ".blob.core.windows.net/templates/Parameters.json";
var deployment = azure.Deployments.Define("myDeployment")
    .WithExistingResourceGroup(groupName)
    .WithTemplateLink(templatePath, "1.0.0.0")
    .WithParametersLink(paramPath, "1.0.0.0")
    .WithMode(Microsoft.Azure.Management.ResourceManager.Fluent.Models.DeploymentMode.Incremental)
    .Create();
Console.WriteLine("Press enter to delete the resource group...");
Console.ReadLine();
```

## <a name="delete-the-resources"></a>De resources verwijderen

Omdat u in rekening voor resources die worden gebruikt in Azure gebracht, maar het is altijd raadzaam om resources verwijderen die niet langer nodig zijn. U hoeft niet elke resource afzonderlijk verwijderen uit een resourcegroep. Verwijder de resourcegroep en alle bijbehorende resources worden automatisch verwijderd. 

Voeg deze code aan de Main-methode voor het verwijderen van de resourcegroep:

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-the-application"></a>De toepassing uitvoeren

Het moet over vijf minuten duren voordat deze consoletoepassing volledig uitvoeren vanaf het begin te voltooien. 

1. Als u wilt de consoletoepassing uitvoeren, klikt u op **Start**.

2. Voordat u op **Enter** om te starten als u resources verwijdert, u kan even duren om te controleren of het maken van de resources in de Azure-portal. Klik op de implementatiestatus voor informatie over de implementatie.

## <a name="next-steps"></a>Volgende stappen
* Als er problemen met de implementatie zijn, een volgende stap zou zijn om te kijken naar [oplossen van veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager](../../resource-manager-common-deployment-errors.md).
* Informatie over het implementeren van een virtuele machine en de ondersteunende resources aan de hand van [implementeren van een Azure virtuele Machine met behulp van C#](csharp.md).
