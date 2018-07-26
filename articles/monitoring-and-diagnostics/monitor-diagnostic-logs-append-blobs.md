---
title: Voorbereiden voor de indeling wijzigen naar diagnostische logboeken van Azure Monitor
description: Diagnostische logboeken in Azure worden verplaatst naar het gebruik van toevoeg-blobs op 1 November 2018.
author: johnkemnetz
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: johnkem
ms.component: logs
ms.openlocfilehash: b83c67e5c2ca47e73c1743d8eeaea03a8d92ea1f
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247849"
---
# <a name="prepare-for-format-change-to-azure-monitor-diagnostic-logs-archived-to-a-storage-account"></a>Indeling wijzigen naar diagnostische logboeken van Azure Monitor gearchiveerd naar een opslagaccount voorbereiden

> [!WARNING]
> Als u verzendt [diagnostische logboeken van Azure-resource of metrische gegevens naar een opslagaccount met behulp van de instellingen voor resourcediagnose](./monitoring-archive-diagnostic-logs.md) of [activiteitenlogboeken streamen naar een opslagaccount door middel van logboekprofielen](./monitoring-archive-activity-log.md), de indeling van de gegevens in het storage-account wordt gewijzigd in JSON-regels op 1 november 2018. De onderstaande instructies beschrijven de impact en het bijwerken van uw hulpprogramma's voor het afhandelen van de nieuwe indeling. 
>
> 

## <a name="what-is-changing"></a>Wat wordt gewijzigd

Azure Monitor biedt een functie waarmee u voor het verzenden van diagnostische gegevens van resources en gegevens van een activiteitenlogboek in Azure storage-account, Event Hubs-naamruimte, of in Log Analytics. Teneinde een prestatieprobleem systeem op **1 November 2018 om 12:00 middernacht UTC** de indeling van het logboek gegevens verzenden naar de blob storage wordt gewijzigd. Als u het hulpprogramma-dat wil zeggen lezen van gegevens uit blob-opslag hebt, moet u uw hulpprogramma's voor meer informatie over de indeling van de nieuwe bijwerken.

