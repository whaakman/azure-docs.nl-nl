---
title: Archiveren van diagnostische logboeken in Azure
description: Leer hoe u uw diagnostische logboeken van Azure voor langdurige bewaarperioden in een opslagaccount archiveren.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: johnkem
ms.component: logs
ms.openlocfilehash: ab7689cb18dcd11926abc2e9806885b1072e3886
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50960171"
---
# <a name="archive-azure-diagnostic-logs"></a>Archiveren van diagnostische logboeken in Azure

In dit artikel laten we zien hoe u de Azure portal, PowerShell-Cmdlets, CLI of REST-API gebruiken kunt om te archiveren uw [diagnostische logboeken in Azure](monitoring-overview-of-diagnostic-logs.md) in een storage-account. Deze optie is handig als u wilt behouden van de diagnostische logboeken met een optionele bewaarbeleid voor controle-, statische analysis- of back-up. Het storage-account heeft geen zich in hetzelfde abonnement als de resource dat Logboeken verzendt, zolang de gebruiker die de instelling configureert de juiste RBAC-toegang voor beide abonnementen heeft.

> [!WARNING]
> De indeling van de logboekgegevens in het opslagaccount wordt op 1 november 2018 gewijzigd in JSON Lines. [Raadpleeg dit artikel voor een beschrijving van de gevolgen en hoe u uw tooling kunt bijwerken om de nieuwe indeling te verwerken. ](./monitor-diagnostic-logs-append-blobs.md) 
>
> 

## <a name="prerequisites"></a>Vereisten

Voordat u begint, moet u [een opslagaccount maken](../storage/common/storage-create-storage-account.md) waarop uw logboeken met diagnostische gegevens kunnen worden gearchiveerd. Het is raadzaam dat u gebruik niet een bestaand opslagaccount met andere, niet-bewaking gegevens opgeslagen in het zodat u toegang tot bewakingsgegevens beter kunt beheren. Echter, als u ook van uw activiteitenlogboek en diagnostische gegevens naar een opslagaccount archiveren, het wellicht verstandig dat opslagaccount voor uw logboeken met diagnostische gegevens ook gebruiken om te houden van alle gegevens op een centrale locatie.

> [!NOTE]
>  U kan momenteel niet archiveren gegevens aan een storage-account die achter een beveiligd virtueel netwerk.

## <a name="diagnostic-settings"></a>Diagnostische instellingen

Als u wilt archiveren van uw logboeken met diagnostische gegevens met behulp van een van de onderstaande methoden, stelt u een **diagnostische instelling** voor een bepaalde resource. Een diagnostische instelling voor een resource definieert de categorieën van Logboeken en metrische gegevens die worden verzonden naar een bestemming (storage-account, Event Hubs-naamruimte of Log Analytics). Het definieert ook de (aantal dagen wilt behouden) van het bewaarbeleid voor gebeurtenissen van elke logboekcategorie en metrische gegevens die zijn opgeslagen in een storage-account. Als een bewaarbeleid is ingesteld op nul, worden gebeurtenissen voor die categorie logboekbestanden opgeslagen voor onbepaalde tijd (dat wil zeggen, permanent). Een bewaarbeleid kan anders zijn voor een willekeurig aantal dagen tussen 1 en 2147483647. [U kunt meer lezen over de diagnostische instellingen hier](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings). Bewaarbeleid zijn toegewezen per dag, dus aan het einde van een dag (UTC), logboeken van de dag dat nu is buiten de bewaarperiode van beleid wordt verwijderd. Bijvoorbeeld, als u een beleid voor het bewaren van één dag had, worden aan het begin van de dag vandaag nog de logboeken van de dag voor gisteren vernietigd. De verwijderbewerking begint bij middernacht UTC, maar houd er rekening mee dat het kan tot 24 uur duren voor de logboeken worden verwijderd uit uw storage-account. 

> [!NOTE]
> Het verzenden van multidimensionale metrische gegevens via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Een voorbeeld*: de meetwaarde 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Wanneer de waarde wordt geëxporteerd via diagnostische instellingen, wordt deze echter voorgesteld als alle binnenkomende berichten voor alle wachtrijen in de Event Hub.
>
>

## <a name="archive-diagnostic-logs-using-the-portal"></a>De diagnostische logboeken archiveren met behulp van de portal

1. In de portal, gaat u naar Azure Monitor en klikt u op **diagnostische instellingen**

    ![Sectie van Azure Monitor bewaking](media/monitoring-archive-diagnostic-logs/diagnostic-settings-blade.png)

2. (Optioneel) de lijst met door de resourcegroep of resourcetype filteren en klik vervolgens op de resource waarvoor u wilt een diagnostische instelling instellen.

