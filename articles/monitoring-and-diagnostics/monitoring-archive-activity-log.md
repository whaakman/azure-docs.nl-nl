---
title: Archiveren van de Azure Activity Log | Microsoft Docs
description: Informatie over het archiveren van uw Azure Activity Log voor lange bewaartermijn in een opslagaccount.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: johnkem
ms.openlocfilehash: 0e3a5b84f57eac96249430fa1c2c4cc076c2926a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="archive-the-azure-activity-log"></a>Archiveren van de Azure Activity Log
In dit artikel, laten we zien hoe u de Azure-portal, PowerShell-Cmdlets of platformoverschrijdende CLI gebruiken kunt bij de archivering van uw [ **Azure Activity Log** ](monitoring-overview-activity-logs.md) in een opslagaccount. Deze optie is handig als u wilt behouden uw activiteitenlogboek van meer dan 90 dagen (met volledige controle over het bewaarbeleid) voor controle, statische analyses of back-up. Als u hoeft alleen uw gebeurtenissen worden bewaard gedurende 90 dagen of minder u niet hoeft instellen naar een opslagaccount archivering omdat activiteitenlogboek gebeurtenissen worden behouden in de Azure-platform gedurende 90 dagen zonder in te schakelen archivering.

## <a name="prerequisites"></a>Vereisten
Voordat u begint, moet u [een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) waarmee u uw activiteitenlogboek kunt archiveren. Het is raadzaam dat u niet een bestaand opslagaccount met andere, niet-bewaking gegevens die zijn opgeslagen in het gebruikt zodat u toegang tot bewakingsgegevens beter kunt beheren. Echter, als u ook van diagnostische logboeken en metrische gegevens naar een opslagaccount archiveren, kan zinvol zijn voor dit opslagaccount voor uw logboek ook gebruiken om alle bewakingsgegevens in een centrale locatie. Het opslagaccount dat u moet een algemeen opslagaccount, niet een blob storage-account. Het opslagaccount heeft geen zich in hetzelfde abonnement als het abonnement dat Logboeken, zolang de gebruiker die de instelling configureert juiste RBAC toegang tot beide abonnementen heeft.

## <a name="log-profile"></a>Logboek-profiel
Als u wilt archiveren het activiteitenlogboek met behulp van de methoden hieronder, u stelt de **logboek profiel** voor een abonnement. Het logboek-profiel definieert het type gebeurtenissen die zijn opgeslagen of gestreamd en de uitvoer-storage-account en/of event hub. Het definieert ook het bewaarbeleid (aantal dagen wilt bewaren) voor gebeurtenissen die zijn opgeslagen in een opslagaccount. Als het bewaarbeleid is ingesteld op nul, worden gebeurtenissen voor onbepaalde tijd opgeslagen. Dit kan anders worden ingesteld op een waarde tussen 1 en 2147483647. Bewaarbeleid zijn toegepaste per dag, aan het einde van een dag (UTC), logboeken van de dag dat nu is buiten de bewaarperiode beleid wordt dus verwijderd. Bijvoorbeeld, als u had een bewaarbeleid van één dag, zou aan het begin van vandaag de dag de logboeken van de dag voordat gisteren worden verwijderd. [U kunt meer lezen over log profielen hier](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile). 

