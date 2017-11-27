---
title: Een Azure-IoT-Hub met een PowerShell-cmdlet maken | Microsoft Docs
description: Het gebruik van een PowerShell-cmdlet voor het maken van een IoT-hub.
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 1d22e1a20d18e06686f893b1f4ae22e6373633b3
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="create-an-iot-hub-using-the-new-azurermiothub-cmdlet"></a>Een iothub met de cmdlet New-AzureRmIotHub maken

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Inleiding

U kunt Azure PowerShell-cmdlets gebruiken om te maken en beheren van Azure IoT hubs. Deze zelfstudie laat zien hoe u een IoT-hub maken met PowerShell.

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Azure Resource Manager en classic](../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel wordt behandeld met het Azure Resource Manager-implementatiemodel.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-account. <br/>Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.
* [Azure PowerShell-cmdlets][lnk-powershell-install].

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

## <a name="create-resource-group"></a>Een resourcegroep maken

U moet een resourcegroep voor het implementeren van een IoT-hub. U kunt een bestaande resourcegroep gebruiken of een nieuwe maken.

U kunt de volgende opdracht gebruiken voor het detecteren van de locaties waar u een IoT-hub kunt implementeren:

```powershell
((Get-AzureRmResourceProvider `
  -ProviderNamespace Microsoft.Devices).ResourceTypes `
  | Where-Object ResourceTypeName -eq IoTHubs).Locations
```

Gebruik de volgende opdracht voor het maken van een resourcegroep voor uw IoT-hub in een van de ondersteunde locaties voor IoT Hub. In dit voorbeeld maakt u een resourcegroep aangeroepen **MyIoTRG1** in de **VS-Oost** regio:

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

Voor het maken van een IoT-hub in de resourcegroep die u in de vorige stap hebt gemaakt, moet u de volgende opdracht gebruiken. In dit voorbeeld wordt een **S1** hub aangeroepen **MyTestIoTHub** in de **VS-Oost** regio:

```powershell
New-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

De naam van de IoT-hub moet uniek zijn.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


U kunt alle IoT-hubs weergeven in uw abonnement met de volgende opdracht:

```powershell
Get-AzureRmIotHub
```

Het vorige voorbeeld wordt een standaard IoT-Hub van S1, waarvoor u wordt gefactureerd. U kunt de iothub met de volgende opdracht verwijderen:

```powershell
Remove-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

U kunt ook kunt u een resourcegroep en alle resources die deze bevat de volgende opdracht:

```powershell
Remove-AzureRmResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a>Volgende stappen

Nu u een iothub met een PowerShell-cmdlet hebt geïmplementeerd, kunt u verder verkennen:

* Detecteren van andere [PowerShell-cmdlets voor het werken met uw IoT-hub][lnk-iothub-cmdlets].
* Meer informatie over de mogelijkheden van de [resourceprovider IoT Hub REST-API][lnk-rest-api].

Zie de volgende artikelen voor meer informatie over het ontwikkelen voor IoT-Hub:

* [Inleiding tot C-SDK][lnk-c-sdk]
* [Azure IoT SDK 's][lnk-sdks]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op de edge-apparaten met Azure IoT rand][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-iothub-cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.iothub/
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
