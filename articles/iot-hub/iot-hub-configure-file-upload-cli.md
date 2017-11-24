---
title: Bestand uploaden naar IoT Hub met Azure CLI (az.py) configureren | Microsoft Docs
description: Het configureren van uploads met Azure IoT Hub met behulp van de platformoverschrijdende Azure CLI 2.0 (az.py).
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
ms.openlocfilehash: 6b100e65aba604fd8becb02c3a205b3348872bc4
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a>Uploaden van bestanden met Azure CLI IoT-Hub configureren

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

Gebruik de [bestand uploaden functionaliteit in IoT-Hub][lnk-upload], moet u eerst een Azure Storage-account koppelen met uw IoT-hub. U kunt een bestaand opslagaccount gebruiken of een nieuwe maken.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.
* [Azure CLI 2.0][lnk-CLI-install].
* Een Azure-IoT-hub. Als u een IoT-hub hebt, kunt u de `az iot hub create` [opdracht] [ lnk-cli-create-iothub] een maken of gebruiken van de portal [maken van een IoT-hub] [lnk-portal-hub].
* Een Azure Storage-account. Als u geen Azure Storage-account hebt, kunt u de [Azure CLI 2.0 - storage-accounts beheren] [ lnk-manage-storage] te maken of de portal gebruiken om [een opslagaccount maken] [ lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Aanmelden en uw Azure-account instellen

Aanmelden bij uw Azure-account en uw abonnement te selecteren.

1. Voer bij de opdrachtprompt de [aanmelding opdracht][lnk-login-command]:

    ```azurecli
    az login
    ```

    Volg de instructies om te verifiëren met de code en meld u aan bij uw Azure-account via een webbrowser.

1. Als u meerdere Azure-abonnementen hebt, aanmelden bij Azure u toegang verleent tot de Azure accounts die zijn gekoppeld aan uw referenties. Gebruik de volgende [opdracht om een lijst van de Azure-accounts] [ lnk-az-account-command] beschikbaar moet worden gebruikt:

    ```azurecli
    az account list
    ```

    Gebruik de volgende opdracht om abonnement die u gebruiken wilt voor het uitvoeren van de opdrachten voor het maken van uw IoT-hub te selecteren. U kunt de naam van abonnement of de ID van de uitvoer van de vorige opdracht gebruiken:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a>De gegevens van uw storage-account ophalen

De volgende stappen wordt ervan uitgegaan dat u hebt gemaakt dat uw storage-account met de **Resource Manager** -implementatiemodel, en niet de **klassieke** implementatiemodel.

Als u wilt configureren bestandsuploads van uw apparaten, moet u de verbindingsreeks voor Azure storage-account. Het opslagaccount moet zich in hetzelfde abonnement als uw IoT-hub. U moet ook de naam van een blob-container in het opslagaccount. Gebruik de volgende opdracht voor het ophalen van uw toegangscodes voor opslag:

```azurecli
az storage account show-connection-string --name {your storage account name} --resource-group {your storage account resource group}
```

Noteer de **connectionString** waarde. U moet deze in de volgende stappen.

U kunt een bestaande blobcontainer voor uw bestandsuploads gebruiken of nieuwe maken:

* Als de bestaande blobcontainers in uw opslagaccount wilt weergeven, moet u de volgende opdracht gebruiken:

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* Voor het maken van een blob-container in uw opslagaccount, moet u de volgende opdracht gebruiken:

    ```azurecli
    az storage container create --name {container name} --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a>Bestand uploaden

U kunt nu uw IoT-hub om in te schakelen configureren [bestand uploaden functionaliteit] [ lnk-upload] met behulp van de gegevens van uw opslag.

De configuratie is vereist voor de volgende waarden:

**Storage-container**: een blobcontainer in Azure storage-account in uw huidige Azure-abonnement wilt koppelen aan uw IoT-hub. U hebt de accountgegevens voor de benodigde opslag in de vorige sectie hebt opgehaald. IoT Hub genereert automatisch SAS URI's met schrijfmachtigingen in voor deze blob-container voor apparaten voor gebruik bij het uploaden van bestanden.

**Meldingen ontvangen voor de geüploade bestanden**: bestand uploaden meldingen- of uitschakelen.

**SAS TTL**: deze instelling wordt de time-to-live van de SAS-URI's die aan het apparaat wordt geretourneerd door de IoT Hub. Standaard ingesteld op één uur.

**Bestand notification instellingen standaard TTL**: de time-to-live van een bestand uploaden melding voordat het is verlopen. Standaard ingesteld op één dag.

**Melding levering van het maximum aantal bestanden**: het aantal keren dat de IoT-Hub probeert een bestand uploaden een melding. Standaard ingesteld op 10.

Gebruik de volgende Azure CLI-opdrachten in het bestand uploaden om instellingen te configureren op uw IoT-hub:

Bij een bash-shell-toepassing:

```azurecli
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Op de gebruik van een Windows-opdrachtprompt:

```azurecli
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.connectionString="{your storage account connection string}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.containerName="{your storage container name}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.sasTtlAsIso8601=PT1H0M0S"

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

U kunt de configuratie van de uploaden bestand bekijken op uw IoT-hub met de volgende opdracht:

```azurecli
az iot hub show --name {your iot hub name}
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

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: iot-hub-security-ground-up.md


[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-manage-storage]:../storage/common/storage-azure-cli.md#manage-storage-accounts
[lnk-portal-storage]:../storage/common/storage-create-storage-account.md
[lnk-cli-create-iothub]: https://docs.microsoft.com/cli/azure/iot/hub#az_iot_hub_create