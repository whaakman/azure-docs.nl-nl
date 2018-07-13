---
title: Maken van een Azure-IoT-Hub met behulp van een sjabloon (.NET) | Microsoft Docs
description: Het gebruik van een Azure Resource Manager-sjabloon een IoT-Hub maken met een C#-programma.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 1a64749b7218fccfdad6b6eeebfac39a44aa0522
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38687768"
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a>Een IoT-hub maken met Azure Resource Manager-sjabloon (.NET)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Azure Resource Manager kunt u maken en beheren van Azure IoT-hubs via een programma. Deze zelfstudie leert u hoe u een Azure Resource Manager-sjabloon gebruiken om te maken van een IoT-hub uit een C#-programma.

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Azure Resource Manager en klassiek](../azure-resource-manager/resource-manager-deployment-model.md).  In dit artikel bevat informatie over het Azure Resource Manager-implementatiemodel.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Visual Studio 2015 of Visual Studio 2017.
* Een actief Azure-account. <br/>Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.
* Een [Azure Storage-account] [ lnk-storage-account] waar u uw Azure Resource Manager sjabloonbestanden kan opslaan.
* [Azure PowerShell 1.0] [ lnk-powershell-install] of hoger.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Visual Studio-project voorbereiden

1. Maak in Visual Studio een Visual C# Windows Classic Desktop-project met de **Console-App (.NET Framework)** projectsjabloon. Noem het project **CreateIoTHub**.

2. Klik in Solution Explorer met de rechtermuisknop op uw project en klik vervolgens op **NuGet-pakketten beheren**.

3. Controleer in NuGet Package Manager **Include prerelease**, en klik op de **Bladeren** zoekopdracht met resultatenpagina voor **Microsoft.Azure.Management.ResourceManager**. Selecteer het pakket, klikt u op **installeren**in **wijzigingen controleren** klikt u op **OK**, klikt u vervolgens op **ik ga akkoord** te accepteren van de licenties.

4. Zoeken in NuGet Package Manager **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klik op **installeren**in **wijzigingen controleren** klikt u op **OK**, klikt u vervolgens op **ik ga akkoord** om de licentie te accepteren.

5. Vervang in Program.cs de bestaande **met behulp van** instructies met de volgende code:

    ```csharp
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

6. Toevoegen in Program.cs de volgende statische variabelen, vervang de tijdelijke aanduiding voor waarden. U hebt genoteerd een **ApplicationId**, **SubscriptionId**, **TenantId**, en **wachtwoord** eerder in deze zelfstudie. **De naam van uw Azure Storage-account** is de naam van de Azure Storage-account waar u uw Azure Resource Manager-sjabloonbestanden opslaat. **De naam van resourcegroep** is de naam van de resourcegroep die u gebruikt bij het maken van de IoT-hub. De naam mag bestaan uit een bestaande of nieuwe resourcegroep. **Naam van de implementatie** is een naam voor de implementatie, zoals **Deployment_01**.

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

## <a name="submit-a-template-to-create-an-iot-hub"></a>Indienen van een sjabloon voor het maken van een IoT-hub

Een JSON-sjabloon en de parameter-bestand gebruiken om te maken van een IoT-hub in de resourcegroep. U kunt ook een Azure Resource Manager-sjabloon gebruiken om een bestaande IoT-hub te wijzigen.

1. Klik in Solution Explorer met de rechtermuisknop op uw project, klikt u op **toevoegen**, en klik vervolgens op **Nieuw Item**. Toevoegen van een JSON-bestand met de naam **template.json** aan uw project.

2. Toevoegen van een standard IoT-hub kunt u de **VS-Oost** regio, vervang de inhoud van **template.json** met de volgende resourcedefinitie. Zie voor de huidige lijst met regio's die ondersteuning bieden voor IoT Hub [Azure Status][lnk-status]:

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

3. Klik in Solution Explorer met de rechtermuisknop op uw project, klikt u op **toevoegen**, en klik vervolgens op **Nieuw Item**. Toevoegen van een JSON-bestand met de naam **parameters.json** aan uw project.

4. Vervang de inhoud van **parameters.json** met de volgende parameterinformatie die Hiermee stelt u een naam voor de nieuwe IoT hub, zoals **{uw initialen} mynewiothub**. De naam van de IoT-hub moet globaal uniek zijn, zodat hierbij moet uw naam of initialen:

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

5. In **Server Explorer**maakt verbinding met uw Azure-abonnement en maak een container met de naam in uw Azure Storage-account **sjablonen**. In de **eigenschappen** deelvenster, stelt u de **openbare leestoegang** machtigingen voor de **sjablonen** container **Blob**.

6. In **Server Explorer**, met de rechtermuisknop op de **sjablonen** container en klik vervolgens op **weergave Blob-Container**. Klik op de **Blob uploaden** knop, selecteert u de twee bestanden **parameters.json** en **templates.json**, en klik vervolgens op **Open** het uploaden van de JSON-bestanden naar de **sjablonen** container. De URL's van de blobs met de JSON-gegevens zijn:

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

8. Voeg de volgende code aan de **CreateIoTHub** methode voor het verzenden van de sjabloon en de parameterbestanden bestanden naar de Azure Resource Manager:

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

9. Voeg de volgende code aan de **CreateIoTHub** methode waarmee de status en de sleutels voor de nieuwe IoT hub worden weergegeven:

    ```csharp
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Voltooien en de toepassing wordt uitgevoerd

U kunt de toepassing nu uitvoeren door het aanroepen van de **CreateIoTHub** methode voordat u bouwen en uitvoeren.

1. Voeg de volgende code toe aan het einde van de **Main** methode:

    ```csharp
    CreateIoTHub(client);
    Console.ReadLine();
    ```

2. Klik op **bouwen** en vervolgens **-oplossing bouwen**. Eventuele fouten te corrigeren.

3. Klik op **Debug** en vervolgens **Start Debugging** de toepassing uit te voeren. Het duurt enkele minuten voor de implementatie om uit te voeren.

4. Als u wilt controleren of uw toepassing toegevoegd de nieuwe IoT hub, gaat u naar de [Azure-portal] [ lnk-azure-portal] en uw lijst met resources weer te geven. U kunt ook de **Get-AzureRmResource** PowerShell-cmdlet.

> [!NOTE]
> In dit voorbeeld van de toepassing wordt toegevoegd een Standard IoT-Hub van S1, die in rekening worden gebracht. U kunt de IoT-hub via verwijderen de [Azure-portal] [ lnk-azure-portal] of met behulp van de **Remove-AzureRmResource** PowerShell-cmdlet als u klaar bent.

## <a name="next-steps"></a>Volgende stappen
Nu u een IoT-hub met behulp van een Azure Resource Manager-sjabloon met een C#-programma hebt geïmplementeerd, kunt u verder te verkennen:

* Meer informatie over de mogelijkheden van de [REST API voor IoT Hub-resourceprovider][lnk-rest-api].
* Lezen [overzicht van Azure Resource Manager] [ lnk-azure-rm-overview] voor meer informatie over de mogelijkheden van Azure Resource Manager.

Zie de volgende artikelen voor meer informatie over het ontwikkelen van IoT-Hub:

* [Inleiding tot C SDK][lnk-c-sdk]
* [Azure IoT SDK 's][lnk-sdks]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]

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
