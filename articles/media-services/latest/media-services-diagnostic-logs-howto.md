---
title: Bewaken van diagnostische logboeken van de Media Services via Azure Monitor | Microsoft Docs
description: In dit artikel laat zien hoe routeren en diagnostische logboeken via Azure Monitor weer te geven.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: 233b043ffdc295fe94ed2e3ba837d4229848df22
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795840"
---
# <a name="monitor-media-services-diagnostic-logs"></a>Diagnostische logboeken van mediaservices controleren

[Azure Monitor](../../azure-monitor/overview.md) kunt u metrische gegevens controleren en tot diagnostische logboeken waarmee u inzicht in hoe uw toepassingen worden uitgevoerd. Voor een gedetailleerde beschrijving van deze functie en om te zien waarom u wilt met behulp van Logboeken voor het metrische en diagnostische gegevens van Azure Media Services, Zie [Media-Services controleren metrische gegevens en logboeken met diagnostische gegevens](media-services-metrics-diagnostic-logs.md).

In dit artikel wordt beschreven hoe u gegevens doorsturen naar het opslagaccount en bekijk vervolgens de gegevens. 

## <a name="prerequisites"></a>Vereisten

- [Een Azure Media Services-account maken](create-account-cli-how-to.md).
- Beoordeling [Media-Services controleren metrische gegevens en logboeken met diagnostische gegevens](media-services-metrics-diagnostic-logs.md).

## <a name="route-data-to-the-storage-account-using-the-portal"></a>Gegevens routeren naar het opslagaccount met behulp van de portal

1. Meld u aan bij Azure Portal op https://portal.azure.com.
1. Navigeer naar uw Media Services-account in en klikt u op **diagnostische instellingen** onder **Monitor**. Hier kunt u een lijst zien van alle resources in uw abonnement die bewakingsgegevens via Azure Monitor genereren. 

    ![Sectie Diagnostische instellingen](media/media-services-diagnostic-logs/logs01.png)

1. Klik op **diagnostische instelling toevoegen**.

   De diagnostische instelling van een resource is een definitie van *welke* bewakingsgegevens moeten worden doorgestuurd vanuit een bepaalde resource en *waar* die bewakingsgegevens naartoe moeten worden gestuurd.

1. Geef in de weergegeven sectie de instelling een **naam** en schakel het selectievakje in voor **Archiveren naar een opslagaccount**.

    Selecteer het opslagaccount waarnaar u wilt verzenden van Logboeken en drukt u op **OK**.
1. Schakel alle selectievakjes onder **Logboek** en **Metrische gegevens** in. Afhankelijk van het resourcetype is er mogelijk maar één van deze opties beschikbaar. Met deze selectievakjes bepaalt u welke categorieën beschikbare logboekgegevens en metrische gegevens voor een bepaald resourcetype worden verzonden naar de bestemming die u hebt geselecteerd (in dit geval: een opslagaccount).

   ![Sectie Diagnostische instellingen](media/media-services-diagnostic-logs/logs02.png)
1. Stel de schuifregelaar **Bewaarperiode (dagen)** in op 30. Met deze schuifregelaar stelt u in hoeveel dagen u de bewakingsgegevens wilt bewaren in het opslagaccount. Gegevens die ouder zijn dan het aantal opgegeven dagen, worden automatisch verwijderd. Bij een bewaarperiode van nul dagen worden de gegevens voor onbepaalde tijd opgeslagen.
1. Klik op **Opslaan**.

Bewakingsgegevens uit uw resource worden nu doorgestuurd naar het opslagaccount.

## <a name="route-data-to-the-storage-account-using-the-cli"></a>Gegevens routeren naar het opslagaccount met behulp van de CLI

Om in te schakelen opslag van logboeken met diagnostische gegevens in een Storage-Account, moet u het volgende uitvoeren `az monitor diagnostic-settings` CLI-opdracht: 

```cli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <name or ID of storage account> \
    --resource <target resource object ID> \
    --resource-group <storage account resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

Bijvoorbeeld:

```cli
az monitor diagnostic-settings create --name amsv3diagnostic \
    --storage-account storageaccountforams  \
    --resource "/subscriptions/00000000-0000-0000-0000-0000000000/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount" \
    --resource-group "amsResourceGroup" \
    --logs '[{"category": "KeyDeliveryRequests",  "enabled": true, "retentionPolicy": {"days": 3, "enabled": true }}]'
