---
title: Archiveren van Azure diagnostische logboeken | Microsoft Docs
description: Informatie over het archiveren van uw Azure diagnostische logboeken voor lange bewaartermijn in een opslagaccount.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 3a55c73f-2ef3-45f3-8956-bcf9c0cb7e05
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: dbc5f89001dcb6cd1ab061cb0a9632e4e5d2c1c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="archive-azure-diagnostic-logs"></a>Azure logboeken met diagnostische gegevens archiveren
In dit artikel, laten we zien hoe u de Azure-portal, PowerShell-Cmdlets, CLI of REST-API gebruiken kunt bij de archivering van uw [Azure diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md) in een opslagaccount. Deze optie is handig als u wilt behouden van de diagnostische logboeken met een optionele bewaarbeleid voor controle, statische analyses of back-up. Het opslagaccount heeft geen zich in hetzelfde abonnement als de bron logboeken verzenden zolang de gebruiker die de instelling configureert juiste RBAC toegang tot beide abonnementen heeft.

## <a name="prerequisites"></a>Vereisten
Voordat u begint, moet u [een opslagaccount maken](../storage/storage-create-storage-account.md) waarnaar uw logboeken met diagnostische gegevens kunnen worden gearchiveerd. Het is raadzaam dat u niet een bestaand opslagaccount met andere, niet-bewaking gegevens die zijn opgeslagen in het gebruikt zodat u toegang tot bewakingsgegevens beter kunt beheren. Echter, als u ook uw activiteitenlogboek en diagnostische metrische gegevens naar een opslagaccount archiveert, kan zinvol zijn voor dit opslagaccount voor uw logboeken met diagnostische gegevens ook gebruiken om alle bewakingsgegevens in een centrale locatie. Het opslagaccount dat u moet een algemeen opslagaccount, niet een blob storage-account.

## <a name="diagnostic-settings"></a>Diagnostische instellingen
Als u wilt archiveren uw logboeken met diagnostische gegevens met behulp van de methoden hieronder, u stelt een **diagnostische instelling** voor een bepaalde resource. Een diagnostische instelling voor een resource definieert de categorieën van Logboeken en metrische gegevens die worden verzonden naar een bestemming (storage-account, Event Hubs-naamruimte of Log Analytics). Het definieert ook het bewaarbeleid (aantal dagen wilt bewaren) voor gebeurtenissen van elke categorie logboek en metrische gegevens die zijn opgeslagen in een opslagaccount. Als een bewaarbeleid is ingesteld op nul, worden gebeurtenissen voor die categorie logboekbestanden opgeslagen voor onbepaalde tijd (dat wil zeggen, permanent). Een bewaarbeleid kan anders zijn voor een willekeurig aantal dagen tussen 1 en 2147483647. [U kunt meer lezen over de diagnostische instellingen hier](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings). Bewaarbeleid zijn toegepaste per dag, aan het einde van een dag (UTC), logboeken van de dag dat nu is buiten de bewaarperiode beleid wordt dus verwijderd. Bijvoorbeeld, als u had een bewaarbeleid van één dag, worden aan het begin van vandaag de dag de logboeken van de dag voordat gisteren verwijderd

## <a name="archive-diagnostic-logs-using-the-portal"></a>Archief diagnostische logboeken met behulp van de portal
1. In de portal, gaat u naar Azure Monitor en klikt u op **diagnostische instellingen**

    ![Sectie van de Monitor Azure bewaking](media/monitoring-archive-diagnostic-logs/diagnostic-settings-blade.png)

2. Optioneel de lijst filteren op resourcegroep of brontype en klik vervolgens op de bron die u wilt geen diagnostische instellen.

3. Als er geen instellingen bestaan op de bron voor hebt u geselecteerd, u wordt gevraagd om een instelling te maken. Klik op "Diagnostische gegevens inschakelen."

   ![Diagnostische instelling - geen bestaande instellingen toevoegen](media/monitoring-archive-diagnostic-logs/diagnostic-settings-none.png)

   Als er bestaande instellingen op de bron, ziet u een lijst met instellingen die al zijn geconfigureerd op deze resource. Klik op 'Diagnostische instelling toevoegen'.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. Geef een naam van de instelling en schakel het selectievakje voor **exporteren naar Opslagaccount**, selecteert u een opslagaccount. Eventueel, stel een aantal dagen wilt bewaren van deze logboeken met behulp van de **bewaartermijn (dagen)** schuifregelaars. Een bewaartermijn van nul dagen worden de logboeken voor onbepaalde tijd opgeslagen.
   
   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-archive-diagnostic-logs/diagnostic-settings-configure.png)
    
4. Klik op **Opslaan**.

De nieuwe instelling wordt weergegeven in de lijst met instellingen voor deze bron na enkele ogenblikken en logboeken met diagnostische gegevens naar die storage-account worden gearchiveerd zodra er nieuwe gebeurtenisgegevens wordt gegenereerd.

