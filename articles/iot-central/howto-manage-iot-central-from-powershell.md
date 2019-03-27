---
title: IoT Central beheren vanuit Azure PowerShell | Microsoft Docs
description: IoT Central beheren vanuit Azure PowerShell.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 01/14/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 8d432d588cfd563ee48b93f9042339e9590bb5e2
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58481770"
---
# <a name="manage-iot-central-from-azure-powershell"></a>IoT Central beheren vanuit Azure PowerShell

[!INCLUDE [iot-central-selector-manage](../../includes/iot-central-selector-manage.md)]

In plaats van het maken en beheren van IoT Central-toepassingen van de IoT Central [Toepassingsbeheer](https://aka.ms/iotcentral) pagina, kunt u [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) voor het beheren van uw toepassingen.

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u liever om uit te voeren van Azure PowerShell op uw lokale computer, Zie [installeren van de Azure PowerShell-module](https://docs.microsoft.com/powershell/azure/install-az-ps). Wanneer u Azure PowerShell lokaal uitvoert, gebruikt u de **Connect AzAccount** cmdlet om aan te melden bij Azure voordat u de cmdlets in dit artikel.

## <a name="install-the-iot-central-module"></a>De IoT Central-module installeren

Voer de volgende opdracht om te controleren of de [IoT Central module](https://docs.microsoft.com/powershell/module/az.iotcentral/) is geïnstalleerd in uw PowerShell-omgeving:

```powershell
Get-InstalledModule -name Az.I*
```

Als de lijst met geïnstalleerde modules bevat geen **Az.IotCentral**, voer de volgende opdracht uit:

```powershell
Install-Module Az.IotCentral
```

## <a name="create-an-application"></a>Een app maken

Gebruik de [New-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/New-AzIotCentralApp) cmdlet voor het maken van een IoT Central-toepassing in uw Azure-abonnement. Bijvoorbeeld:

```powershell
# Create a resource group for the IoT Central application
New-AzResourceGroup -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Location "East US"
```

```powershell
# Create an IoT Central application
New-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Name "myiotcentralapp" -Subdomain "mysubdomain" `
  -Sku "S1" -Template "iotc-demo@1.0.0" `
  -DisplayName "My Custom Display Name"
```

Het script maakt u eerst een resourcegroep in de regio voor de toepassing VS-Oost. De volgende tabel beschrijft de parameters gebruikt met de **New-AzIotCentralApp** opdracht:

|Parameter         |Description |
|------------------|------------|
|ResourceGroupName |De resourcegroep die de toepassing bevat. Deze resourcegroep moet al bestaan in uw abonnement. |
|Locatie |Deze cmdlet maakt standaard gebruik van de locatie van de resourcegroep. Op dit moment kunt u een IoT Central-toepassing in de **VS-Oost**, **VS-West**, **Noord-Europa**, of **West-Europa** regio's. |
|Name              |De naam van de toepassing in Azure portal. |
|Subdomein         |Het subdomein in de URL van de toepassing. In het voorbeeld wordt de URL van de toepassing is https://mysubdomain.azureiotcentral.com. |
|Sku               |Op dit moment de enige waarde die is **S1** (standard-laag). Zie [prijzen voor Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/). |
|Template          | De toepassingssjabloon te gebruiken. Zie de volgende tabel voor meer informatie: |
|DisplayName       |De naam van de toepassing, zoals weergegeven in de gebruikersinterface. |

**Sjablonen voor toepassingen**

|Naam van sjabloon  |Description |
|---------------|------------|
|iotc-default@1.0.0 |Hiermee maakt u een lege toepassing die u kunt vullen met uw eigen apparaatsjablonen en apparaten. |
|iotc-demo@1.0.0    |Hiermee maakt u een toepassing die onder andere een vooraf gemaakte apparaatjabloon voor een gekoelde verkoopautomaat bevat. Gebruik deze sjabloon als u wilt beginnen met het verkennen van Azure IoT Central. |
|iotc-devkit-sample@1.0.0 |Hiermee maakt u een toepassing met apparaatsjablonen die u in staat stelt verbinding te maken met een MXChip- of Raspberry Pi-apparaat. Gebruik deze sjabloon als u een ontwikkelaar van het apparaat te experimenteren met een van deze apparaten. |

## <a name="view-your-iot-central-applications"></a>Uw toepassingen IoT Central bekijken

Gebruik de [Get-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Get-AzIotCentralApp) cmdlet lijst met uw IoT Central-toepassingen en weergeven van metagegevens.

## <a name="modify-an-application"></a>Wijzigen van een toepassing

Gebruik de [Set AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/set-aziotcentralapp) cmdlet voor het bijwerken van de metagegevens van een IoT Central-toepassing. Als u bijvoorbeeld de weergegeven naam van uw toepassing te wijzigen:

```powershell
Set-AzIotCentralApp -Name "myiotcentralapp" `
  -ResourceGroupName "MyIoTCentralResourceGroup" `
  -DisplayName "My new display name"
```

## <a name="remove-an-application"></a>Een toepassing verwijderen

Gebruik de [Remove-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Remove-AzIotCentralApp) cmdlet voor het verwijderen van een IoT Central-toepassing. Bijvoorbeeld:

```powershell
Remove-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
 -Name "myiotcentralapp"
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u Azure IoT Central om toepassingen te beheren vanuit Azure PowerShell, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Uw toepassing beheren](howto-administer.md)
