---
title: Een Azure IoT Hub maken met een PowerShell-cmdlet | Microsoft Docs
description: Het gebruik van een PowerShell-cmdlet om te maken van een IoT-hub.
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: robinsh
ms.openlocfilehash: 6462673f6c3992aacbaee168eafc6bdb1b2fa944
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56733467"
---
# <a name="create-an-iot-hub-using-the-new-aziothub-cmdlet"></a>Een IoT-hub met behulp van de cmdlet New-AzIotHub maken

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Inleiding

U kunt Azure PowerShell-cmdlets gebruiken om te maken en beheren van Azure IoT-hubs. Deze zelfstudie leert u hoe u een IoT-hub maken met PowerShell.

Als u wilt deze procedures hebt voltooid, moet u een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="connect-to-your-azure-subscription"></a>Verbinding maken met uw Azure-abonnement

Als u van de Cloud Shell gebruikmaakt, bent u al aangemeld bij uw abonnement. Als u PowerShell lokaal in plaats daarvan uitvoert, voert u de volgende opdracht uit om aan te melden bij uw Azure-abonnement:

```powershell
# Log into Azure account.
Login-AzAccount
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

U moet een resourcegroep maken voor het implementeren van een IoT-hub. U kunt een bestaande resourcegroep gebruiken of een nieuwe maken.

Gebruik voor het maken van een resourcegroep voor uw IoT-hub de [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.Resources/New-azResourceGroup) opdracht. In dit voorbeeld maakt u een resourcegroep met de naam **MyIoTRG1** in de **VS-Oost** regio:

```azurepowershell-interactive
New-AzResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

Voor het maken van een IoT-hub in de resourcegroep die u in de vorige stap hebt gemaakt, gebruikt u de [New-AzIotHub](https://docs.microsoft.com/powershell/module/az.IotHub/New-azIotHub) opdracht. Dit voorbeeld maakt u een **S1** hub met de naam **MyTestIoTHub** in de **VS-Oost** regio:

```azurepowershell-interactive
New-AzIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

De naam van de IoT-hub moet globaal uniek zijn.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

U kunt alle IoT-hubs weergeven in uw abonnement met de [Get-AzIotHub](https://docs.microsoft.com/powershell/module/az.IotHub/Get-azIotHub) opdracht:

```azurepowershell-interactive
Get-AzIotHub
```

Dit voorbeeld toont de S1 Standard IoT-Hub die u in de vorige stap hebt gemaakt.

U kunt verwijderen met de IoT hub met behulp van de [Remove-AzIotHub](https://docs.microsoft.com/powershell/module/az.iothub/remove-aziothub) opdracht:

```azurepowershell-interactive
Remove-AzIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

U kunt ook kunt u een resourcegroep en alle resources die deze bevat met behulp van de [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.Resources/Remove-azResourceGroup) opdracht:

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a>Volgende stappen

U hebt nu een IoT-hub met behulp van een PowerShell-cmdlet geïmplementeerd als u wilt verder te verkennen, kunt u de volgende artikelen:

* [PowerShell-cmdlets voor het werken met uw IoT-hub](https://docs.microsoft.com/powershell/module/az.iothub/).

* [REST-API voor IoT Hub-resourceprovider](https://docs.microsoft.com/rest/api/iothub/iothubresource).

Zie de volgende artikelen voor meer informatie over het ontwikkelen van IoT-Hub:

* [Inleiding tot C SDK](iot-hub-device-sdk-c-intro.md)

* [SDK's voor Azure IoT](iot-hub-devguide-sdks.md)

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
