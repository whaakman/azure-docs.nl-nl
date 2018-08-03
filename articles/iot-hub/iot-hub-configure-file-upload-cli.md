---
title: Uploaden van bestanden naar IoT Hub met behulp van Azure CLI (az.py) configureren | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van uploads voor Azure IoT Hub met behulp van de platformoverschrijdende Azure CLI 2.0 (az.py).
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 0eac620d44967827f7703da9cf409703a123ab07
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39460196"
---
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a>IoT Hub-bestand wordt geüpload met behulp van Azure CLI configureren

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

Gebruik de [bestand functionaliteit voor het uploaden van IoT-Hub][lnk-upload], moet u eerst een Azure Storage-account koppelen met uw IoT-hub. U kunt een bestaand opslagaccount gebruiken of een nieuwe maken.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.
* [Azure CLI 2.0][lnk-CLI-install].
* Een Azure IoT-hub. Als u een IoT-hub hebt, kunt u de `az iot hub create` [opdracht] [ lnk-cli-create-iothub] te maken of de portal gebruiken [een IoT-hub maken] [lnk-portal-hub].
* Een Azure Storage-account. Als u een Azure Storage-account niet hebt, kunt u de [Azure CLI 2.0 - storage-accounts beheren] [ lnk-manage-storage] te maken of de portal gebruiken om [een opslagaccount maken] [ lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Meld u aan en stel uw Azure-account

Meld u aan bij uw Azure-account en selecteer uw abonnement.

1. Voer bij de opdrachtprompt deze [aanmeldingsopdracht][lnk-login-command] uit:

    ```azurecli
    az login
    ```

    Volg de instructies om te verifiëren met de code en meld u aan bij uw Azure-account via een webbrowser.

1. Als u meerdere Azure-abonnementen hebt en u zich aanmeldt bij Azure, hebt u toegang tot alle Azure accounts die zijn gekoppeld aan uw referenties. Gebruik de volgende [opdracht om de Azure-accounts weer te geven][lnk-az-account-command] die u kunt gebruiken:

    ```azurecli
    az account list
    ```

    Gebruik de volgende opdracht om het abonnement te selecteren dat u wilt gebruiken voor het uitvoeren van de opdrachten voor het maken van uw IoT-hub. U kunt de naam van het abonnement of de id van de uitvoer van de vorige opdracht gebruiken:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a>Details van uw opslagaccount ophalen

De volgende stappen wordt ervan uitgegaan dat u hebt gemaakt uw storage-account met de **Resource Manager** implementatiemodel, en niet de **klassieke** implementatiemodel.

Voor het configureren van het uploaden van bestanden van uw apparaten, moet u de verbindingsreeks voor een Azure storage-account. Het opslagaccount moet zich in hetzelfde abonnement als uw IoT-hub. U moet ook de naam van een blob-container in het opslagaccount. Gebruik de volgende opdracht om op te halen van de sleutels van uw storage-account:

```azurecli
az storage account show-connection-string --name {your storage account name} --resource-group {your storage account resource group}
```

Noteer de **connectionString** waarde. U hebt deze nodig in de volgende stappen.

U kunt een bestaande blobcontainer gebruiken voor het uploaden van uw bestanden of nieuwe maken:

* Als u de bestaande blob-containers in uw opslagaccount, gebruik de volgende opdracht:

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* Voor het maken van een blob-container in uw opslagaccount, gebruikt u de volgende opdracht uit:

    ```azurecli
    az storage container create --name {container name} --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a>Bestand uploaden

U kunt nu uw IoT-hub om in te schakelen configureren [functionaliteit voor het uploaden bestand] [ lnk-upload] met behulp van de details van uw opslagaccount.

De configuratie moet de volgende waarden:

**Storage-container**: een blob-container in Azure storage-account in uw huidige Azure-abonnement koppelen aan uw IoT-hub. U hebt de benodigde opslag-accountgegevens in de voorgaande sectie hebt opgehaald. IoT Hub wordt automatisch gegenereerd SAS URI's met schrijfmachtigingen voor deze blob-container voor apparaten moet worden gebruikt bij het uploaden van bestanden.

**Meldingen over geüploade bestanden ontvangen**: bestand uploaden meldingen in of uit.

**SAS TTL**: deze instelling wordt de time-to-live van de SAS-URI's op het apparaat wordt geretourneerd door de IoT Hub. Standaard ingesteld op één uur.

**Melding instellingen standaard TTL-bestand**: de time-to-live van een bestand uploaden melding voordat deze is verlopen. Standaard ingesteld op één dag.

**Maximumaantal leveringen voor melding bestand**: het aantal keren dat de IoT-Hub wil ervoor zorgen dat een bestand uploaden een melding. Standaard ingesteld op 10.

Gebruik de volgende Azure CLI-opdrachten voor het configureren van instellingen voor het uploaden van uw IoT-hub:

In een bash-shell gebruiken:

```azurecli
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Op een Windows-opdrachtregel de opdracht gebruiken:

```azurecli
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.connectionString="{your storage account connection string}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.containerName="{your storage container name}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.sasTtlAsIso8601=PT1H0M0S"

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

U kunt de configuratie van de uploaden bestand bekijken op uw IoT-hub met behulp van de volgende opdracht uit:

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de functies voor het uploaden van bestanden van IoT-Hub, [uploaden van bestanden vanaf een apparaat][lnk-upload].

Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [IoT-apparaten bulksgewijs beheren][lnk-bulk]
* [Metrische gegevens van IoT Hub][lnk-metrics]
* [Bewerkingen controleren][lnk-monitor]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]
* [Beveiligen van uw IoT-oplossing vanaf het begin van][lnk-securing]

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: /azure/iot-fundamentals/iot-security-ground-up


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
[lnk-cli-create-iothub]: https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-create