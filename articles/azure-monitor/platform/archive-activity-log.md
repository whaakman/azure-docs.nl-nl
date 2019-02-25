---
title: De Azure-activiteitenlogboek archiveren
description: Archiveer uw activiteitenlogboek van Azure voor langdurige bewaarperioden in een storage-account.
author: nkiest
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: nikiest
ms.subservice: logs
ms.openlocfilehash: f02b17ff4e83c3300973c86f26db76ebff5a8d0a
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56750885"
---
# <a name="archive-the-azure-activity-log"></a>De Azure-activiteitenlogboek archiveren
In dit artikel laten we zien hoe u de Azure portal, PowerShell-Cmdlets of platformoverschrijdende CLI gebruiken kunt om te archiveren uw [ **Azure Activity Log** ](../../azure-monitor/platform/activity-logs-overview.md) in een storage-account. Deze optie is handig als u wilt behouden het activiteitenlogboek is langer dan 90 dagen (met volledige controle over het bewaarbeleid) voor de controle-, statische analysis- of back-up opgenomen. Als u hoeft alleen de gebeurtenissen worden bewaard gedurende 90 dagen of minder u niet hoeft om in te stellen archiveren naar een opslagaccount, omdat gebeurtenissen in activiteitenlogboeken worden bewaard in de Azure-platform voor 90 dagen zonder in te schakelen archivering.

> [!WARNING]
> De indeling van de logboekgegevens in het opslagaccount wordt op 1 november 2018 gewijzigd in JSON Lines. [Raadpleeg dit artikel voor een beschrijving van de gevolgen en hoe u uw tooling kunt bijwerken om de nieuwe indeling te verwerken. ](./../../azure-monitor/platform/diagnostic-logs-append-blobs.md) 
>
> 

## <a name="prerequisites"></a>Vereisten
Voordat u begint, moet u [een opslagaccount maken](../../storage/common/storage-quickstart-create-account.md) waarop kunt u uw activiteitenlogboek archiveren. Het is raadzaam dat u gebruik niet een bestaand opslagaccount met andere, niet-bewaking gegevens opgeslagen in het zodat u toegang tot bewakingsgegevens beter kunt beheren. Echter, als u ook diagnostische logboeken en metrische gegevens naar een opslagaccount archiveren weet, het wellicht verstandig dat opslagaccount voor uw activiteitenlogboek ook gebruiken om te houden van alle gegevens op een centrale locatie. Het storage-account heeft geen zich in hetzelfde abonnement bevinden als het abonnement dat Logboeken verzendt, zolang de gebruiker die de instelling configureert de juiste RBAC-toegang voor beide abonnementen heeft.

