---
title: Verbinding maken met en beheren van Microsoft Azure Data Box Edge-apparaat via de Windows PowerShell-interface | Microsoft Docs
description: Beschrijft hoe u verbinding maken met en gegevens in Edge vervolgens beheren via de Windows PowerShell-interface.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/21/2019
ms.author: alkohli
ms.openlocfilehash: 9b0e94deda205497cda4ebf383f302c6c3bb896a
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403592"
---
# <a name="manage-an-azure-data-box-edge-device-via-windows-powershell"></a>Een Azure Data Box-Edge-apparaat via Windows PowerShell beheren

Azure Data Box-Edge-oplossing kunt u gegevens verwerken en te verzenden via het netwerk naar Azure. Dit artikel worden enkele van de configuratie en beheer taken voor uw gegevens in het Edge-apparaat. U kunt de Azure portal, de lokale web-UI of de Windows PowerShell-interface gebruiken om uw apparaat te beheren.

In dit artikel richt zich op de taken die u met behulp van de PowerShell-interface.

In dit artikel bevat de volgende procedures:

- Verbinding maken met de PowerShell-interface
- Een ondersteuningssessie starten
- Een ondersteuningspakket maken
- Certificaat uploaden
- Het apparaat opnieuw instellen
- Gegevens van een apparaat weergeven
- Compute-logboeken ophalen
- Bewaken en problemen oplossen compute-modules

## <a name="connect-to-the-powershell-interface"></a>Verbinding maken met de PowerShell-interface

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="create-a-support-package"></a>Een ondersteuningspakket maken

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Certificaat uploaden

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

## <a name="view-device-information"></a>Gegevens van een apparaat weergeven
 
[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="reset-your-device"></a>Uw apparaat opnieuw instelt

[!INCLUDE [Reset your device](../../includes/data-box-edge-gateway-deactivate-device.md)]

## <a name="get-compute-logs"></a>Compute-logboeken ophalen

Als de compute-rol is geconfigureerd op uw apparaat, kunt u ook de compute-logboeken ophalen via de PowerShell-interface.

1. [Verbinding maken met de PowerShell-interface](#connect-to-the-powershell-interface).
2. Gebruik de `Get-AzureDataBoxEdgeComputeRoleLogs` om op te halen van de compute-logboeken voor uw apparaat.

    Het volgende voorbeeld ziet u het gebruik van deze cmdlet:

    ```
    Get-AzureDataBoxEdgeComputeRoleLogs -Path "\\hcsfs\logs\myacct" -Credential "username/password" -RoleInstanceName "IotRole" -FullLogCollection
    ```
    Hier volgt een beschrijving van de parameters voor de cmdlet:
    - `Path`: Geef een netwerkpad naar de bestandsshare waar u de compute-log-pakket maken.
    - `Credential`: Geef de gebruikersnaam en wachtwoord voor de netwerkshare.
    - `RoleInstanceName`: Geef deze tekenreeks `IotRole` voor deze parameter.
    - `FullLogCollection`: Deze parameter zorgt ervoor dat het logboek-pakket alle compute-Logboeken bevat. Het logboek-pakket bevat standaard alleen een subset van Logboeken.

## <a name="monitor-and-troubleshoot-compute-modules"></a>Bewaken en problemen oplossen compute-modules

[!INCLUDE [Monitor and troubleshoot compute modules](../../includes/data-box-edge-monitor-troubleshoot-compute.md)]


## <a name="next-steps"></a>Volgende stappen

- [Azure Data Box Edge](data-box-edge-deploy-prep.md) in de Azure-portal implementeren.
