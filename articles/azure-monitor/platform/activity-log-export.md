---
title: De Azure-activiteitenlogboek exporteren
description: Azure-activiteitenlogboek exporteren naar de opslag voor archivering of Azure Eventhubs voor het exporteren van buiten Azure.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: acf2526e79519e610614dc5217efbfe5e327b90f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66248142"
---
# <a name="export-azure-activity-log-to-storage-or-azure-event-hubs"></a>Azure-activiteitenlogboek exporteren naar storage of Azure Event Hubs
De [Azure Activity Log](activity-logs-overview.md) biedt inzicht in gebeurtenissen op abonnementsniveau die hebben plaatsgevonden in uw Azure-abonnement. Naast het activiteitenlogboek weergeven in Azure portal of kopieert naar een Log Analytics-werkruimte waar ze kan worden geanalyseerd met andere gegevens die door Azure Monitor worden verzameld, kunt u een logboekprofiel archiveer het activiteitenlogboek in een Azure storage-account of stream het naar een  Event Hub.

## <a name="archive-activity-log"></a>Het activiteitenlogboek archiveren
Het activiteitenlogboek naar een opslagaccount archiveren is handig als u wilt uw logboekgegevens die langer is dan 90 dagen (met volledige controle over het bewaarbeleid) voor de controle-, statische analysis- of back-up behouden. Als u hoeft alleen de gebeurtenissen worden bewaard gedurende 90 dagen of minder u niet hoeft om in te stellen archiveren naar een opslagaccount, omdat gebeurtenissen in activiteitenlogboeken gedurende 90 dagen worden bewaard in de Azure-platform.

## <a name="stream-activity-log-to-event-hub"></a>Stream-activiteitenlogboek naar Event Hub
[Azure Event Hubs](/azure/event-hubs/) is een platform en een gebeurtenisopneemservice die kan worden ontvangen streamen van gegevens en miljoenen gebeurtenissen per seconde verwerken. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen door gebruik te maken van een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Er zijn twee manieren waarop u kunt de streaming-mogelijkheden voor het activiteitenlogboek gebruiken:
* **Stream naar de logboeken en telemetrie systemen van derden**: Na verloop van tijd, Azure Event Hubs streamen, worden het mechanisme voor het doorgeven van het activiteitenlogboek is opgenomen in de siem's van derden en meld u analytics-oplossingen.
* **Een aangepaste Telemetrie-en logboekregistratie platform**: Als u al beschikken over een platform op maat gemaakte telemetrie of na te denken over het bouwen van een zijn, het zeer schaalbare publish-subscribe aard van Event Hubs kunt u het activiteitenlogboek flexibel opnemen. 

## <a name="prerequisites"></a>Vereisten

### <a name="storage-account"></a>Storage-account
Als u uw activiteitenlogboek archiveren bent, moet u [een opslagaccount maken](../../storage/common/storage-quickstart-create-account.md) als u er nog geen hebt. Niet moet u een bestaand opslagaccount met andere, niet-bewaking gegevens opgeslagen in het zodat u toegang tot bewakingsgegevens beter kunt beheren. Als u ook diagnostische logboeken en metrische gegevens naar een opslagaccount archiveren zijn, maar kunt u besluiten dat hetzelfde opslagaccount gebruiken om te houden van alle gegevens op een centrale locatie.

Het storage-account heeft geen zich in hetzelfde abonnement bevinden als het abonnement dat Logboeken verzendt, zolang de gebruiker die de instelling configureert de juiste RBAC-toegang voor beide abonnementen heeft.
> [!NOTE]
>  U gegevens naar een opslagaccount die zich achter een beveiligd virtueel netwerk kan momenteel niet archiveren.

### <a name="event-hubs"></a>Event Hubs
Als u uw activiteitenlogboek naar een event hub verzendt, dan u moet [maken van een event hub](../../event-hubs/event-hubs-create.md) als u er nog geen hebt. Als u gestreamd eerder activiteitenlogboek gebeurtenissen naar deze Event Hubs-naamruimte, wordt die event hub worden hergebruikt.

Het beleid voor gedeelde toegang definieert de machtigingen die het mechanisme voor streaming is. Streamen naar Event Hubs vereist machtigingen beheren, verzenden en luisteren. U kunt maken of wijzigen van beleid voor gedeelde toegang voor de Event Hubs-naamruimte in Azure portal onder het tabblad configureren voor uw Event Hubs-naamruimte.

Voor het bijwerken van het activiteitenlogboek logboekprofiel om op te nemen streaming, moet u de machtiging ListKey hebben op deze Event Hubs-autorisatieregel. De Event Hubs-naamruimte is niet nodig om zich in hetzelfde abonnement bevinden als het abonnement dat wordt dat Logboeken verzendt, zolang de gebruiker die de instelling configureert de juiste RBAC heeft toegang tot beide abonnementen en beide abonnementen in de dezelfde AAD-tenant zijn.