## <a name="archive-diagnostic-logs-via-azure-powershell"></a>Diagnostische logboeken archief via Azure PowerShell
```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Eigenschap | Vereist | Beschrijving |
| --- | --- | --- |
| ResourceId |Ja |Bron-ID van de resource waarop u wilt geen diagnostische instellen. |
| StorageAccountId |Nee |Resource-ID van het Opslagaccount waarin u de logboeken met diagnostische gegevens worden opgeslagen. |
| Categorieën |Nee |Door komma's gescheiden lijst met categorieën logboek om in te schakelen. |
| Ingeschakeld |Ja |Een Boolean die aangeeft of diagnostische gegevens zijn ingeschakeld of uitgeschakeld op deze resource. |
| RetentionEnabled |Nee |Een Boolean die aangeeft of een bewaarbeleid zijn ingeschakeld op deze resource. |
| RetentionInDays |Nee |Aantal dagen waarvoor gebeurtenissen moeten worden gehandhaafd tussen 1 en 2147483647. De waarde nul wordt de logboeken voor onbepaalde tijd opgeslagen. |

## <a name="archive-diagnostic-logs-via-the-cross-platform-cli"></a>Diagnostische logboeken archief via de platformoverschrijdende CLI
```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Eigenschap | Vereist | Beschrijving |
| --- | --- | --- |
| resourceId |Ja |Bron-ID van de resource waarop u wilt geen diagnostische instellen. |
| storageId |Nee |Resource-ID van het Opslagaccount waarin u de logboeken met diagnostische gegevens worden opgeslagen. |
| Categorieën |Nee |Door komma's gescheiden lijst met categorieën logboek om in te schakelen. |
| ingeschakeld |Ja |Een Boolean die aangeeft of diagnostische gegevens zijn ingeschakeld of uitgeschakeld op deze resource. |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Diagnostische logboeken archief via de REST-API
[Dit document](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) voor meer informatie over hoe u een diagnostische instelling met de REST-API van Azure Monitor kunt instellen.

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Schema van diagnostische logboeken in de storage-account
Nadat u hebt ingesteld archivering, wordt een storage-container gemaakt in de storage-account zodra een gebeurtenis optreedt in een van de logboek-categorieën die u hebt ingeschakeld. De blobs in de container Volg dezelfde indeling over diagnostische logboeken en het activiteitenlogboek. De structuur van deze BLOB's is:

> Insights - logs-{categorie logboeknaam} / resourceId = / ABONNEMENTEN / {abonnements-ID} /RESOURCEGROUPS/ {Resourcegroepnaam} /PROVIDERS/ {resource provider name} / {brontype} / {resourcenaam} / y = {4-cijferige numerieke year} / m = {jaartallen met twee numerieke month} / d = {jaartallen met twee numerieke dag} /h = {jaartallen met twee 24-uurs klok hour}/m=00/PT1H.json
> 
> 

Of eenvoudigweg

> Insights - logs-{categorie logboeknaam} / resourceId = / {resource Id} / y = {4-cijferige numerieke year} / m = {jaartallen met twee numerieke month} / d = {jaartallen met twee numerieke dag} /h = {jaartallen met twee 24-uurs klok hour}/m=00/PT1H.json
> 
> 

Zo mogelijk een blob-naam:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
> 
> 

Elke blob PT1H.json bevat een JSON-blob van gebeurtenissen die hebben plaatsgevonden binnen het uur die is opgegeven in de blob-URL (bijvoorbeeld, h = 12). Tijdens het huidige uur gebeurtenissen naar het bestand PT1H.json toegevoegd wanneer deze zich voordoen. De minuut waarde (m = 00) is altijd 00, aangezien diagnostische gebeurtenissen worden onderverdeeld in afzonderlijke blobs per uur.

Elke gebeurtenis wordt in het bestand PT1H.json opgeslagen in de matrix 'records', volgt deze indeling:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Elementnaam | Beschrijving |
| --- | --- |
| tijd |Tijdstempel wanneer de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag de gebeurtenis overeenkomt. |
| resourceId |Bron-ID van de betrokken resource. |
| operationName |De naam van de bewerking. |
| category |De categorie van het logboek van de gebeurtenis. |
| properties |Een set `<Key, Value>` paren (dat wil zeggen woordenboek) met een beschrijving van de details van de gebeurtenis. |

> [!NOTE]
> De eigenschappen en het gebruik van deze eigenschappen kunnen variëren afhankelijk van de resource.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Blobs voor analyse downloaden](../storage/storage-dotnet-how-to-use-blobs.md)
* [Diagnostische logboeken van de stream naar een Event Hubs-naamruimte](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Lees meer over de logboeken met diagnostische gegevens](monitoring-overview-of-diagnostic-logs.md)
