---
title: Uploaden van bestanden naar IoT Hub met behulp van Azure CLI configureren | Microsoft Docs
description: Het configureren van bestand wordt geüpload naar Azure IoT Hub met behulp van de platformoverschrijdende Azure CLI.
author: dominicbetts
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 6cd0b657c8d0352c41e0da538396b166d633306a
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42055310"
---
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a>IoT Hub-bestand wordt geüpload met behulp van Azure CLI configureren

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

Naar [uploaden van bestanden vanaf een apparaat](iot-hub-devguide-file-upload.md), moet u eerst een Azure Storage-account koppelen met uw IoT-hub. U kunt een bestaand opslagaccount gebruiken of een nieuwe maken.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-account. Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.

* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

* Een Azure IoT-hub. Als u een IoT-hub hebt, kunt u de [ `az iot hub create` opdracht](https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-create) een te maken of [maken van een IoT-hub met behulp van de portal](iot-hub-create-through-portal.md).

* Een Azure Storage-account. Als u een Azure Storage-account niet hebt, kunt u de [Azure CLI - storage-accounts beheren](../storage/common/storage-azure-cli.md#manage-storage-accounts) te maken of de portal gebruiken om [een opslagaccount maken](../storage/common/storage-create-storage-account.md).

## <a name="sign-in-and-set-your-azure-account"></a>Meld u aan en stel uw Azure-account

Meld u aan bij uw Azure-account en selecteer uw abonnement.

1. Voer bij de opdrachtprompt de [aanmeldingsopdracht](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest):

    ```azurecli
    az login
    ```

    Volg de instructies om te verifiëren met de code en meld u aan bij uw Azure-account via een webbrowser.

2. Als u meerdere Azure-abonnementen hebt en u zich aanmeldt bij Azure, hebt u toegang tot alle Azure accounts die zijn gekoppeld aan uw referenties. Gebruik de volgende [opdracht om een lijst van de Azure-accounts](https://docs.microsoft.com/cli/azure/account) die u kunt gebruiken:

    ```azurecli
    az account list
    ```

    Gebruik de volgende opdracht om het abonnement dat u wilt gebruiken voor het uitvoeren van de opdrachten voor het maken van uw IoT-hub te selecteren. U kunt de naam van het abonnement of de id van de uitvoer van de vorige opdracht gebruiken:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a>Details van uw opslagaccount ophalen

De volgende stappen wordt ervan uitgegaan dat u hebt gemaakt uw storage-account met de **Resource Manager** implementatiemodel, en niet de **klassieke** implementatiemodel.

Voor het configureren van het uploaden van bestanden van uw apparaten, moet u de verbindingsreeks voor een Azure storage-account. Het opslagaccount moet zich in hetzelfde abonnement als uw IoT-hub. U moet ook de naam van een blob-container in het opslagaccount. Gebruik de volgende opdracht om op te halen van de sleutels van uw storage-account:

```azurecli
az storage account show-connection-string --name {your storage account name} \
  --resource-group {your storage account resource group}
```

Noteer de **connectionString** waarde. U hebt deze nodig in de volgende stappen.

U kunt een bestaande blobcontainer gebruiken voor het uploaden van uw bestanden of een nieuwe maken:

* Als u de bestaande blob-containers in uw opslagaccount, gebruik de volgende opdracht:

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* Voor het maken van een blob-container in uw opslagaccount, gebruikt u de volgende opdracht uit:

    ```azurecli
    az storage container create --name {container name} \
      --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a>Bestand uploaden

U kunt nu uw IoT-hub zodat de mogelijkheid om te configureren [bestanden uploaden naar de IoT-hub](iot-hub-devguide-file-upload.md) met behulp van de details van uw opslagaccount.

De configuratie moet de volgende waarden:

* **Storage-container**: een blob-container in Azure storage-account in uw huidige Azure-abonnement koppelen aan uw IoT-hub. U hebt de benodigde opslag-accountgegevens in de voorgaande sectie hebt opgehaald. IoT Hub wordt automatisch gegenereerd SAS URI's met schrijfmachtigingen voor deze blob-container voor apparaten moet worden gebruikt bij het uploaden van bestanden.

* **Meldingen over geüploade bestanden ontvangen**: bestand uploaden meldingen in of uit.

* **SAS TTL**: deze instelling wordt de time-to-live van de SAS-URI's op het apparaat wordt geretourneerd door de IoT Hub. Standaard ingesteld op één uur.

* **Melding instellingen standaard TTL-bestand**: de time-to-live van een bestand uploaden melding voordat deze is verlopen. Standaard ingesteld op één dag.

* **Maximumaantal leveringen voor melding bestand**: het aantal keren dat de IoT-Hub wil ervoor zorgen dat een bestand uploaden een melding. Standaard ingesteld op 10.

Gebruik de volgende Azure CLI-opdrachten voor het configureren van instellingen voor het uploaden van uw IoT-hub:

<!--Robinsh this is out of date, add cloud powershell -->

Gebruik in een bash-shell:

```azurecli
az iot hub update --name {your iot hub name} \
  --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"

az iot hub update --name {your iot hub name} \
  --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"

az iot hub update --name {your iot hub name} \
  --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} \
  --set properties.enableFileUploadNotifications=true

az iot hub update --name {your iot hub name} \
  --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10

az iot hub update --name {your iot hub name} \
  --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

U kunt de configuratie van de uploaden bestand bekijken op uw IoT-hub met behulp van de volgende opdracht uit:

```azurecli
az iot hub show --name {your iot hub name}
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