```

## <a name="view-data-in-the-storage-account-using-the-portal"></a>Gegevens weergeven in de storage-account met behulp van de portal

Als u de voorgaande stappen hebt gevolgd, worden gegevens nu doorgestuurd naar uw opslagaccount.

Wellicht moet u vijf minuten wachten voordat de gebeurtenis in het opslagaccount wordt weergegeven.

1. Navigeer in de portal naar de sectie **Opslagaccounts** vanuit de linker navigatiebalk.
1. Identificeer het opslagaccount dat u in de voorgaande sectie hebt gemaakt en klik erop.
1. Klik op **Blobs**, klikt u vervolgens op de container met het label **insights-logs-keydeliveryrequests**. Dit is de container met de logboeken. Bewakingsgegevens worden onderverdeeld in containers per resource-ID, klikt u vervolgens op datum en tijd.
1. Ga naar het bestand PT1H.json door te klikken in de containers voor resource-id, datum en tijd. Klik op het bestand PT1H.json en op **Downloaden**.

 U kunt nu de JSON-gebeurtenis zien die in het opslagaccount werd opgeslagen.

### <a name="examples-of-pt1hjson"></a>Voorbeelden van PT1H.json

#### <a name="clear-key-delivery-log"></a>Schakel sleutellevering log

```json
{
  "time": "2019-05-21T00:07:33.2820450Z",
  "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000000/RESOURCEGROUPS/amsResourceGroup/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/AMSACCOUNT",
  "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
  "operationVersion": "1.0",
  "category": "KeyDeliveryRequests",
  "resultType": "Succeeded",
  "resultSignature": "OK",
  "durationMs": 253,
  "identity": {
    "authorization": {
      "issuer": "myIssuer",
      "audience": "myAudience"
    },
    "claims": {
      "urn:microsoft:azure:mediaservices:contentkeyidentifier": "e4276e1d-c012-40b1-80d0-ac15808b9277",
      "nbf": "1558396914",
      "exp": "1558400814",
      "iss": "myIssuer",
      "aud": "myAudience"
    }
  },
  "level": "Informational",
  "location": "westus2",
  "properties": {
    "requestId": "fb5c2b3a-bffa-4434-9c6f-73d689649add",
    "keyType": "Clear",
    "keyId": "e4276e1d-c012-40b1-80d0-ac15808b9277",
    "policyName": "SharedContentKeyPolicyUsedByAllAssets",
    "tokenType": "JWT",
    "statusMessage": "OK"
  }
}
```

#### <a name="widevine-encrypted-key-delivery-log"></a>Widevine versleutelde sleutellevering log

```json
{
  "time": "2019-05-20T23:15:22.7088747Z",
  "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000000/RESOURCEGROUPS/amsResourceGroup/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/AMSACCOUNT",
  "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
  "operationVersion": "1.0",
  "category": "KeyDeliveryRequests",
  "resultType": "Succeeded",
  "resultSignature": "OK",
  "durationMs": 69,
  "identity": {
    "authorization": {
      "issuer": "myIssuer",
      "audience": "myAudience"
    },
    "claims": {
      "urn:microsoft:azure:mediaservices:contentkeyidentifier": "0092d23a-0a42-4c5f-838e-6d1bbc6346f8",
      "nbf": "1558392430",
      "exp": "1558396330",
      "iss": "myIssuer",
      "aud": "myAudience"
    }
  },
  "level": "Informational",
  "location": "westus2",
  "properties": {
    "requestId": "49613dd2-16aa-4595-a6e0-4e68beae6d37",
    "keyType": "Widevine",
    "keyId": "0092d23a-0a42-4c5f-838e-6d1bbc6346f8",
    "policyName": "DRMContentKeyPolicy",
    "tokenType": "JWT",
    "statusMessage": "OK"
  }
}
```

## <a name="see-also"></a>Zie ook

* [Azure Monitor Metrics](../../azure-monitor/platform/data-platform.md)
* [Azure Monitor diagnostische logboeken](../../azure-monitor/platform/diagnostic-logs-overview.md)
* [Het verzamelen en gebruiken van logboekgegevens van uw Azure-resources](../../azure-monitor/platform/diagnostic-logs-overview.md)

## <a name="next-steps"></a>Volgende stappen

[Monitor voor metrische gegevens](media-services-metrics-howto.md)
