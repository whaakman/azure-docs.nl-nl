---
title: Overzicht van de Azure Activity Log | Microsoft Docs
description: Meer informatie over wat het Azure Activity Log is en hoe u deze kunt gebruiken om te begrijpen gebeurtenissen binnen uw Azure-abonnement.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c274782f-039d-4c28-9ddb-f89ce21052c7
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: johnkem
ms.openlocfilehash: a101039b59eb1a4a3bcac25162c7f6373283e1b6
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="monitor-subscription-activity-with-the-azure-activity-log"></a>Monitor abonnement activiteit met de Azure Activity Log
De **Azure Activity Log** is een abonnementlogboek die biedt inzicht in het abonnement op gebeurtenissen die hebben plaatsgevonden in Azure. Dit omvat een bereik van gegevens van operationele gegevens van de Azure Resource Manager-updates op Service Health-gebeurtenissen. Het activiteitenlogboek heette vroeger 'Controlelogboeken' of 'Operationele Logs' sinds de beheercategorie rapporten besturingselement vlak gebeurtenissen voor uw abonnementen. Met het activiteitenlogboek, kunt u bepalen de ' wat, wie, en wanneer ' voor een (PUT, POST, verwijderen schrijfbewerkingen) die zijn gemaakt op de resources in uw abonnement. U kunt ook de status van de bewerking en andere relevante eigenschappen begrijpen. Het activiteitenlogboek bevat geen leesbewerkingen (GET) en bewerkingen voor resources die gebruikmaken van het klassieke / 'RDFE' model.

![Activiteit logboeken tegenover andere typen logboeken ](./media/monitoring-overview-activity-logs/Activity_Log_vs_other_logs_v5.png)

Afbeelding 1: Activiteitenlogboeken tegenover andere typen logboeken

