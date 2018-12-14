---
title: Overzicht van de Azure-activiteitenlogboek
description: Ontdek wat de Azure-activiteitenlogboek er en hoe u deze kunt gebruiken om te begrijpen van gebeurtenissen die plaatsvinden binnen uw Azure-abonnement.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: johnkem
ms.component: activitylog
ms.openlocfilehash: 9b2566458bc32f9b1d7a36790fbdd5fbb2419ec1
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384289"
---
# <a name="monitor-subscription-activity-with-the-azure-activity-log"></a>Abonnement-activiteit controleren met de Azure-activiteitenlogboek

De **Azure Activity Log** is een abonnementlogboek die biedt inzicht in gebeurtenissen op abonnementsniveau die hebben plaatsgevonden in Azure. Dit omvat een scala aan gegevens van operationele gegevens van de Azure Resource Manager-updates op Service Health-gebeurtenissen. Het activiteitenlogboek was voorheen bekend als 'Audit Logs' of "Operationele Logboeken," sinds de beheercategorie rapporten controlelaag gebeurtenissen voor uw abonnementen. Met het activiteitenlogboek, kunt u bepalen de ' wat, wie, en wanneer ' voor (PUT, POST, DELETE schrijfbewerkingen) die wordt gemaakt op de resources in uw abonnement. U kunt ook de status van de bewerking en andere relevante eigenschappen begrijpen. Het activiteitenlogboek bevat geen lees (GET)-bewerkingen of bewerkingen voor resources die gebruikmaken van het klassieke / 'RDFE'-model.

![Activiteiten logboeken en andere typen logboeken ](./media/monitoring-overview-activity-logs/Activity_Log_vs_other_logs_v5.png)

Afbeelding 1: Activiteiten logboeken en andere typen logboeken

