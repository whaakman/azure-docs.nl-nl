---
title: Maken van een Azure-IoT-Hub met een sjabloon (.NET) | Microsoft Docs
description: Het gebruik van een Azure Resource Manager-sjabloon maken van een IoT Hub met C#-programma.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a447b40c-c728-487e-875d-db554db5adc3
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 3f84ab26c43f33827479faed46db1de246920ae5
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a>Een iothub met Azure Resource Manager-sjabloon (.NET) maken

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Azure Resource Manager kunt u maken en beheren van Azure IoT hubs via een programma. Deze zelfstudie ziet u het gebruik van een Azure Resource Manager-sjabloon voor een iothub maken vanuit een C#-programma.

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Azure Resource Manager en classic](../azure-resource-manager/resource-manager-deployment-model.md).  In dit artikel wordt behandeld met het Azure Resource Manager-implementatiemodel.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Een actief Azure-account. <br/>Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.
* Een [Azure Storage-account] [ lnk-storage-account] waarin u uw Azure Resource Manager sjabloonbestanden kunt opslaan.
* [Azure PowerShell 1.0] [ lnk-powershell-install] of hoger.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Visual Studio-project voorbereiden

1. In Visual Studio maakt een Visual C# Classic Windows Desktop-project met de **Console-App (.NET Framework)** projectsjabloon. Noem het project **CreateIoTHub**.

2. Klik in Solution Explorer met de rechtermuisknop op uw project en klik vervolgens op **NuGet-pakketten beheren**.

3. Controleer in NuGet Package Manager **Include prerelease**, en klik op de **Bladeren** pagina zoeken naar **Microsoft.Azure.Management.ResourceManager**. Selecteer het pakket, klikt u op **installeren**in **wijzigingen** klikt u op **OK**, klikt u vervolgens op **ik ga akkoord** te accepteren van de licenties.

4. Zoekt u in NuGet Package Manager **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klik op **installeren**in **wijzigingen** klikt u op **OK**, klikt u vervolgens op **ik ga akkoord** de licentievoorwaarden accepteren.

5. Vervang in Program.cs de bestaande **met** instructies met de volgende code:

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. Voeg de volgende statische variabelen Vervang de tijdelijke aanduiding voor waarden in Program.cs. U hebt een genoteerd **ApplicationId**, **SubscriptionId**, **TenantId**, en **wachtwoord** eerder in deze zelfstudie. **De naam van uw Azure Storage** is de naam van de Azure Storage-account waar u uw Azure Resource Manager-sjabloonbestanden opslaat. **De naam van resourcegroep** is de naam van de resourcegroep die u bij het maken van de IoT-hub. De naam mag een vooraf bestaande of nieuwe resourcegroep. **Implementatienaam** is een naam voor de implementatie, zoals **Deployment_01**.

    ```csharp
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-a-template-to-create-an-iot-hub"></a>Een sjabloon voor het maken van een IoT-hub te verzenden

Gebruik een JSON-sjabloon en de parameter-bestand te maken van een IoT-hub in de resourcegroep. U kunt ook een Azure Resource Manager-sjabloon gebruiken om een bestaande IoT-hub te wijzigen.

1. Klik in Solution Explorer met de rechtermuisknop op uw project, klikt u op **toevoegen**, en klik vervolgens op **Nieuw Item**. Toevoegen van een JSON-bestand aangeroepen **template.json** aan uw project.

2. Toevoegen van een standaard iothub en de **VS-Oost** regio, vervang de inhoud van **template.json** met de volgende resourcedefinitie. Zie voor de huidige lijst met regio's die ondersteuning bieden voor IoT Hub [Azure Status][lnk-status]:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

3. Klik in Solution Explorer met de rechtermuisknop op uw project, klikt u op **toevoegen**, en klik vervolgens op **Nieuw Item**. Toevoegen van een JSON-bestand aangeroepen **parameters.json** aan uw project.

4. Vervang de inhoud van **parameters.json** met de volgende parameterinformatie die een naam voor de nieuwe iothub, zoals ingesteld **{uw initialen} mynewiothub**. De naam van de IoT-hub moet wereldwijd uniek zodat deze uw naam of initialen bevatten:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

5. In **Server Explorer**maakt verbinding met uw Azure-abonnement en maak een zogenaamd in uw Azure Storage-account **sjablonen**. In de **eigenschappen** deelvenster, stelt u de **openbare leestoegang** machtigingen voor de **sjablonen** container **Blob**.

6. In **Server Explorer**, met de rechtermuisknop op de **sjablonen** container en klik vervolgens op **weergave Blob-Container**. Klik op de **Blob uploaden** knop, selecteert u de twee bestanden **parameters.json** en **templates.json**, en klik vervolgens op **Open** voor het uploaden van de JSON-bestanden toe aan de **sjablonen** container. De URL's van de blobs die met de JSON-gegevens zijn:

    ```csharp
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. Voeg de volgende methode toe aan Program.cs:

    ```csharp
    static void CreateIoTHub(ResourceManagementClient client)
    {

    }
    ```

8. Voeg de volgende code naar de **CreateIoTHub** methode voor het verzenden van de sjabloon en de parameter-bestanden in de Azure Resource Manager:

    ```csharp
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

9. Voeg de volgende code naar de **CreateIoTHub** methode die de status en de sleutels voor de nieuwe iothub worden weergegeven:

    ```csharp
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Voltooid en voer de toepassing

U kunt de toepassing nu voltooien door het aanroepen van de **CreateIoTHub** methode voordat u bouwen en uitvoeren.

1. Voeg de volgende code toe aan het einde van de **Main** methode:

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. Klik op **bouwen** en vervolgens **oplossing bouwen**. Eventuele fouten te corrigeren.

3. Klik op **Debug** en vervolgens **foutopsporing starten** de toepassing uit te voeren. Duurt enkele minuten voor de implementatie om uit te voeren.

4. Als u wilt controleren of uw toepassing de nieuwe iothub toegevoegd, gaat u naar de [Azure-portal] [ lnk-azure-portal] en uw lijst met resources weergeven. U kunt ook de **Get-AzureRmResource** PowerShell-cmdlet.

> [!NOTE]
> Deze voorbeeldtoepassing voegt een standaard IoT-Hub van S1, waarvoor u wordt gefactureerd. U kunt de IoT-hub via verwijderen de [Azure-portal] [ lnk-azure-portal] of met behulp van de **verwijderen AzureRmResource** PowerShell-cmdlet als u klaar bent.

## <a name="next-steps"></a>Volgende stappen
Nu u een iothub met een Azure Resource Manager-sjabloon met een C#-programma hebt geïmplementeerd, kunt u verder verkennen:

* Meer informatie over de mogelijkheden van de [resourceprovider IoT Hub REST-API][lnk-rest-api].
* Lees [overzicht van Azure Resource Manager] [ lnk-azure-rm-overview] voor meer informatie over de mogelijkheden van Azure Resource Manager.

Zie de volgende artikelen voor meer informatie over het ontwikkelen voor IoT-Hub:

* [Inleiding tot C-SDK][lnk-c-sdk]
* [Azure IoT SDK 's][lnk-sdks]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op de edge-apparaten met Azure IoT rand][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-storage-account]:../storage/common/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
