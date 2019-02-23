---
title: Maken van een Azure-IoT-Hub met behulp van een sjabloon (PowerShell) | Microsoft Docs
description: Het gebruik van een Azure Resource Manager-sjabloon maken van een IoT-Hub met PowerShell.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 474360bfa874298b35b1e7c754545ff576013d50
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56734741"
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>Een IoT-hub maken met Azure Resource Manager-sjabloon (PowerShell)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Azure Resource Manager kunt u maken en beheren van Azure IoT-hubs via een programma. Deze zelfstudie leert u hoe u een Azure Resource Manager-sjabloon een IoT-hub maken met PowerShell.

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Azure Resource Manager en klassieke](../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over het Azure Resource Manager-implementatiemodel.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-account. <br/>Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.
* [Azure PowerShell 1.0] [ lnk-powershell-install] of hoger.

> [!TIP]
> Het artikel [met behulp van Azure PowerShell met Azure Resource Manager] [ lnk-powershell-arm] vindt u meer informatie over het gebruik van PowerShell en Azure Resource Manager-sjablonen te maken van Azure-resources.

## <a name="connect-to-your-azure-subscription"></a>Verbinding maken met uw Azure-abonnement

Voer de volgende opdracht uit om aan te melden bij uw Azure-abonnement in een PowerShell-opdrachtprompt:

```powershell
Connect-AzAccount
```

Als u meerdere Azure-abonnementen hebt, u aanmelden bij Azure in, hebt u toegang tot alle de Azure-abonnementen die zijn gekoppeld aan uw referenties. Gebruik de volgende opdracht om de Azure-abonnementen beschikbaar voor gebruik weer te geven:

```powershell
Get-AzSubscription
```

Gebruik de volgende opdracht om het abonnement te selecteren dat u wilt gebruiken voor het uitvoeren van de opdrachten voor het maken van uw IoT-hub. U kunt de naam van het abonnement of de id van de uitvoer van de vorige opdracht gebruiken:

```powershell
Select-AzSubscription `
    -SubscriptionName "{your subscription name}"
```

U kunt de volgende opdrachten gebruiken om te ontdekken waar u een IoT-hub en de momenteel ondersteunde API-versies kunt implementeren:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Maak een resourcegroep voor uw IoT-hub met behulp van de volgende opdracht uit in een van de ondersteunde locaties voor IoT-Hub. In dit voorbeeld maakt u een resourcegroep met de naam **MyIoTRG1**:

```powershell
New-AzResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-to-create-an-iot-hub"></a>Indienen van een sjabloon voor het maken van een IoT-hub

Een JSON-sjabloon gebruiken om te maken van een IoT-hub in de resourcegroep. U kunt ook een Azure Resource Manager-sjabloon gebruiken om een bestaande IoT-hub te wijzigen.

1. Gebruik een teksteditor om u te maken van een Azure Resource Manager-sjabloon met de naam **template.json** met de volgende resourcedefinitie te maken van een nieuwe standaard IoT-hub. In dit voorbeeld wordt de IoT Hub in de **VS-Oost** regio, maakt u twee consumer-groepen (**cg1** en **cg2**) op de Event Hub-compatibele eindpunt en maakt gebruik van de  **03-02-2016** API-versie. Deze sjabloon ook wordt verwacht dat u in de naam van de IoT hub als een parameter doorgeven met de naam **hubName**. Zie voor de huidige lijst met locaties die ondersteuning bieden voor IoT Hub [Azure Status][lnk-status].

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
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
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

2. Sla het Azure Resource Manager sjabloonbestand op uw lokale computer. In dit voorbeeld wordt ervan uitgegaan dat u deze opslaat in een map genaamd **c:\templates**.

3. Voer de volgende opdracht voor het implementeren van uw nieuwe IoT-hub, de naam van uw IoT-hub wordt doorgegeven als parameter. In dit voorbeeld wordt de naam van de IoT-hub is `abcmyiothub`. De naam van uw IoT-hub moet globaal uniek zijn:

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. De uitvoer geeft de sleutels voor de IoT-hub die u hebt gemaakt.

5. Als u wilt controleren of uw toepassing toegevoegd de nieuwe IoT hub, gaat u naar de [Azure-portal] [ lnk-azure-portal] en uw lijst met resources weer te geven. U kunt ook de **Get-AzResource** PowerShell-cmdlet.

> [!NOTE]
> In dit voorbeeld van de toepassing wordt toegevoegd een Standard IoT-Hub van S1, die in rekening worden gebracht. U kunt de IoT-hub via verwijderen de [Azure-portal] [ lnk-azure-portal] of met behulp van de **Remove-AzResource** PowerShell-cmdlet als u klaar bent.

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT-hub met behulp van een Azure Resource Manager-sjabloon met PowerShell hebt geïmplementeerd, kunt u verder te verkennen:

* Meer informatie over de mogelijkheden van de [REST API voor IoT Hub-resourceprovider][lnk-rest-api].
* Lezen [overzicht van Azure Resource Manager] [ lnk-azure-rm-overview] voor meer informatie over de mogelijkheden van Azure Resource Manager.
* Zie voor de JSON-syntaxis en de eigenschappen voor gebruik in sjablonen, [Microsoft.Devices resourcetypen](/azure/templates/microsoft.devices/iothub-allversions).

Zie de volgende artikelen voor meer informatie over het ontwikkelen van IoT-Hub:

* [Inleiding tot C SDK][lnk-c-sdk]
* [Azure IoT SDK 's][lnk-sdks]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-Az-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