## <a name="log-profile"></a>Logboekprofiel
Als u wilt archiveren het activiteitenlogboek via een van de onderstaande methoden, stelt u de **Logboekprofiel** voor een abonnement. Het Logboekprofiel bepaalt het type gebeurtenissen die zijn opgeslagen of gestreamd en de uitvoer-storage-account en/of event hub. Het definieert ook de (aantal dagen wilt behouden) van het bewaarbeleid voor gebeurtenissen die zijn opgeslagen in een storage-account. Als het bewaarbeleid is ingesteld op nul, worden gebeurtenissen voor onbepaalde tijd opgeslagen. Dit kan anders worden ingesteld op een waarde tussen 1 en 365. Bewaarbeleid zijn toegewezen per dag, dus aan het einde van een dag (UTC), logboeken van de dag dat nu is buiten de bewaarperiode van beleid wordt verwijderd. Bijvoorbeeld, als u een beleid voor het bewaren van één dag had, worden aan het begin van de dag vandaag nog de logboeken van de dag voor gisteren vernietigd. De verwijderbewerking begint bij middernacht UTC, maar houd er rekening mee dat het kan tot 24 uur duren voor de logboeken worden verwijderd uit uw storage-account. [U kunt meer lezen over log profielen hier](../../azure-monitor/platform/activity-logs-overview.md#export-the-activity-log-with-a-log-profile). 

## <a name="archive-the-activity-log-using-the-portal"></a>Met behulp van de portal activiteitenlogboek archiveren
1. Klik in de portal op de **activiteitenlogboek** koppeling op de navigatiebalk aan de linkerkant. Als u een koppeling voor het activiteitenlogboek niet ziet, klikt u op de **alle Services** eerst koppelen.
   
    ![Navigeer naar de blade met activiteitenlogboek](media/archive-activity-log/activity-logs-portal-navigate-v2.png)
2. Aan de bovenkant van de blade, klikt u op **exporteren naar Event Hub**.
   
    ![Klik op de knop exporteren](media/archive-activity-log/activity-logs-portal-export-v2.png)
3. In de blade die wordt weergegeven, schakel het selectievakje voor **exporteren naar een opslagaccount** en selecteer een opslagaccount.
   
    ![Een storage-account instellen](media/archive-activity-log/act-log-portal-export-blade.png)
4. Met behulp van de schuifregelaar of tekstvak, definieert u een aantal dagen (0-365) voor die activiteit logboekgebeurtenissen moeten worden opgeslagen in uw storage-account. Als u liever uw gegevens persistent gemaakt in de storage-account voor onbepaalde tijd, stelt u dit aantal naar nul. Als u moet het nummer van meer dan 365 dagen invoeren, gebruikt u de onderstaande methoden van PowerShell of CLI.
5. Klik op **Opslaan**.

## <a name="archive-the-activity-log-via-powershell"></a>Archiveren van het activiteitenlogboek via PowerShell

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzureRmLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your storage account belongs to>"
   $storageAccountName = "<your storage account name>"

   # Build the storage account Id from the settings above
   $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

   Add-AzureRmLogProfile -Name $logProfileName -Location $locations -StorageAccountId $storageAccountId
   ```

| Eigenschap | Vereist | Description |
| --- | --- | --- |
| StorageAccountId |Ja |Resource-ID van het Opslagaccount waarin u de activiteitenlogboeken worden opgeslagen. |
| Locaties |Ja |Door komma's gescheiden lijst met regio's waarvoor u wilt verzamelen van gebeurtenissen in activiteitenlogboeken. U vindt een lijst van alle regio's voor uw abonnement met `(Get-AzureRmLocation).Location`. |
| RetentionInDays |Nee |Het aantal dagen voor welke gebeurtenissen worden bewaard, tussen 1 en 2147483647. Een waarde van nul wordt de logboeken voor onbepaalde tijd opgeslagen (permanent). |
| Categorieën |Nee |Door komma's gescheiden lijst met categorieën van gebeurtenissen die moeten worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en actie.  Als niet is opgegeven, klikt u vervolgens alle mogelijke waarden wordt aangenomen dat |

## <a name="archive-the-activity-log-via-cli"></a>Archiveren van het activiteitenlogboek via CLI

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --storage-account-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>"
   ```

| Eigenschap | Vereist | Description |
| --- | --- | --- |
| naam |Ja |Naam van uw logboekprofiel. |
| storage-account-id |Ja |Resource-ID van het Opslagaccount waarin u de activiteitenlogboeken worden opgeslagen. |
| locaties |Ja |Spaties gescheiden lijst met regio's waarvoor u wilt verzamelen van gebeurtenissen in activiteitenlogboeken. U vindt een lijst van alle regio's voor uw abonnement met `az account list-locations --query [].name`. |
| dagen |Ja |Het aantal dagen voor welke gebeurtenissen worden bewaard, tussen 1 en 2147483647. De waarde nul worden de logboeken voor onbepaalde tijd opgeslagen (permanent).  Als nul is, klikt u vervolgens de ingeschakelde parameter moet worden ingesteld op true. |
|ingeschakeld | Ja |Waar of ONWAAR.  Gebruikt voor het in- of uitschakelen van het bewaarbeleid.  Indien waar, moet de parameter dagen een waarde die groter is dan 0 zijn.
| categorieën |Ja |Spaties gescheiden lijst met categorieën van gebeurtenissen die moeten worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en actie. |

## <a name="storage-schema-of-the-activity-log"></a>Schema van de opslag van het activiteitenlogboek
Nadat u hebt ingesteld gearchiveerd, wordt een storage-container gemaakt in de storage-account als een gebeurtenis in het activiteitenlogboek. De blobs in de container Volg dezelfde naamconventie voor activiteitenlogboeken en logboeken met diagnostische gegevens, zoals hier wordt geïllustreerd:

```
insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{subscription ID}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Bijvoorbeeld, kan een blobnaam zijn:

```
insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Elke PT1H.json-blob bevat een JSON-blob van gebeurtenissen die hebben plaatsgevonden binnen het uur dat is opgegeven in de blob-URL (bijvoorbeeld h = 12). Tijdens het huidige uur worden gebeurtenissen toegevoegd aan het bestand PT1H.json wanneer deze zich voordoen. De minuutwaarde (m = 00) is altijd 00 omdat logboekgebeurtenissen activiteit zijn onderverdeeld in afzonderlijke blobs per uur.

Elke gebeurtenis wordt in het bestand PT1H.json opgeslagen in de matrix "records", volgen deze indeling:

``` JSON
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


| De naam van element | Description |
| --- | --- |
| time |Tijdstip waarop de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag die de gebeurtenis overeenkomt. |
| resourceId |Resource-ID van de betrokken resource. |
| operationName |Naam van de bewerking. |
| category |Categorie van de actie, bijvoorbeeld. Schrijven, lezen, actie. |
| resultType |Het type van het resultaat, bijvoorbeeld. Success, Failure, Start |
| resultSignature |Afhankelijk van het resourcetype. |
| durationMs |Duur van de bewerking in milliseconden |
| callerIpAddress |IP-adres van de gebruiker die de bewerking, UPN-claim of op basis van beschikbaarheid SPN-claim heeft uitgevoerd. |
| correlationId |Meestal een GUID in de indeling van de verbindingsreeks. Gebeurtenissen die delen van een correlationId behoren tot dezelfde uber actie. |
| identity |JSON-blob met een beschrijving van de autorisatie en claims. |
| Autorisatie |De BLOB van RBAC-eigenschappen van de gebeurtenis. Omvat gewoonlijk de "action", 'rol' en 'bereik'-Eigenschappen. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', "Error", 'Waarschuwing', "Informatief" en "Uitgebreide" |
| locatie |De regio in de locatie is opgetreden (of globale). |
| eigenschappen |Instellen van `<Key, Value>` paren (dat wil zeggen woordenboek) met een beschrijving van de details van de gebeurtenis. |

> [!NOTE]
> De eigenschappen en het gebruik van deze eigenschappen kunnen variëren afhankelijk van de resource.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Downloaden van blobs voor analyse](../../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Stream het activiteitenlogboek naar Eventhubs](../../azure-monitor/platform/activity-logs-stream-event-hubs.md)
* [Meer informatie over het activiteitenlogboek](../../azure-monitor/platform/activity-logs-overview.md)