Het activiteitenlogboek verschilt van [diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md). Activiteitenlogboeken bevatten gegevens over de bewerkingen van een resource van buitenaf (de ' besturingselement-vlak'). Logboeken met diagnostische gegevens worden gegenereerd door een resource en bieden informatie over de werking van de bron (de "data-vlak').

U kunt gebeurtenissen ophalen uit uw activiteitenlogboek met de Azure portal, CLI, PowerShell-cmdlets en REST-API van Azure-Monitor.


> [!WARNING]
> De Azure Activity Log is voornamelijk bedoeld voor activiteiten die in Azure Resource Manager optreden. Resources met behulp van het klassieke/RDFE-model worden niet bijgehouden. Sommige klassieke resource-typen hebben een proxy-resourceprovider in Azure Resource Manager (bijvoorbeeld Microsoft.ClassicCompute). Als u met een resourcetype klassieke via Azure Resource Manager met behulp van deze proxy-resourceproviders werken, is de bewerkingen worden weergegeven in het gebeurtenissenlogboek. Als u met een klassiek brontype buiten de Azure Resource Manager-proxy's communiceren, worden alleen uw acties in het logboek geregistreerd. Het logboek kan worden gebladerd in een apart gedeelte van de portal.
>
>

Bekijk de volgende video introductie van het activiteitenlogboek.
> [!VIDEO https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz/player]
> 
>

## <a name="categories-in-the-activity-log"></a>Categorieën in het gebeurtenissenlogboek
Het activiteitenlogboek bevat verschillende categorieën van gegevens. Voor volledige informatie over de schema's uit deze categorieën [Raadpleeg dit artikel](monitoring-activity-log-schema.md). Deze omvatten:
* **Beheerdersrechten** -deze categorie bevat de record van alle maken, update, delete en actie bewerkingen uitgevoerd via Resource Manager. Voorbeelden van welke typen gebeurtenissen u in deze categorie ziet zijn 'virtuele machine maken' en 'netwerkbeveiligingsgroep verwijderen' elke actie op die door een gebruiker of toepassing die met Resource Manager is gemodelleerd als een bewerking op een bepaald resourcetype. Als het bewerkingstype schrijven, verwijderen of actie is, worden de records van de begin- en het slagen of mislukken van die bewerking vastgelegd in de beheercategorie. De beheercategorie omvat ook eventuele wijzigingen aan rollen gebaseerd toegangsbeheer in een abonnement.
* **Servicestatus** -deze categorie bevat de record van de service health incidenten die hebben plaatsgevonden in Azure. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "SQL Azure in VS-Oost ondervindt uitvaltijd." Gebeurtenissen van de health service in vijf soorten komen: actie vereist, ondersteunde herstel, Incident, onderhoud, gegevens of beveiliging, en wordt alleen weergegeven als er een resource in het abonnement dat zou worden beïnvloed door de gebeurtenis.
* **Waarschuwing** -deze categorie bevat de record van alle activeringen van waarschuwingen van Azure. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "CPU-percentage op myVM is meer dan 80 voor de afgelopen vijf minuten." Een verscheidenheid aan Azure systemen hebben een waarschuwingsmethoden concept--u kunt een regel bepaalde hardwaresleutel definiëren en een melding ontvangen wanneer voorwaarden overeenkomen met die regel. Elke keer dat een ondersteunde Azure Waarschuwingstype 'wordt geactiveerd,' of de voorwaarden wordt voldaan voor het genereren van een melding, een record van de activering is ook naar deze categorie van het activiteitenlogboek gepusht.
* **Automatisch schalen** -deze categorie bevat de record van alle gebeurtenissen met betrekking tot de werking van de engine voor het automatisch schalen op basis van de instellingen voor automatisch schalen die u hebt gedefinieerd in uw abonnement. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is "Automatisch schalen opschaling van de actie is mislukt." Met automatisch schalen, kunt u automatisch geschaald uitbreiden of schalen op basis van tijd van de dag en/of laden (metrische) gegevens met behulp van een instelling voor automatisch schalen van het aantal exemplaren in een ondersteunde brontype. Wanneer de voorwaarden wordt voldaan aan schaal omhoog of omlaag, de begin- en geslaagd of mislukt gebeurtenissen worden vastgelegd in deze categorie.
* **Aanbeveling** -deze categorie bevat gebeurtenissen die aanbeveling van bepaalde brontypen, zoals websites en SQL-servers. Deze gebeurtenissen bieden aanbevelingen voor hoe u beter gebruikmaken van uw resources. U ontvangt alleen gebeurtenissen van dit type als u hebt resources die aanbevelingen verzenden.
* **Beveiliging** -deze categorie bevat de record van alle waarschuwingen die door Azure Security Center. Een voorbeeld van het type gebeurtenis u in deze categorie ziet is 'dubbele extensie verdachte file wordt uitgevoerd'
* **Beleid en de resourcestatus** -deze categorieën bevatten niet alle gebeurtenissen; ze zijn gereserveerd voor toekomstig gebruik.

## <a name="event-schema-per-category"></a>Schema van de gebeurtenis per categorie
[Zie dit artikel leert u het schema van de gebeurtenis activiteitenlogboek per categorie.](monitoring-activity-log-schema.md)

## <a name="what-you-can-do-with-the-activity-log"></a>Wat u kunt doen met het activiteitenlogboek
Hier volgen enkele dingen die u met het activiteitenlogboek doen kunt:

![Azure Activity log](./media/monitoring-overview-activity-logs/Activity_Log_Overview_v3.png)


* Vragen en deze bekijken in de **Azure-portal**.
* [Maak een waarschuwing op een activiteit van het gebeurtenislogboek.](monitoring-activity-log-alerts.md)
* [Stream naar een **Event Hub** ](monitoring-stream-activity-logs-event-hubs.md) voor opname door een service van derden of aangepaste analytics-oplossing zoals Power BI.
* Analyseren in Power BI met behulp van de [ **Power BI-inhoudspakket**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
* [Opslaan op een **Opslagaccount** voor archivering of handmatig inspectie](monitoring-archive-activity-log.md). Kunt u de bewaarperiode (in dagen) met behulp van de **logboek profiel**.
* Deze query's uitvoeren via PowerShell-Cmdlet, CLI of REST-API.

## <a name="query-the-activity-log-in-the-azure-portal"></a>Query het activiteitenlogboek in de Azure portal
U kunt uw activiteitenlogboek bekijken op verschillende plaatsen binnen de Azure-portal:
* De **activiteitenlogboek blade**, die u kunt openen door te zoeken naar het activiteitenlogboek onder 'Meer Services' in het navigatiedeelvenster links.
* De **Monitor blade**, die standaard in het navigatiedeelvenster links wordt weergegeven. Het activiteitenlogboek is een onderdeel van deze blade Azure Monitor.
* Een resource **resourceblade**, bijvoorbeeld de configuratie-blade voor een virtuele Machine. Het activiteitenlogboek is een van de secties op de meeste van deze resourceblades en de gebeurtenissen die betrekking hebben op specifieke hulpbron die automatisch erop te klikken filtert.

U kunt uw activiteitenlogboek door deze velden filteren in de Azure-portal:
* TimeSpan - de begin- en -tijd voor gebeurtenissen.
* Categorie - de gebeurteniscategorie zoals hierboven is beschreven.
* Abonnement - een of meer namen van de Azure-abonnement.
* Resourcegroep - een of meer resourcegroepen binnen deze abonnementen.
* Bron (naam) - de naam van een specifieke bron.
* Brontype - het type resource, bijvoorbeeld Microsoft.Compute/virtualmachines.
* Naam van de bewerking - de naam van een Azure Resource Manager-bewerking, bijvoorbeeld Microsoft.SQL/servers/Write.
* Ernst: de ernst van de gebeurtenis (ter informatie, waarschuwing, fout, kritiek).
* De gebeurtenis wordt gestart door - de 'beller', of de gebruiker die de bewerking uitgevoerd.
* Open search - dit is een open zoeken in het tekstvak waarin wordt gezocht naar de tekenreeks die tussen alle velden in alle gebeurtenissen.

Wanneer u een verzameling filters hebt gedefinieerd, kunt u deze opslaan als een query die over de sessies worden bewaard als u ooit moet dezelfde query uitvoeren met deze filters zijn toegepast opnieuw in de toekomst. U kunt ook een query vastmaken aan uw Azure-dashboard altijd gaten te houden van specifieke gebeurtenissen.

Te klikken op 'Toepassing' uw query wordt uitgevoerd en alle overeenkomende gebeurtenissen tonen. Te klikken op een gebeurtenis in de lijst ziet u de samenvatting van die gebeurtenis, evenals de volledige onbewerkte JSON van die gebeurtenis.

Voor nog meer voeding, kunt u de **logboek zoeken** pictogram waarin uw activiteitenlogboek van gegevens in de [Log Analytics activiteit Log Analytics-oplossing](../log-analytics/log-analytics-activity.md). De blade activiteitenlogboek biedt een eenvoudige filter-/ Bladerervaring van Logboeken, maar Log Analytics kunt u draait, opvragen en visualiseren van uw gegevens op krachtige manieren.

## <a name="export-the-activity-log-with-a-log-profile"></a>Het activiteitenlogboek met een logboek profiel exporteren
Een **logboek profiel** bepaalt hoe uw activiteitenlogboek wordt geëxporteerd. Een logboek-profiel gebruikt, kunt u configureren:

* Waar het activiteitenlogboek moeten worden verzonden (Storage-Account of Event Hubs)
* Welke gebeurteniscategorieën (schrijven, verwijderen, actie) moeten worden verzonden. *De betekenis van 'categorie' in het logboek profielen en logboekgebeurtenissen activiteit verschilt. In het logboek-profiel vertegenwoordigt "Categorie" de bewerking (schrijven, verwijderen, actie). De eigenschap "categorie" vertegenwoordigt in een gebeurtenis activiteitenlogboek van de bron- of type gebeurtenis (bijvoorbeeld, beheer, ServiceHealth, waarschuwing en meer).*
* Welke regio's (locaties) moeten worden geëxporteerd. Zorg ervoor dat 'global', zoals veel gebeurtenissen in het gebeurtenissenlogboek algemene gebeurtenissen zijn.
* Hoe lang het activiteitenlogboek worden bewaard in een Opslagaccount.
    - Een bewaartermijn van nul dagen betekent logboeken permanent worden bewaard. Anders wordt mag de waarde een onbeperkt aantal dagen tussen 1 en 2147483647.
    - Als bewaarbeleid worden ingesteld, maar Logboeken opslaan in een Opslagaccount is uitgeschakeld (bijvoorbeeld, als er alleen Event Hubs of OMS-opties zijn geselecteerd), is het bewaarbeleid hebben geen effect.
    - Bewaarbeleid zijn toegepaste per dag, dus aan het einde van een dag (UTC), logboeken van de dag dat nu is buiten de bewaarperiode beleid worden verwijderd. Bijvoorbeeld, als u had een bewaarbeleid van één dag, zou aan het begin van vandaag de dag de logboeken van de dag voordat gisteren worden verwijderd.

U kunt een opslag-account of gebeurtenis hub naamruimte die zich niet in hetzelfde abonnement als een tekensetcodering Logboeken kunt gebruiken. De gebruiker die de instelling configureert, moet de juiste RBAC-toegang tot beide abonnementen hebben.

Deze instellingen kunnen worden geconfigureerd via de optie "Export" in de blade activiteitenlogboek in de portal. Ze kunnen ook programmatisch te worden geconfigureerd [met de REST-API van Azure Monitor](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShell-cmdlets of CLI. Een abonnement kan slechts één logboek profiel hebben.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Logboek profielen met de Azure portal configureren
U kunt het activiteitenlogboek naar een Event Hub stream of opslaan in een Opslagaccount met de optie "Export" in de Azure-portal.

1. Navigeer naar de **activiteitenlogboek** blade via het menu aan de linkerkant van de portal.

    ![Navigeer naar activiteitenlogboek in portal](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klik op de **exporteren** knop aan de bovenkant van de blade.

    ![Knop exporteren in de portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. In de blade die wordt weergegeven, kunt u het volgende selecteren:  
  * regio's waarvoor u wilt exporteren van gebeurtenissen
  * het Opslagaccount waarnaar u wilt opslaan van gebeurtenissen
  * het aantal dagen dat u wilt bewaren van deze gebeurtenissen in de opslag. Een instelling van 0 dagen behoudt altijd de logboeken.
  * de Service Bus Namespace waarin u een Event Hub wilt voor streaming van deze gebeurtenissen worden gemaakt.

     ![Activiteitenlogboek blade exporteren](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klik op **opslaan** deze instellingen op te slaan. De instellingen zijn onmiddellijk worden toegepast op uw abonnement.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Logboek profielen met de PowerShell-Cmdlets voor Azure configureren
#### <a name="get-existing-log-profile"></a>Bestaande profiel voor het logboek ophalen
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Een profiel van het logboek toevoegen
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Eigenschap | Vereist | Beschrijving |
| --- | --- | --- |
| Naam |Ja |Naam van uw logboek-profiel. |
| StorageAccountId |Nee |Resource-ID van het Opslagaccount waarin het activiteitenlogboek moet worden opgeslagen. |
| serviceBusRuleId |Nee |Service Bus regel-ID voor de Service Bus-naamruimte die u hebben van event hubs gemaakt wilt in. Is een tekenreeks met deze indeling: `{service bus resource ID}/authorizationrules/{key name}`. |
| Locaties |Ja |Door komma's gescheiden lijst met regio's waarvoor u wilt verzamelen van gebeurtenissen voor Activity Log. |
| RetentionInDays |Ja |Aantal dagen voor welke gebeurtenissen worden bewaard, tussen 1 en 2147483647. Een waarde van nul wordt de logboeken voor onbepaalde tijd opgeslagen (permanent). |
| Categorieën |Nee |Door komma's gescheiden lijst met categorieën van gebeurtenissen die moeten worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en in te grijpen. |

#### <a name="remove-a-log-profile"></a>Een logboek-profiel verwijderen
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Logboek profielen met behulp van de platformoverschrijdende CLI van Azure configureren
#### <a name="get-existing-log-profile"></a>Bestaande profiel voor het logboek ophalen
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
De `name` eigenschap moet de naam van uw logboek-profiel.

#### <a name="add-a-log-profile"></a>Een profiel van het logboek toevoegen
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Eigenschap | Vereist | Beschrijving |
| --- | --- | --- |
| naam |Ja |Naam van uw logboek-profiel. |
| storageId |Nee |Resource-ID van het Opslagaccount waarin het activiteitenlogboek moet worden opgeslagen. |
| serviceBusRuleId |Nee |Service Bus regel-ID voor de Service Bus-naamruimte die u hebben van event hubs gemaakt wilt in. Is een tekenreeks met deze indeling: `{service bus resource ID}/authorizationrules/{key name}`. |
| Locaties |Ja |Door komma's gescheiden lijst met regio's waarvoor u wilt verzamelen van gebeurtenissen voor Activity Log. |
| RetentionInDays |Ja |Aantal dagen voor welke gebeurtenissen worden bewaard, tussen 1 en 2147483647. Een waarde van nul wordt de logboeken voor onbepaalde tijd opgeslagen (permanent). |
| categorieën |Nee |Door komma's gescheiden lijst met categorieën van gebeurtenissen die moeten worden verzameld. Mogelijke waarden zijn schrijven, verwijderen en in te grijpen. |

#### <a name="remove-a-log-profile"></a>Een logboek-profiel verwijderen
```
azure insights logprofile delete --name my_log_profile
```

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het activiteitenlogboek (voorheen controlelogboeken)](../azure-resource-manager/resource-group-audit.md)
* [Stream de Azure Activity Log naar Event Hubs](monitoring-stream-activity-logs-event-hubs.md)
