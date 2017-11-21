---
title: Azure PowerShell gebruiken om te uploaden bestand configureren | Microsoft Docs
description: Het gebruik van de Azure PowerShell-cmdlets voor het configureren van uw IoT-hub zodat bestand uploadt van verbonden apparaten. Bevat informatie over het configureren van de doel-Azure storage-account.
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: c6966fd4a60681643c2a690013035bde20abee78
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="configure-iot-hub-file-uploads-using-powershell"></a>Uploaden van bestanden met PowerShell IoT-Hub configureren

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

Gebruik de [bestand uploaden functionaliteit in IoT-Hub][lnk-upload], moet u eerst een Azure storage-account koppelen met uw IoT-hub. U kunt een bestaand opslagaccount gebruiken of een nieuwe maken.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.
* [Azure PowerShell-cmdlets][lnk-powershell-install].
* Een Azure-IoT-hub. Als u een IoT-hub hebt, kunt u de [cmdlet New-AzureRmIoTHub] [ lnk-powershell-iothub] te maken of de portal gebruiken om [een iothub maken][lnk-portal-hub].
* Een Azure Storage-account. Als u geen Azure storage-account hebt, kunt u de [Azure PowerShell-cmdlets Storage] [ lnk-powershell-storage] te maken of de portal gebruiken om [een opslagaccount maken] [lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Aanmelden en uw Azure-account instellen

Aanmelden bij uw Azure-account en uw abonnement te selecteren.

1. Uitvoeren op de PowerShell-prompt de **Login-AzureRmAccount** cmdlet:

    ```powershell
    Login-AzureRmAccount
    ```

1. Als u meerdere Azure-abonnementen hebt, verleent aanmelden bij Azure u toegang tot alle de Azure-abonnementen die zijn gekoppeld aan uw referenties. Gebruik de volgende opdracht voor een lijst met de Azure-abonnementen beschikbaar moet worden gebruikt:

    ```powershell
    Get-AzureRMSubscription
    ```

    Gebruik de volgende opdracht om abonnement die u gebruiken wilt voor het uitvoeren van de opdrachten voor het beheren van uw IoT-hub te selecteren. U kunt de naam van abonnement of de ID van de uitvoer van de vorige opdracht gebruiken:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a>De gegevens van uw storage-account ophalen

De volgende stappen wordt ervan uitgegaan dat u hebt gemaakt dat uw storage-account met de **Resource Manager** -implementatiemodel, en niet de **klassieke** implementatiemodel.

Als u wilt configureren bestandsuploads van uw apparaten, moet u de verbindingsreeks voor Azure storage-account. Het opslagaccount moet zich in hetzelfde abonnement als uw IoT-hub. U moet ook de naam van een blob-container in het opslagaccount. Gebruik de volgende opdracht voor het ophalen van uw toegangscodes voor opslag:

```powershell
Get-AzureRmStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

Noteer de **key1** sleutelwaarde storage-account. U moet deze in de volgende stappen.

U kunt een bestaande blobcontainer voor uw bestandsuploads gebruiken of nieuwe maken:

* Als de bestaande blobcontainers in uw opslagaccount wilt weergeven, gebruikt u de volgende opdrachten:

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzureStorageContainer -Context $ctx
    ```

* Voor het maken van een blob-container in uw opslagaccount, gebruik de volgende opdrachten:

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzureStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a>Configureren van uw IoT-hub

U kunt nu uw IoT-hub om in te schakelen configureren [bestand uploaden functionaliteit] [ lnk-upload] met behulp van de gegevens van uw opslag.

De configuratie is vereist voor de volgende waarden:

**Storage-container**: een blobcontainer in Azure storage-account in uw huidige Azure-abonnement wilt koppelen aan uw IoT-hub. U hebt de accountgegevens voor de benodigde opslag in de vorige sectie hebt opgehaald. IoT Hub genereert automatisch SAS URI's met schrijfmachtigingen in voor deze blob-container voor apparaten voor gebruik bij het uploaden van bestanden.

**Meldingen ontvangen voor de geüploade bestanden**: bestand uploaden meldingen- of uitschakelen.

**SAS TTL**: deze instelling wordt de time-to-live van de SAS-URI's die aan het apparaat wordt geretourneerd door de IoT Hub. Standaard ingesteld op één uur.

**Bestand notification instellingen standaard TTL**: de time-to-live van een bestand uploaden melding voordat het is verlopen. Standaard ingesteld op één dag.

**Melding levering van het maximum aantal bestanden**: het aantal keren dat de IoT-Hub probeert een bestand uploaden een melding. Standaard ingesteld op 10.

Gebruik de volgende PowerShell-cmdlet voor het configureren van het bestand uploaden instellingen op uw IoT-hub:

```powershell
Set-AzureRmIotHub `
    -ResourceGroupName "{your iot hub resource group}" `
    -Name "{your iot hub name}" `
    -FileUploadNotificationTtl "01:00:00" `
    -FileUploadSasUriTtl "01:00:00" `
    -EnableFileUploadNotifications $true `
    -FileUploadStorageConnectionString "DefaultEndpointsProtocol=https;AccountName={your storage account name};AccountKey={your storage account key};EndpointSuffix=core.windows.net" `
    -FileUploadContainerName "{your blob container name}" `
    -FileUploadNotificationMaxDeliveryCount 10
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de functies voor het uploaden van bestanden van IoT Hub [uploaden van bestanden van een apparaat][lnk-upload].

Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [Bulksgewijs IoT-apparaten beheren][lnk-bulk]
* [IoT Hub metrische gegevens][lnk-metrics]
* [Bewerkingen controleren][lnk-monitor]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [AI implementeren op de edge-apparaten met Azure IoT rand][lnk-iotedge]
* [Beveiligen van uw IoT-oplossing bouwen up][lnk-securing]

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-powershell-storage]: https://docs.microsoft.com/powershell/module/azurerm.storage/
[lnk-powershell-iothub]: https://docs.microsoft.com/powershell/module/azurerm.iothub/new-azurermiothub
[lnk-portal-hub]: iot-hub-create-through-portal.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal-storage]:../storage/common/storage-create-storage-account.md