## <a name="archive-the-activity-log-using-the-portal"></a>Archiveren van het activiteitenlogboek via de portal
1. Klik in de portal op de **activiteitenlogboek** koppeling op de navigatiebalk aan de linkerkant. Als u een koppeling voor het logboek niet ziet, klikt u op de **meer Services** eerst koppelen.
   
    ![Ga naar de blade Activity Log](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. Klik boven aan de blade op **exporteren**.
   
    ![Klik op de knop exporteren](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. Schakel het selectievakje voor de blade **exporteren naar een opslagaccount** en selecteer een opslagaccount.
   
    ![Een opslagaccount instellen](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. De schuifregelaar of in het tekstvak, definieert een aantal dagen waarvoor activiteitenlogboek gebeurtenissen moeten worden opgeslagen in uw opslagaccount. Als u liever uw gegevens op de voor onbepaalde tijd bewaard in de storage-account, dit nummer wordt ingesteld op nul.
5. Klik op **Opslaan**.

## <a name="archive-the-activity-log-via-powershell"></a>Het activiteitenlogboek via PowerShell archiveren
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Eigenschap | Vereist | Beschrijving |
| --- | --- | --- |
| StorageAccountId |Nee |Resource-ID van het Opslagaccount waarin activiteitenlogboeken moet worden opgeslagen. |
| Locaties |Ja |Door komma's gescheiden lijst met regio's waarvoor u wilt verzamelen van gebeurtenissen voor Activity Log. U kunt een lijst weergeven met alle regio's [via deze pagina](https://azure.microsoft.com/en-us/regions) of met behulp van [de Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| RetentionInDays |Ja |Aantal dagen voor welke gebeurtenissen worden bewaard, tussen 1 en 2147483647. Een waarde van nul wordt de logboeken voor onbepaalde tijd opgeslagen (permanent). |
| Categorieën |Ja |Door komma's gescheiden lijst met categorieën van gebeurtenissen die moeten worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en in te grijpen. |

## <a name="archive-the-activity-log-via-cli"></a>Het activiteitenlogboek via CLI archiveren
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Eigenschap | Vereist | Beschrijving |
| --- | --- | --- |
| naam |Ja |Naam van uw logboek-profiel. |
| storageId |Nee |Resource-ID van het Opslagaccount waarin activiteitenlogboeken moet worden opgeslagen. |
| Locaties |Ja |Door komma's gescheiden lijst met regio's waarvoor u wilt verzamelen van gebeurtenissen voor Activity Log. U kunt een lijst weergeven met alle regio's [via deze pagina](https://azure.microsoft.com/en-us/regions) of met behulp van [de Azure Management REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| RetentionInDays |Ja |Aantal dagen voor welke gebeurtenissen worden bewaard, tussen 1 en 2147483647. De waarde nul wordt de logboeken voor onbepaalde tijd worden opgeslagen (permanent). |
| Categorieën |Ja |Door komma's gescheiden lijst met categorieën van gebeurtenissen die moeten worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en in te grijpen. |

## <a name="storage-schema-of-the-activity-log"></a>Opslag-schema van het activiteitenlogboek
Nadat u hebt ingesteld archivering, wordt een opslagcontainer gemaakt in de storage-account zodra een activiteitenlogboek gebeurtenis plaatsvindt. De blobs in de container Volg dezelfde indeling voor de activiteitenlogboek- en diagnostische logboeken. De structuur van deze BLOB's is:

> Insights-operationele-logboeken/name = standaard/resourceId = / ABONNEMENTEN / {abonnements-ID} / y = {4-cijferige numerieke year} / m = {jaartallen met twee numerieke month} / d = {jaartallen met twee numerieke dag} /h = {jaartallen met twee 24-uurs klok hour}/m=00/PT1H.json
> 
> 

Zo mogelijk een blob-naam:

> Insights-Operational-Logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON
> 
> 

Elke blob PT1H.json bevat een JSON-blob van gebeurtenissen die hebben plaatsgevonden binnen het uur die is opgegeven in de blob-URL (bijvoorbeeld h = 12). Tijdens het huidige uur gebeurtenissen naar het bestand PT1H.json toegevoegd wanneer deze zich voordoen. De minuut waarde (m = 00) is altijd 00, omdat het activiteitenlogboek gebeurtenissen worden onderverdeeld in afzonderlijke blobs per uur.

Elke gebeurtenis wordt in het bestand PT1H.json opgeslagen in de matrix 'records', volgt deze indeling:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
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
| category |Categorie van de actie, bijvoorbeeld. Schrijven, lezen, in te grijpen. |
| resultType |Het type van het resultaat, bijv. Geslaagd, mislukt, Start |
| resultSignature |Afhankelijk van het brontype. |
| durationMs |Duur van de bewerking in milliseconden |
| callerIpAddress |IP-adres van de gebruiker die is uitgevoerd. de bewerking, UPN-claim of SPN claim op basis van beschikbaarheid. |
| correlationId |Meestal een GUID in de indeling van de tekenreeks. Gebeurtenissen die een correlationId share deel uitmaken van dezelfde uber actie. |
| identity |JSON-blob met een beschrijving van de autorisatie en claims. |
| Autorisatie |De BLOB van RBAC-eigenschappen van de gebeurtenis. Omvat gewoonlijk het eigenschappen "action", 'rol' en 'bereik'. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Ter informatie' en 'Verbose' |
| location |De regio in de locatie is opgetreden (of globale). |
| properties |Een set `<Key, Value>` paren (dat wil zeggen woordenboek) met een beschrijving van de details van de gebeurtenis. |

> [!NOTE]
> De eigenschappen en het gebruik van deze eigenschappen kunnen variëren afhankelijk van de resource.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Blobs voor analyse downloaden](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
* [Stream het activiteitenlogboek naar Event Hubs](monitoring-stream-activity-logs-event-hubs.md)
* [Lees meer over het activiteitenlogboek](monitoring-overview-activity-logs.md)