Het activiteitenlogboek wijkt af van [diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md). Activiteitenlogboeken bevatten gegevens over de bewerkingen op een resource van buitenaf (de "controlelaag"). Logboeken met diagnostische gegevens worden gegenereerd door een resource en geef informatie op over de werking van die resource (de "gegevenslaag').

> [!WARNING]
> De Azure-activiteitenlogboek is voornamelijk bedoeld voor activiteiten die in Azure Resource Manager plaatsvinden. Resources met behulp van het model Klassiek/RDFE worden niet bijgehouden. Sommige resourcetypen klassiek hebben een proxy-resourceprovider in Azure Resource Manager (bijvoorbeeld Microsoft.ClassicCompute). Als u met een resourcetype klassiek via Azure Resource Manager met behulp van deze proxy-resourceproviders werken, is de bewerkingen worden weergegeven in het activiteitenlogboek. Als u met een resourcetype klassiek buiten de Azure Resource Manager-proxy's communiceert, zijn alleen uw acties in het logboek geregistreerd. Het logboek kan worden gebladerd in een apart gedeelte van de portal.
>
>

U kunt gebeurtenissen ophalen uit het activiteitenlogboek is opgenomen met Azure portal, CLI, PowerShell-cmdlets en REST-API van Azure Monitor.

> [!NOTE]
> [De nieuwere waarschuwingen](../azure-monitor/platform/alerts-overview.md) bieden een betere ervaring wanneer het maken en beheren van de activiteit zich waarschuwingsregels.  [Meer informatie](../azure-monitor/platform/alerts-activity-log.md).


## <a name="categories-in-the-activity-log"></a>Categorieën in het activiteitenlogboek
Het activiteitenlogboek bevat verschillende categorieën van gegevens. Voor volledige informatie over de schema's van deze categorieën [Raadpleeg dit artikel](../azure-monitor/platform/activity-log-schema.md). Deze omvatten:
* **Administratieve** -deze categorie bevat de record van alle maken, bijwerken, verwijderen en actie bewerkingen uitgevoerd via Resource Manager. Voorbeelden van de typen gebeurtenissen u in deze categorie ziet zijn 'virtuele machine maken' en 'netwerkbeveiligingsgroep verwijderen' elke actie op die door een gebruiker of een toepassing met behulp van Resource Manager is gemodelleerd als een bewerking op een bepaald resourcetype. Als het bewerkingstype schrijven, verwijderen of actie is, worden de records van de begin- en het slagen of mislukken van deze bewerking worden opgenomen in de beheercategorie. De beheercategorie omvat ook eventuele wijzigingen in de op rollen gebaseerd toegangsbeheer in een abonnement.
* **Servicestatus** -deze categorie bevat de record van de service health incidenten die hebben plaatsgevonden in Azure. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "SQL Azure in VS-Oost ondervindt uitvaltijd." Er zijn vijf soorten service health-gebeurtenissen: Actie vereist, telefonische herstel, Incident, onderhoud, informatie of beveiliging, en wordt alleen weergegeven als u een resource in het abonnement dat wordt beïnvloed door de gebeurtenis.
* **Resource Health** -deze categorie bevat de record van een resource health-gebeurtenissen die hebben plaatsgevonden naar uw Azure-resources. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "Virtuele Machine health-status gewijzigd in niet beschikbaar." Resource health-gebeurtenissen kunnen gelden voor een van de vier health-statussen: Beschikbaar, niet beschikbaar is, verminderde en onbekend. Resource health-gebeurtenissen kunnen ook worden gecategoriseerd als Platform die gebruiker geïnitieerd of.
* **Waarschuwing** -deze categorie bevat de record van alle activeringen van de Azure-waarschuwingen. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "CPU-percentage op myVM is meer dan 80 voor de afgelopen vijf minuten." Een reeks systemen die Azure hebben een waarschuwingen concept--u kunt een regel voor een definiëren en een melding ontvangen wanneer er voorwaarden overeenkomen met die regel. Telkens wanneer een ondersteunde Azure Waarschuwingstype 'wordt geactiveerd,' of de voorwaarden wordt voldaan voor het genereren van een melding, een record van de activering wordt ook gepusht naar deze categorie van het activiteitenlogboek.
* **Automatisch schalen** -deze categorie bevat de record van alle gebeurtenissen die betrekking hebben op de werking van de engine voor automatisch schalen op basis van alle instellingen voor automatisch schalen die u hebt gedefinieerd in uw abonnement. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "Schalen via automatisch schalen van de actie is mislukt". Automatisch schalen gebruik, u kunt automatisch omhoog of omlaag schalen op basis van tijd van de dag en/of load (metrische) gegevens met behulp van een instelling voor automatisch schalen van het aantal exemplaren in een ondersteunde resourcetype. Wanneer de voorwaarden wordt voldaan aan omhoog of omlaag schalen, de begin- en geslaagde of mislukte gebeurtenissen worden opgenomen in deze categorie.
* **Aanbeveling** -deze categorie bevat aanbevelingsgebeurtenissen die van Azure Advisor.
* **Beveiliging** -deze categorie bevat de record van alle waarschuwingen die worden gegenereerd door Azure Security Center. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "verdacht bestand met dubbele extensie uitgevoerd."
* **Beleid** -deze categorie bevat niet alle gebeurtenissen; dit is gereserveerd voor toekomstig gebruik. 

## <a name="event-schema-per-category"></a>Gebeurtenisschema per categorie
[Zie dit artikel voor informatie over de gebeurtenisschema in het activiteitenlogboek per categorie.](../azure-monitor/platform/activity-log-schema.md)

## <a name="what-you-can-do-with-the-activity-log"></a>U kunt doen met het activiteitenlogboek
Hier volgen enkele dingen die u met het activiteitenlogboek kunt doen:

![Azure-activiteitenlogboek](./media/monitoring-overview-activity-logs/Activity_Log_Overview_v3.png)


* Vragen en weer te geven in de **Azure-portal**.
* [Een waarschuwing op een gebeurtenis voor activiteitenlogboek maken.](../azure-monitor/platform/activity-log-alerts.md)
* [Stream het naar een **Event Hub** ](monitoring-stream-activity-logs-event-hubs.md) voor opname van een service van derden of aangepaste analyseoplossing zoals Power BI.
* Analyseren in Power BI met behulp van de [ **Power BI-inhoudspakket**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
* [Opslaan naar een **Opslagaccount** voor archivering of handmatige inspectie](../azure-monitor/platform/archive-activity-log.md). U kunt opgeven de bewaartermijn (in dagen) met behulp van de **Logboekprofiel**.
* Deze opvragen via PowerShell-Cmdlet, CLI of REST-API.

## <a name="query-the-activity-log-in-the-azure-portal"></a>Query uitvoeren op het activiteitenlogboek in Azure portal
U kunt uw activiteitenlogboek weergeven op verschillende plaatsen binnen de Azure-portal:
* De **activiteitenlogboek** die toegankelijk is door te zoeken naar het activiteitenlogboek onder **alle services** in het navigatiedeelvenster links.
* **Monitor** standaard in het navigatiedeelvenster links wordt weergegeven. Het activiteitenlogboek is een gedeelte van Azure Monitor.
* De meeste **resources**, bijvoorbeeld de configuratieblade voor een virtuele Machine. Het activiteitenlogboek is een gedeelte van de meeste resourceblades en de gebeurtenissen die zijn gerelateerd aan die specifieke resource te klikken op het automatisch gefilterd.

U kunt het activiteitenlogboek is opgenomen door deze velden filteren in de Azure-portal:
* TimeSpan - de begin- en -tijd voor gebeurtenissen.
* Categorie - categorie van de gebeurtenis, zoals hierboven beschreven.
* Abonnement: een of meer namen van de Azure-abonnement.
* Resourcegroep: een of meer resourcegroepen binnen die abonnementen.
* Bron (naam) - de naam van een specifieke resource.
* Brontype - het type resource, bijvoorbeeld Microsoft.Compute/virtualmachines.
* Naam van bewerking: de naam van een Azure Resource Manager-bewerking, bijvoorbeeld Microsoft.SQL/servers/Write.
* Ernst - de ernst van de gebeurtenis (informatief, waarschuwing, fout, kritiek).
* Gebeurtenis gestart door - de 'beller' of de gebruiker die de bewerking heeft uitgevoerd.
* Open search - dit is een open tekstzoekvak waarin wordt gezocht die tekenreeks voor alle velden in alle gebeurtenissen.

Als u een set filters hebt gedefinieerd, kunt u een query vastmaken aan uw Azure-dashboard om altijd te controleren op specifieke gebeurtenissen.

Voor nog meer voeding, klikt u op de **logboeken** pictogram, waarin de gegevens van uw activiteitenlogboek in de [Log Analytics Activity Log Analytics-oplossing](../azure-monitor/platform/collect-activity-logs.md). De blade met activiteitenlogboek biedt een eenvoudige filter/browse-ervaring op Logboeken, maar Log Analytics kunt u draait, vragen en Visualiseer uw gegevens op een krachtigere manier.

## <a name="export-the-activity-log-with-a-log-profile"></a>Exporteren van het activiteitenlogboek met een Logboekprofiel
Een **Logboekprofiel** bepaalt hoe uw activiteitenlogboek wordt geëxporteerd. Met behulp van een Logboekprofiel, die u kunt configureren:

* Waar het activiteitenlogboek moeten worden verzonden (Opslagaccount of Event Hubs)
* Welke gebeurteniscategorieën (schrijven, verwijderen, actie) moeten worden verzonden. *De betekenis van 'categorie' in Logboekprofielen en gebeurtenissen in activiteitenlogboeken verschilt. In het Logboekprofiel vertegenwoordigt 'Categorie' het bewerkingstype (schrijven, verwijderen, actie). De eigenschap 'categorie' vertegenwoordigt in een activiteitenlogboek-gebeurtenis, de bron- of type gebeurtenis (bijvoorbeeld, beheer, ServiceHealth, waarschuwing, en meer).*
* Welke regio's (locaties) moeten worden geëxporteerd. Zorg ervoor dat 'global', omdat veel gebeurtenissen in het activiteitenlogboek algemene gebeurtenissen zijn.
* Hoe lang het activiteitenlogboek worden bewaard in een Storage-Account.
    - Een bewaarperiode van nul dagen betekent dat Logboeken altijd worden bewaard. De waarde kan anders een willekeurig aantal dagen tussen 1 en 2147483647 zijn.
    - Als Logboeken opslaan in een Storage-Account is uitgeschakeld (bijvoorbeeld, als er alleen Event Hubs of Log Analytics-opties zijn geselecteerd), bewaarbeleid worden ingesteld, maar hebben het bewaarbeleid geen effect.
    - Bewaarbeleid zijn toegepast per dag, dus aan het einde van een dag (UTC), logboeken van de dag dat nu is buiten de bewaarperiode van beleid worden verwijderd. Bijvoorbeeld, als u een beleid voor het bewaren van één dag had, worden aan het begin van de dag vandaag nog de logboeken van de dag voor gisteren vernietigd. De verwijderbewerking begint bij middernacht UTC, maar houd er rekening mee dat het kan tot 24 uur duren voor de logboeken worden verwijderd uit uw storage-account.

U kunt een storage-account of event hub-naamruimte die zich niet in hetzelfde abonnement als een dat Logboeken verzendt. De gebruiker die de instelling configureert, moet de juiste RBAC-toegang tot beide abonnementen hebben.

> [!NOTE]
>  U gegevens naar een opslagaccount die zich achter een beveiligd virtueel netwerk kan momenteel niet archiveren.

> [!WARNING]
> De indeling van de logboekgegevens in de storage-account gewijzigd in JSON-regels op 1 november 2018. [Raadpleeg dit artikel voor een beschrijving van de gevolgen en hoe u uw tooling kunt bijwerken om de nieuwe indeling te verwerken. ](./monitor-diagnostic-logs-append-blobs.md) 
>
> 

Deze instellingen kunnen worden geconfigureerd via de optie "Export" in de blade met activiteitenlogboek in de portal. Ze kunnen ook programmatisch te worden geconfigureerd [met behulp van de Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShell-cmdlets of CLI. Een abonnement kan slechts één logboekprofiel hebben.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Logboekprofielen met behulp van de Azure-portal configureren
U kunt het activiteitenlogboek naar een Event Hub streamen of sla ze op een Storage-Account met behulp van de optie 'Exporteren naar Event Hub' in de Azure-portal.

1. Navigeer naar **activiteitenlogboek** via het menu aan de linkerkant van de portal.

    ![Navigeer naar activiteitenlogboek in portal](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate-v2.png)
2. Klik op de **exporteren naar Event Hub** knop aan de bovenkant van de blade.

    ![De knop exporteren in de portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export-v2.png)
3. In de blade die wordt weergegeven, kunt u het volgende selecteren:  
  * regio's waarvoor u wilt exporteren van gebeurtenissen
  * het Opslagaccount waarnaar u wilt opslaan van gebeurtenissen
  * het aantal dagen dat u wilt dat deze gebeurtenissen in de opslag worden bewaard. Een instelling van 0 dagen worden de logboeken altijd behouden.
  * de Service Bus Namespace waarin u een Event Hub moet worden gemaakt voor het streamen van deze gebeurtenissen wilt.

     ![Blade met activiteitenlogboek exporteren](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klik op **opslaan** deze instellingen op te slaan. De instellingen worden onmiddellijk toegepast op uw abonnement.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Logboekprofielen met behulp van de Azure PowerShell-Cmdlets configureren

#### <a name="get-existing-log-profile"></a>Bestaand logboekprofiel ophalen

```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Toevoegen van een logboekprofiel

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Location global,westus,eastus -RetentionInDays 90 -Category Write,Delete,Action
```

| Eigenschap | Vereist | Description |
| --- | --- | --- |
| Name |Ja |Naam van uw logboekprofiel. |
| StorageAccountId |Nee |Resource-ID van het Opslagaccount waarnaar het activiteitenlogboek moeten worden opgeslagen. |
| serviceBusRuleId |Nee |Service Bus-regel-ID voor de Service Bus-naamruimte hebt gemaakt in eventhubs hebben. Een tekenreeks zijn met deze indeling is: `{service bus resource ID}/authorizationrules/{key name}`. |
| Locatie |Ja |Door komma's gescheiden lijst met regio's waarvoor u wilt verzamelen van gebeurtenissen in activiteitenlogboeken. |
| RetentionInDays |Ja |Het aantal dagen voor welke gebeurtenissen worden bewaard, tussen 1 en 2147483647. Een waarde van nul wordt de logboeken voor onbepaalde tijd opgeslagen (permanent). |
| Categorie |Nee |Door komma's gescheiden lijst met categorieën van gebeurtenissen die moeten worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en actie. |

#### <a name="remove-a-log-profile"></a>Een logboekprofiel verwijderen
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cli"></a>Logboekprofielen met de Azure CLI configureren

#### <a name="get-existing-log-profile"></a>Bestaand logboekprofiel ophalen

```azurecli
az monitor log-profiles list
az monitor log-profiles show --name <profile name>
```

De `name` eigenschap moet de naam van uw logboekprofiel.

#### <a name="add-a-log-profile"></a>Toevoegen van een logboekprofiel

```azurecli
az monitor log-profiles create --name <profile name> \
    --locations <location1 location2 ...> \
    --location <location> \
    --categories <category1 category2 ...>
```

Zie voor de volledige documentatie voor het maken van een monitorprofiel met de CLI, de [CLI-opdrachten](/cli/azure/monitor/log-profiles#az-monitor-log-profiles-create)

#### <a name="remove-a-log-profile"></a>Een logboekprofiel verwijderen

```azurecli
az monitor log-profiles delete --name <profile name>
```

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het activiteitenlogboek (voorheen controlelogboeken)](../azure-resource-manager/resource-group-audit.md)
* [Stream het Azure-activiteitenlogboek naar Eventhubs](monitoring-stream-activity-logs-event-hubs.md)