Het activiteitenlogboek naar een Event Hub door Stream [het maken van een Logboekprofiel](#create-a-log-profile).

## <a name="create-a-log-profile"></a>Maken van een logboekprofiel
U definieert hoe uw Azure-activiteitenlogboek wordt geëxporteerd met behulp van een **logboekprofiel**. Elk Azure-abonnement kan slechts één logboekprofiel hebben. Deze instellingen kunnen worden geconfigureerd via de **exporteren** optie in de blade met activiteitenlogboek in de portal. Ze kunnen ook programmatisch te worden geconfigureerd [met behulp van de Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShell-cmdlets of CLI.

Het logboekprofiel definieert de volgende gegevens.

**Waar het activiteitenlogboek moeten worden verzonden.** De beschikbare opties zijn momenteel Opslagaccount of Event Hubs.

**Welke gebeurteniscategorieën moeten worden verzonden.** De betekenis van *categorie* in Logboekprofielen en het activiteitenlogboek gebeurtenissen verschilt. In het profiel Log *categorie* vertegenwoordigt het bewerkingstype (schrijven, verwijderen, actie). In het geval van een activiteitenlogboek, de *categorie*"* eigenschap vertegenwoordigt de bron- of type gebeurtenis (bijvoorbeeld, beheer, ServiceHealth en waarschuwing).

**Welke regio's (locaties) moeten worden geëxporteerd.** Omdat veel gebeurtenissen in het activiteitenlogboek algemene gebeurtenissen zijn, moet u alle locaties opnemen.

**Hoe lang het activiteitenlogboek worden bewaard in een Storage-Account.** Een bewaarperiode van nul dagen betekent dat Logboeken altijd worden bewaard. De waarde kan anders een willekeurig aantal dagen tussen 1 en 2147483647 zijn.

Als het bewaarbeleid worden ingesteld, maar het opslaan van Logboeken in een storage-account is uitgeschakeld, hebben voor het bewaren van geen effect. Bewaarbeleid zijn toegepast per dag, dus aan het einde van een dag (UTC), logboeken van de dag dat nu is buiten de bewaarperiode van beleid worden verwijderd. Bijvoorbeeld, als u een beleid voor het bewaren van één dag had, worden aan het begin van de dag vandaag nog de logboeken van de dag voor gisteren vernietigd. De verwijderbewerking begint bij middernacht UTC, maar houd er rekening mee dat het kan tot 24 uur duren voor de logboeken worden verwijderd uit uw storage-account.



> [!WARNING]
> De indeling van de logboekgegevens in de storage-account gewijzigd in JSON-regels op 1 november 2018. [Raadpleeg dit artikel voor een beschrijving van de gevolgen en hoe u uw tooling kunt bijwerken om de nieuwe indeling te verwerken. ](diagnostic-logs-append-blobs.md)
>
>

### <a name="create-log-profile-using-the-azure-portal"></a>Logboekprofiel maken met de Azure-portal

Maken of bewerken van een logboekprofiel met de **exporteren naar Event Hub** optie in Azure portal.

1. Uit de **Monitor** in het menu in de Azure portal, selecteer **exporteren naar Event Hub**.

    ![De knop exporteren in de portal](media/activity-log-export/portal-export.png)

3. In de blade die wordt weergegeven, geeft u het volgende:
   * De regio's met de gebeurtenissen om te exporteren. U moet alle regio's om ervoor te zorgen dat u belangrijke gebeurtenissen niet mist omdat het activiteitenlogboek een algemene (niet-regionaal)-logboek is en zodat de meeste gebeurtenissen niet in een regio die is gekoppeld aan deze hoeft selecteren. 
   * Als u wilt om te schrijven naar opslagaccount:
       * Het Opslagaccount waarnaar u wilt opslaan van gebeurtenissen.
       * het aantal dagen dat u wilt dat deze gebeurtenissen in de opslag worden bewaard. Een instelling van 0 dagen worden de logboeken altijd behouden.
   * Als u wilt om te schrijven naar event hub:
       * de Service Bus Namespace waarin u een Event Hub moet worden gemaakt voor het streamen van deze gebeurtenissen wilt.

     ![Blade met activiteitenlogboek exporteren](./media/activity-logs-overview/activity-logs-portal-export-blade.png)


4. Klik op **opslaan** deze instellingen op te slaan. De instellingen worden onmiddellijk toegepast op uw abonnement.


### <a name="configure-log-profile-using-powershell"></a>Logboekprofiel met behulp van PowerShell configureren

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Als een logboekprofiel al bestaat, moet u eerst de bestaande logboekprofiel verwijderen en een nieuwe maken.

1. Gebruik `Get-AzLogProfile` om te identificeren als een logboekprofiel bestaat.  Als een logboekprofiel bestaat, let dan op de *naam* eigenschap.

1. Gebruik `Remove-AzLogProfile` te verwijderen van het logboekprofiel met de waarde van de *naam* eigenschap.

    ```powershell
    # For example, if the log profile name is 'default'
    Remove-AzLogProfile -Name "default"
    ```

3. Gebruik `Add-AzLogProfile` om een nieuwe logboekprofiel te maken:

    ```powershell
    Add-AzLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Location global,westus,eastus -RetentionInDays 90 -Category Write,Delete,Action
    ```

    | Eigenschap | Vereist | Beschrijving |
    | --- | --- | --- |
    | Name |Ja |Naam van uw logboekprofiel. |
    | StorageAccountId |Nee |Resource-ID van het Opslagaccount waarin het activiteitenlogboek moet worden opgeslagen. |
    | serviceBusRuleId |Nee |Service Bus-regel-ID voor de Service Bus-naamruimte hebt gemaakt in eventhubs hebben. Dit is een tekenreeks met de indeling: `{service bus resource ID}/authorizationrules/{key name}`. |
    | Locatie |Ja |Door komma's gescheiden lijst met regio's waarvoor u wilt verzamelen van gebeurtenissen in activiteitenlogboeken. |
    | RetentionInDays |Ja |Aantal dagen waarvoor gebeurtenissen moeten worden bewaard in de storage-account, tussen 1 en 2147483647. De logboeken worden voor onbepaalde tijd opgeslagen door de waarde nul. |
    | Category |Nee |Door komma's gescheiden lijst met categorieën van gebeurtenissen die moeten worden verzameld. Mogelijke waarden zijn _schrijven_, _verwijderen_, en _actie_. |

### <a name="example-script"></a>Voorbeeldscript
Hieronder volgt een voorbeeld PowerShell-script voor het maken van een logboekprofiel die het activiteitenlogboek voor zowel opslag en de event hub schrijft.

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your event hub belongs to>"
   $eventHubNamespace = "<event hub namespace>"

   # Build the service bus rule Id from the settings above
   $serviceBusRuleId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.EventHub/namespaces/$eventHubNamespace/authorizationrules/RootManageSharedAccessKey"

   # Build the storage account Id from the settings above
   $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

   Add-AzLogProfile -Name $logProfileName -Location $locations -ServiceBusRuleId $serviceBusRuleId
   ```


### <a name="configure-log-profile-using-azure-cli"></a>Logboekprofiel met behulp van Azure CLI configureren

Als een logboekprofiel al bestaat, moet u eerst de bestaande logboekprofiel verwijderen en maak vervolgens een nieuwe logboekprofiel.

1. Gebruik `az monitor log-profiles list` om te identificeren als een logboekprofiel bestaat.
2. Gebruik `az monitor log-profiles delete --name "<log profile name>` te verwijderen van het logboekprofiel met de waarde van de *naam* eigenschap.
3. Gebruik `az monitor log-profiles create` om een nieuwe logboekprofiel te maken:

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --service-bus-rule-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUB NAME SPACE>/authorizationrules/RootManageSharedAccessKey"
   ```

    | Eigenschap | Vereist | Description |
    | --- | --- | --- |
    | name |Ja |Naam van uw logboekprofiel. |
    | storage-account-id |Ja |Resource-ID van het Opslagaccount waarin u de activiteitenlogboeken worden opgeslagen. |
    | locations |Ja |Spaties gescheiden lijst met regio's waarvoor u wilt verzamelen van gebeurtenissen in activiteitenlogboeken. U vindt een lijst van alle regio's voor uw abonnement met `az account list-locations --query [].name`. |
    | days |Ja |Het aantal dagen voor welke gebeurtenissen worden bewaard, tussen 1 en 365. De waarde nul worden de logboeken voor onbepaalde tijd opgeslagen (permanent).  Als nul is, klikt u vervolgens de ingeschakelde parameter moet worden ingesteld op true. |
    |enabled | Ja |Waar of ONWAAR.  Gebruikt voor het in- of uitschakelen van het bewaarbeleid.  Indien waar, moet de parameter dagen een waarde die groter is dan 0 zijn.
    | categorieën |Ja |Spaties gescheiden lijst met categorieën van gebeurtenissen die moeten worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en actie. |



## <a name="activity-log-schema"></a>Schema voor het logboek van activiteit
Of naar Azure storage of Event Hub verzonden, worden de gegevens van een activiteitenlogboek worden weggeschreven naar JSON met de volgende indeling.

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
De elementen in deze JSON worden beschreven in de volgende tabel.

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
| authorization |De BLOB van RBAC-eigenschappen van de gebeurtenis. Omvat gewoonlijk de "action", 'rol' en 'bereik'-Eigenschappen. |
| niveau |Niveau van de gebeurtenis. Een van de volgende waarden: _Kritieke_, _fout_, _waarschuwing_, _informatief_, en _uitgebreide_ |
| location |De regio in de locatie is opgetreden (of globale). |
| properties |Instellen van `<Key, Value>` paren (dat wil zeggen woordenboek) met een beschrijving van de details van de gebeurtenis. |

> [!NOTE]
> De eigenschappen en het gebruik van deze eigenschappen kunnen variëren afhankelijk van de resource.



## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het activiteitenlogboek](../../azure-resource-manager/resource-group-audit.md)
* [Activiteitenlogboek te verzamelen en onderbrengen in Logboeken van Azure Monitor](activity-log-collect.md)