* Donderdag, November 1 2018 om 12:00 middernacht UTC, de blob-indeling wordt gewijzigd, zodat worden [JSON regels](http://jsonlines.org/). Dit betekent dat elke record wordt worden gescheiden door een nieuwe regel, met geen matrix buitenste records en geen komma's tussen de JSON-records.
* De wijzigingen aan de indeling blob voor alle diagnostische instellingen voor alle abonnementen in één keer. Het eerste bestand PT1H.json dat is verzonden voor 1 November gebruikmaken van deze nieuwe indeling. Namen van de blob en container blijven hetzelfde.
* Wijzigen van een diagnostische instelling tussen nu en 1 November nog steeds het verzenden van gegevens in de huidige indeling tot 1 November.
* Deze wijziging wordt in één keer uitgevoerd in alle openbare cloud-regio's. De wijziging wordt in Azure China of Azure Duitsland Azure Government-clouds nog hervat.
* Deze wijziging heeft gevolgen voor de volgende gegevenstypen:
  * [Diagnostische logboeken van Azure-resource](./monitoring-archive-diagnostic-logs.md) ([lijst met resources hier Zie](./monitoring-diagnostic-logs-schema.md))
  * [Azure-resource metrische gegevens die worden geëxporteerd door de diagnostische instellingen](./monitoring-overview-of-diagnostic-logs.md#diagnostic-settings)
  * [Azure gegevens van een activiteitenlogboek wordt geëxporteerd met logboekprofielen](./monitoring-archive-activity-log.md)
* Deze wijziging heeft geen invloed op:
  * Netwerk-stroomlogboeken
  * Azure-servicelogboeken niet beschikbaar gesteld via Azure Monitor nog (bijvoorbeeld, diagnostische logboeken van Azure App Service, analyselogboeken voor storage)
  * Routering van diagnostische logboeken in Azure en activiteitenlogboeken streamen naar andere bestemmingen (Event Hubs, Log Analytics)

### <a name="how-to-see-if-you-are-impacted"></a>Hoe om te zien of u wordt beïnvloed

U wordt alleen beïnvloed door deze wijziging als u:
1. Logboekgegevens verzenden naar Azure storage-account met behulp van een diagnostische instelling van de resource en
2. Hulpprogramma's die afhankelijk zijn van de JSON-structuur van deze logboeken in de opslag hebben.
 
Om te identificeren als u instellingen voor resourcediagnose die gegevens naar een Azure storage-account verzenden hebt, kunt u navigeren naar de **Monitor** sectie van de portal, klik op **diagnostische instellingen**, en identificeren alle resources die zijn **diagnostische Status** ingesteld op **ingeschakeld**:

![Azure Monitor diagnostische instellingen-blade](./media/monitor-diagnostic-logs-append-blobs\portal-diag-settings.png)

Als diagnostische Status is ingesteld op ingeschakeld, hebt u een actieve diagnostische instelling voor die bron. Klik op de bron om te zien als alle instellingen voor diagnostische gegevens naar een opslagaccount verzenden:

![Storage-account is ingeschakeld](./media/monitor-diagnostic-logs-append-blobs\portal-storage-enabled.png)

Als u gegevens verzenden naar een opslagaccount met behulp van deze instellingen voor resourcediagnose resources hebt, wordt de indeling van de gegevens in dat opslagaccount worden beïnvloed door deze wijziging. Tenzij u aangepaste hulpprogramma's die de werking van deze opslagaccounts hebt, heeft de indeling wijziging geen gevolgen voor u.

### <a name="details-of-the-format-change"></a>Details van de indeling wijzigen

De huidige indeling van het bestand PT1H.json in Azure blob-opslag maakt gebruik van een JSON-matrix van records. Hier volgt een voorbeeld van een logbestand KeyVault nu:

```json
{
    "records": [
        {
            "time": "2016-01-05T01:32:01.2691226Z",
            "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
            "operationName": "VaultGet",
            "operationVersion": "2015-06-01",
            "category": "AuditEvent",
            "resultType": "Success",
            "resultSignature": "OK",
            "resultDescription": "",
            "durationMs": "78",
            "callerIpAddress": "104.40.82.76",
            "correlationId": "",
            "identity": {
                "claim": {
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com",
                    "appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"
                }
            },
            "properties": {
                "clientInfo": "azure-resource-manager/2.0",
                "requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01",
                "id": "https://contosokeyvault.vault.azure.net/",
                "httpStatusCode": 200
            }
        },
        {
            "time": "2016-01-05T01:33:56.5264523Z",
            "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
            "operationName": "VaultGet",
            "operationVersion": "2015-06-01",
            "category": "AuditEvent",
            "resultType": "Success",
            "resultSignature": "OK",
            "resultDescription": "",
            "durationMs": "83",
            "callerIpAddress": "104.40.82.76",
            "correlationId": "",
            "identity": {
                "claim": {
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com",
                    "appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"
                }
            },
            "properties": {
                "clientInfo": "azure-resource-manager/2.0",
                "requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01",
                "id": "https://contosokeyvault.vault.azure.net/",
                "httpStatusCode": 200
            }
        }
    ]
}
```

Maakt gebruik van de nieuwe indeling [JSON regels](http://jsonlines.org/), waarbij elke gebeurtenis een regel is en de nieuwe-regelteken geeft aan een nieuwe gebeurtenis dat. Dit is wat het bovenstaande voorbeeld ziet er als in het bestand PT1H.json na de wijziging:

```json
{"time": "2016-01-05T01:32:01.2691226Z","resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT","operationName": "VaultGet","operationVersion": "2015-06-01","category": "AuditEvent","resultType": "Success","resultSignature": "OK","resultDescription": "","durationMs": "78","callerIpAddress": "104.40.82.76","correlationId": "","identity": {"claim": {"http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com","appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},"properties": {"clientInfo": "azure-resource-manager/2.0","requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id": "https://contosokeyvault.vault.azure.net/","httpStatusCode": 200}}
{"time": "2016-01-05T01:33:56.5264523Z","resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT","operationName": "VaultGet","operationVersion": "2015-06-01","category": "AuditEvent","resultType": "Success","resultSignature": "OK","resultDescription": "","durationMs": "83","callerIpAddress": "104.40.82.76","correlationId": "","identity": {"claim": {"http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com","appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},"properties": {"clientInfo": "azure-resource-manager/2.0","requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id": "https://contosokeyvault.vault.azure.net/","httpStatusCode": 200}}
```

Deze nieuwe indeling kunt push-logboekbestanden met behulp van Azure Monitor [toevoeg-blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-append-blobs), die zijn efficiënter voor voortdurend nieuwe gebeurtenisgegevens toe te voegen.

## <a name="how-to-update"></a>Het bijwerken van

U hoeft alleen om updates te maken als u aangepaste hulpprogramma's die deze logboekbestanden voor verdere verwerking opnemen. Als u het gebruik van een externe log analytics of de SIEM-hulpprogramma, wordt aangeraden [met eventhubs voor opname van deze gegevens in plaats daarvan](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/). Event hubs-integratie is het gemakkelijker in termen van Logboeken van allerlei services verwerken en locatie in een bepaalde logboek bladwijzers.

Aangepaste hulpprogramma's moeten worden bijgewerkt voor het afhandelen van zowel de huidige indeling en de regels van de JSON-indeling die hierboven worden beschreven. Dit zorgt ervoor dat wanneer gegevens wordt gestart in de nieuwe indeling worden weergegeven, uw hulpprogramma's niet verbreken.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [diagnostische logboeken van de resource naar een opslagaccount archiveren](./monitoring-archive-diagnostic-logs.md)
* Meer informatie over [gegevens naar een opslagaccount van een activiteitenlogboek archiveren](./monitoring-archive-activity-log.md)