3. Als er geen instellingen zijn op de resource hebt u geselecteerd, wordt u gevraagd om een instelling te maken. Klik op "Diagnostische gegevens inschakelen."

   ![Diagnostische instelling - er zijn geen bestaande instellingen toevoegen](media/monitoring-archive-diagnostic-logs/diagnostic-settings-none.png)

   Als er bestaande instellingen op de resource, ziet u een lijst met instellingen die al zijn geconfigureerd voor deze resource. Klik op 'Diagnostische instelling toevoegen'.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. Geef een naam van uw instelling en schakel het selectievakje voor **exporteren naar Opslagaccount**, selecteer vervolgens een storage-account. (Optioneel) Stel een aantal dagen te bewaren van deze logboeken met behulp van de **bewaarperiode (dagen)** schuifregelaars. Een bewaarperiode van nul dagen worden de logboeken voor onbepaalde tijd opgeslagen.

   ![Diagnostische instelling - bestaande instellingen toevoegen](media/monitoring-archive-diagnostic-logs/diagnostic-settings-configure.png)

4. Klik op **Opslaan**.

Na enkele ogenblikken wordt de nieuwe instelling wordt weergegeven in de lijst met instellingen voor deze resource en logboeken met diagnostische gegevens naar dit opslagaccount worden gearchiveerd zodra de gegevens van een nieuwe gebeurtenis wordt gegenereerd.

## <a name="archive-diagnostic-logs-via-azure-powershell"></a>Diagnostische logboeken archiveren via Azure PowerShell

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Eigenschap | Vereist | Beschrijving |
| --- | --- | --- |
| ResourceId |Ja |Resource-ID van de resource waarop u wilt een diagnostische instelling instellen. |
| StorageAccountId |Nee |Resource-ID van het Opslagaccount waarin logboeken met diagnostische gegevens moeten worden opgeslagen. |
| Categorieën |Nee |Door komma's gescheiden lijst met logboekcategorieën om in te schakelen. |
| Ingeschakeld |Ja |Booleaanse waarde die aangeeft of de diagnostische gegevens zijn ingeschakeld of uitgeschakeld op deze resource. |
| RetentionEnabled |Nee |Booleaanse waarde waarmee wordt aangegeven of een bewaarbeleid zijn ingeschakeld voor deze resource. |
| RetentionInDays |Nee |Het aantal dagen waarvoor gebeurtenissen moeten worden bewaard tussen 1 en 2147483647. De logboeken worden voor onbepaalde tijd opgeslagen door de waarde nul. |

## <a name="archive-diagnostic-logs-via-the-azure-cli"></a>Diagnostische logboeken archiveren via de Azure CLI

```azurecli
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

U kunt extra categorieën toevoegen aan de diagnostische logboeken van woordenlijsten toe te voegen aan de JSON-matrix die is doorgegeven als de `--logs` parameter.

De `--resource-group` argument is alleen vereist als `--storage-account` is niet een object-ID. Zie voor de volledige documentatie voor het archiveren van diagnostische logboeken naar de opslag, de [CLI-opdrachten](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create).

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Diagnostische logboeken archiveren via de REST-API

[Raadpleeg dit document](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings) voor meer informatie over hoe u een diagnostische instelling met de Azure Monitor REST API kunt instellen.

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Schema van logboeken met diagnostische gegevens in de storage-account

Nadat u hebt ingesteld gearchiveerd, wordt een opslagcontainer wordt gemaakt in de storage-account als een gebeurtenis optreedt in een van de logboekcategorieën die u hebt ingeschakeld. De blobs in de container Volg dezelfde naamconventie voor activiteitenlogboeken en logboeken met diagnostische gegevens, zoals hier wordt geïllustreerd:

```
insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Bijvoorbeeld, kan een blobnaam zijn:

```
insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Elke PT1H.json-blob bevat een JSON-blob van gebeurtenissen die hebben plaatsgevonden binnen het uur dat is opgegeven in de blob-URL (bijvoorbeeld, h=12). Tijdens het huidige uur worden gebeurtenissen toegevoegd aan het bestand PT1H.json wanneer deze zich voordoen. De minuutwaarde (m = 00) is altijd 00 omdat logboekgebeurtenissen voor diagnostische zijn onderverdeeld in afzonderlijke blobs per uur.

Elke gebeurtenis wordt in het bestand PT1H.json opgeslagen in de matrix "records", volgen deze indeling:

``` JSON
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

| De naam van element | Beschrijving |
| --- | --- |
| tijd |Tijdstip waarop de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag die de gebeurtenis overeenkomt. |
| resourceId |Resource-ID van de betrokken resource. |
| operationName |Naam van de bewerking. |
| category |De categorie van de logboekbestanden van de gebeurtenis. |
| properties |Instellen van `<Key, Value>` paren (dat wil zeggen woordenboek) met een beschrijving van de details van de gebeurtenis. |

> [!NOTE]
> De eigenschappen en het gebruik van deze eigenschappen kunnen variëren afhankelijk van de resource.

## <a name="next-steps"></a>Volgende stappen

* [Downloaden van blobs voor analyse](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Diagnostische logboeken van de Stream naar een Event Hubs-naamruimte](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Logboeken van Azure Active Directory met Azure Monitor archiveren](../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)
* [Lees meer over de diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)
