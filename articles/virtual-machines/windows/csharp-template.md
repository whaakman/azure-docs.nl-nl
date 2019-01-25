---
title: Een virtuele machine met behulp van C# en Resource Manager-sjabloon implementeren | Microsoft Docs
description: Meer informatie over het gebruik van C# en Resource Manager-sjabloon om een Azure-VM te implementeren.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: bfba66e8-c923-4df2-900a-0c2643b81240
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: cynthn
ms.openlocfilehash: 005b0e74084325606a9a07df6b36b9100cad1750
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54885945"
---
# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Een Azure-Machine met behulp van C# en Resource Manager-sjabloon implementeren
Dit artikel ziet u hoe u een Azure Resource Manager-sjabloon met C# implementeren. De sjabloon die u maakt implementeert een enkele virtuele machine met Windows Server in een nieuw virtueel netwerk met één subnet.

Zie voor een gedetailleerde beschrijving van de bron van de virtuele machine, [virtuele machines in een Azure Resource Manager-sjabloon](template-description.md). Zie voor meer informatie over alle resources in een sjabloon, [overzicht Azure Resource Manager-sjabloon](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Het duurt ongeveer 10 minuten deze stappen uitvoeren.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

In deze stap maakt ervoor u zorgen dat Visual Studio is geïnstalleerd en maakt u een consoletoepassing die wordt gebruikt om de sjabloon te implementeren.

1. Als u niet hebt gedaan, installeert u [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Selecteer **.NET-desktopontwikkeling** op de pagina van de werkbelasting, en klik vervolgens op **installeren**. In de samenvatting, kunt u zien dat **.NET Framework 4-4.6 ontwikkelprogramma's** automatisch voor u is geselecteerd. Als u Visual Studio al hebt geïnstalleerd, kunt u de .NET-werkbelasting via Visual Studio starten toevoegen.
2. Klik in Visual Studio op **File** > **New** > **Project**.
3. In **sjablonen** > **Visual C#**, selecteer **Console-App (.NET Framework)**, voer *myDotnetProject* voor de naam van de Project, selecteer de locatie van het project en klik vervolgens op **OK**.

## <a name="install-the-packages"></a>De pakketten installeren

NuGet-pakketten zijn de eenvoudigste manier voor het installeren van de bibliotheken die u moet deze stappen voltooien. Als u de bibliotheken die u nodig hebt in Visual Studio, voert u deze stappen uit:

1. Klik op **extra** > **Nuget Package Manager**, en klik vervolgens op **Package Manager Console**.
2. Typ deze opdrachten in de console:

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    Install-Package WindowsAzure.Storage
    ```

## <a name="create-the-files"></a>De bestanden maken

In deze stap maakt u een sjabloon voor bestanden die worden geïmplementeerd en een parameterbestand waarmee de parameterwaarden voor de sjabloon. U maakt ook een bestand met autorisatieregels die wordt gebruikt voor het Azure Resource Manager-bewerkingen uitvoeren.

### <a name="create-the-template-file"></a>Het sjabloonbestand maken

1. Klik in Solution Explorer met de rechtermuisknop op *myDotnetProject* > **toevoegen** > **Nieuw Item**, en selecteer vervolgens **tekstbestand** in *Visual C#-Items*. Noem het bestand *CreateVMTemplate.json*, en klik vervolgens op **toevoegen**.
2. Deze JSON-code toevoegen aan het bestand dat u hebt gemaakt:

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

### <a name="create-the-parameters-file"></a>Het parameterbestand maken

Waarden voor de resourceparameters die zijn gedefinieerd in de sjabloon wilt opgeven, maakt u een parameterbestand die de waarden bevat.

1. Klik in Solution Explorer met de rechtermuisknop op *myDotnetProject* > **toevoegen** > **Nieuw Item**, en selecteer vervolgens **tekstbestand** in *Visual C#-Items*. Noem het bestand *Parameters.json*, en klik vervolgens op **toevoegen**.
2. Deze JSON-code toevoegen aan het bestand dat u hebt gemaakt:

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

### <a name="create-the-authorization-file"></a>Het bestand met autorisatieregels maken

Voordat u een sjabloon implementeren kunt, moet u ervoor dat u toegang tot hebt een [Active Directory service-principal](../../active-directory/develop/howto-authenticate-service-principal-powershell.md). U ophalen een token voor het verifiëren van aanvragen naar Azure Resource Manager uit de service-principal. U moet ook de toepassings-ID, de verificatiesleutel en de tenant-ID die u nodig hebt in het bestand met autorisatieregels vastleggen.

1. Klik in Solution Explorer met de rechtermuisknop op *myDotnetProject* > **toevoegen** > **Nieuw Item**, en selecteer vervolgens **tekstbestand** in *Visual C#-Items*. Noem het bestand *azureauth.properties*, en klik vervolgens op **toevoegen**.
2. Voeg deze eigenschappen autorisatie:

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

    Vervang **&lt;abonnement-id&gt;** met uw abonnements-id, **&lt;toepassing-id&gt;** met de Active Directory-toepassing id **&lt;verificatiesleutel&gt;** met de sleutel van de toepassing en **&lt;tenant-id&gt;** met de tenant-id.

3. Sla het bestand azureauth.properties.
4. Stel een omgevingsvariabele in Windows met de naam AZURE_AUTH_LOCATION met het volledige pad naar bestand met autorisatieregels die u hebt gemaakt, bijvoorbeeld de volgende PowerShell-opdracht kan worden gebruikt:

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```
    
## <a name="create-the-management-client"></a>De management-client maken

1. Open het bestand Program.cs van het project dat u hebt gemaakt en voeg deze using-instructies toe aan de bestaande instructies aan de bovenkant van het bestand:

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

2. Voor het maken van de management-client, voeg deze code toe aan de methode Main om:

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

Waarden voor de toepassing wilt opgeven, voeg code toe aan de methode Main om:

```
var groupName = "myResourceGroup";
var location = Region.USWest;

var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

## <a name="create-a-storage-account"></a>Create a storage account

De sjabloon en parameters worden van een opslagaccount in Azure geïmplementeerd. In deze stap maakt u het account maken en de bestanden uploaden. 

Voor het maken van het account, voeg deze code toe aan de methode Main om:

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
templateblob.UploadFromFileAsync("..\\..\\CreateVMTemplate.json").Result();

Console.WriteLine("Uploading parameters file...");
var paramblob = container.GetBlockBlobReference("Parameters.json");
paramblob.UploadFromFileAsync("..\\..\\Parameters.json").Result();
```

## <a name="deploy-the-template"></a>De sjabloon implementeren

Implementeer de sjabloon en parameters van het opslagaccount dat is gemaakt. 

Als u wilt implementeren de sjabloon, voeg deze code toe aan de methode Main om:

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

Omdat u voor resources die worden gebruikt in Azure betaalt, is het altijd verstandig resources verwijderen die niet langer nodig zijn. U hoeft niet te verwijderen van elke resource afzonderlijk van een resourcegroep. Verwijder de resourcegroep en alle bijbehorende resources automatisch worden verwijderd. 

Als u wilt verwijderen van de resourcegroep, voeg deze code toe aan de methode Main om:

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-the-application"></a>De toepassing uitvoeren

Het duurt ongeveer vijf minuten voor deze consoletoepassing volledig uitvoeren vanaf het begin te voltooien. 

1. Als u wilt de consoletoepassing uitvoeren, klikt u op **Start**.

2. Voordat u druk op **Enter** om te beginnen met verwijderen van resources, kunt u een paar minuten om te controleren of het maken van de resources in Azure portal kunt nemen. Klik op de status van de implementatie als u wilt weergeven over de implementatie.

## <a name="next-steps"></a>Volgende stappen
* Als er problemen met de implementatie zijn, een volgende stap zou zijn om te kijken naar [veelvoorkomende problemen oplossen Azure-implementatie met Azure Resource Manager](../../resource-manager-common-deployment-errors.md).
* Informatie over het implementeren van een virtuele machine en de ondersteunende resources aan de hand [implementeren van een Azure virtuele Machine met behulp van C#](csharp.md).
