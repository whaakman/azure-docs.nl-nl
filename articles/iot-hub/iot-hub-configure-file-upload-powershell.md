---
title: Azure PowerShell gebruikt voor het uploaden van bestanden configureren | Microsoft Docs
description: Hoe u Azure PowerShell-cmdlets gebruiken om te configureren van uw IoT-hub zodat het bestand wordt geüpload van verbonden apparaten. Bevat informatie over het configureren van de bestemming Azure storage-account.
author: dominicbetts
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 7d63cc4e57ba3c1b962c893bf8c8bd03664dac6f
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729251"
---
# <a name="configure-iot-hub-file-uploads-using-powershell"></a>IoT Hub-bestand wordt geüpload met behulp van PowerShell configureren

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

Gebruik de [bestand functionaliteit voor het uploaden van IoT-Hub](iot-hub-devguide-file-upload.md), moet u eerst een Azure storage-account koppelen met uw IoT-hub. U kunt een bestaand opslagaccount gebruiken of een nieuwe maken.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-account. Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.

* [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/install-Az-ps).

* Een Azure IoT-hub. Als u een IoT-hub hebt, kunt u de [cmdlet New-AzIoTHub](https://docs.microsoft.com/powershell/module/az.iothub/new-aziothub) te maken of de portal gebruiken om [maken van een IoT-hub](iot-hub-create-through-portal.md).

* Een Azure Storage-account. Als u geen Azure storage-account hebt, kunt u de [Azure PowerShell-cmdlets Storage](https://docs.microsoft.com/powershell/module/az.storage/) te maken of de portal gebruiken om [een opslagaccount maken](../storage/common/storage-create-storage-account.md)

## <a name="sign-in-and-set-your-azure-account"></a>Meld u aan en stel uw Azure-account

Meld u aan bij uw Azure-account en selecteer uw abonnement.

1. Voer bij de PowerShell-prompt de **Connect AzAccount** cmdlet:

    ```powershell
    Connect-AzAccount
    ```

2. Als u meerdere Azure-abonnementen hebt, u aanmelden bij Azure in, hebt u toegang tot alle de Azure-abonnementen die zijn gekoppeld aan uw referenties. Gebruik de volgende opdracht om de Azure-abonnementen beschikbaar voor gebruik weer te geven:

    ```powershell
    Get-AzSubscription
    ```

    Gebruik de volgende opdracht om het abonnement dat u wilt gebruiken voor het uitvoeren van de opdrachten voor het beheren van uw IoT-hub te selecteren. U kunt de naam van het abonnement of de id van de uitvoer van de vorige opdracht gebruiken:

    ```powershell
    Select-AzSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a>Details van uw opslagaccount ophalen

De volgende stappen wordt ervan uitgegaan dat u hebt gemaakt uw storage-account met de **Resource Manager** implementatiemodel, en niet de **klassieke** implementatiemodel.

Voor het configureren van het uploaden van bestanden van uw apparaten, moet u de verbindingsreeks voor een Azure storage-account. Het opslagaccount moet zich in hetzelfde abonnement als uw IoT-hub. U moet ook de naam van een blob-container in het opslagaccount. Gebruik de volgende opdracht om op te halen van de sleutels van uw storage-account:

```powershell
Get-AzStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

Noteer de **key1** sleutelwaarde voor storage-account. U hebt deze nodig in de volgende stappen.

U kunt een bestaande blobcontainer gebruiken voor het uploaden van uw bestanden of nieuwe maken:

* Als u de bestaande blob-containers in uw opslagaccount, gebruikt u de volgende opdrachten:

    ```powershell
    $ctx = New-AzStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzStorageContainer -Context $ctx
    ```

* Voor het maken van een blob-container in uw opslagaccount, gebruikt u de volgende opdrachten:

    ```powershell
    $ctx = New-AzStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a>Uw IoT hub configureren

U kunt nu uw IoT-hub kunt u configureren [bestanden uploaden naar de IoT-hub](iot-hub-devguide-file-upload.md) met behulp van de details van uw opslagaccount.

De configuratie moet de volgende waarden:

* **Storage-container**: Een blob-container in Azure storage-account in uw huidige Azure-abonnement koppelen aan uw IoT-hub. U hebt de benodigde opslag-accountgegevens in de voorgaande sectie hebt opgehaald. IoT Hub wordt automatisch gegenereerd SAS URI's met schrijfmachtigingen voor deze blob-container voor apparaten moet worden gebruikt bij het uploaden van bestanden.

* **Meldingen over geüploade bestanden ontvangen**: In- of bestand uploaden meldingen uitschakelen.

* **SAS TTL**: Deze instelling is de time-to-live van de SAS-URI's op het apparaat wordt geretourneerd door de IoT Hub. Standaard ingesteld op één uur.

* **Melding instellingen standaard TTL-bestand**: De time-to-live van een bestand uploaden van melding voordat deze is verlopen. Standaard ingesteld op één dag.

* **Maximumaantal leveringen voor melding bestand**: Het aantal keren dat die de IoT-Hub wil ervoor zorgen dat een bestand uploaden een melding. Standaard ingesteld op 10.

Gebruik de volgende PowerShell-cmdlet voor het configureren van het bestand uploaden-instellingen op uw IoT-hub:

```powershell
Set-AzIotHub `
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

Zie voor meer informatie over de functies voor het uploaden van bestanden van IoT-Hub, [uploaden van bestanden vanaf een apparaat](iot-hub-devguide-file-upload.md).

Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [IoT-apparaten bulksgewijs beheren](iot-hub-bulk-identity-mgmt.md)
* [Metrische gegevens van IoT Hub](iot-hub-metrics.md)
* [Controle van bewerkingen](iot-hub-operations-monitoring.md)

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md)
* [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
* [Beveiligen van uw IoT-oplossing vanaf het begin van](../iot-fundamentals/iot-security-ground-up.md)
