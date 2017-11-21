---
title: Maken van een Azure-IoT-Hub met een sjabloon (PowerShell) | Microsoft Docs
description: Het gebruik van een Azure Resource Manager-sjabloon maken van een IoT Hub met PowerShell.
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 7eade855-c289-4ffb-b5ef-02be8c5f670f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 242c50b61b00bbf71b26e2aea1a66e2b2c55dbd5
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>Een iothub met Azure Resource Manager-sjabloon (PowerShell) maken

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Azure Resource Manager kunt u maken en beheren van Azure IoT hubs via een programma. Deze zelfstudie laat zien hoe u een Azure Resource Manager-sjabloon maken van een iothub met PowerShell.

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Azure Resource Manager en classic](../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel wordt behandeld met het Azure Resource Manager-implementatiemodel.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-account. <br/>Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.
* [Azure PowerShell 1.0] [ lnk-powershell-install] of hoger.

> [!TIP]
> Het artikel [Azure PowerShell gebruiken met Azure Resource Manager] [ lnk-powershell-arm] vindt u meer informatie over het gebruik van PowerShell en Azure Resource Manager-sjablonen voor het maken van Azure-resources.

## <a name="connect-to-your-azure-subscription"></a>Verbinding maken met uw Azure-abonnement

Voer de volgende opdracht aan te melden bij uw Azure-abonnement in een PowerShell-opdrachtprompt:

```powershell
Login-AzureRmAccount
```

Als u meerdere Azure-abonnementen hebt, verleent aanmelden bij Azure u toegang tot alle de Azure-abonnementen die zijn gekoppeld aan uw referenties. Gebruik de volgende opdracht voor een lijst met de Azure-abonnementen beschikbaar moet worden gebruikt:

```powershell
Get-AzureRMSubscription
```

Gebruik de volgende opdracht om abonnement die u gebruiken wilt voor het uitvoeren van de opdrachten voor het maken van uw IoT-hub te selecteren. U kunt de naam van abonnement of de ID van de uitvoer van de vorige opdracht gebruiken:

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

U kunt de volgende opdrachten gebruiken om te detecteren waarin u een IoT-hub en de momenteel ondersteunde API-versies kunt implementeren:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Maak een resourcegroep voor uw IoT-hub met de volgende opdracht in een van de ondersteunde locaties voor IoT Hub bevatten. In dit voorbeeld maakt u een resourcegroep aangeroepen **MyIoTRG1**:

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-to-create-an-iot-hub"></a>Een sjabloon voor het maken van een IoT-hub te verzenden

Gebruik een JSON-sjabloon maken van een IoT-hub in de resourcegroep. U kunt ook een Azure Resource Manager-sjabloon gebruiken om een bestaande IoT-hub te wijzigen.

1. Gebruik een teksteditor te maken van een Azure Resource Manager-sjabloon aangeroepen **template.json** met de volgende resourcedefinitie maken van een nieuwe standaard IoT-hub. In dit voorbeeld wordt de IoT-Hub in de **VS-Oost** regio, maakt u twee consumergroepen (**cg1** en **cg2**) op de Event Hub-compatibele eindpunt en gebruikt de **2016-02-03** API-versie. Deze sjabloon verwacht u doorgeven in de naam van de IoT-hub als een parameter genaamd **hubName**. Zie voor de huidige lijst van de locaties die ondersteuning bieden voor IoT Hub [Azure Status][lnk-status].

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

2. Sla het sjabloonbestand Azure Resource Manager op uw lokale machine. In dit voorbeeld wordt ervan uitgegaan dat u deze opslaan in een map met de naam **c:\templates**.

3. Voer de volgende opdracht voor het implementeren van uw nieuwe iothub, de naam van uw IoT-hub wordt doorgegeven als parameter. In dit voorbeeld wordt de naam van de IoT-hub is `abcmyiothub`. De naam van uw IoT-hub moet wereldwijd uniek zijn:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. De uitvoer geeft de sleutels voor de IoT-hub die u hebt gemaakt.

5. Als u wilt controleren of uw toepassing de nieuwe iothub toegevoegd, gaat u naar de [Azure-portal] [ lnk-azure-portal] en uw lijst met resources weergeven. U kunt ook de **Get-AzureRmResource** PowerShell-cmdlet.

> [!NOTE]
> Deze voorbeeldtoepassing voegt een standaard IoT-Hub van S1, waarvoor u wordt gefactureerd. U kunt de IoT-hub via verwijderen de [Azure-portal] [ lnk-azure-portal] of met behulp van de **verwijderen AzureRmResource** PowerShell-cmdlet als u klaar bent.

## <a name="next-steps"></a>Volgende stappen

Nu u een iothub met een Azure Resource Manager-sjabloon met PowerShell hebt geïmplementeerd, kunt u verder verkennen:

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
[lnk-powershell-install]: /powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
