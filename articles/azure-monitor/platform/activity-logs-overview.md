---
title: Overzicht van de Azure-activiteitenlogboek
description: Ontdek wat de Azure-activiteitenlogboek er en hoe u deze kunt gebruiken om te begrijpen van gebeurtenissen die plaatsvinden binnen uw Azure-abonnement.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/19/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 6fc00bf0dfb83f349da91989a579f31be2027ff0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071671"
---
# <a name="overview-of-azure-activity-log"></a>Overzicht van Azure-activiteitenlogboek

De **Azure Activity Log** biedt inzicht in gebeurtenissen op abonnementsniveau die hebben plaatsgevonden in Azure. Dit omvat een scala aan gegevens van operationele gegevens van de Azure Resource Manager-updates op Service Health-gebeurtenissen. Het activiteitenlogboek was voorheen bekend als _auditlogboeken_ of _operationele logboeken_, omdat de beheercategorie controlelaag gebeurtenissen voor uw abonnementen rapporten. 

Het activiteitenlogboek gebruiken om te bepalen de _wat_, _die_, en _wanneer_ voor (PUT, POST, DELETE schrijfbewerkingen) die wordt gemaakt op de resources in uw abonnement. U kunt ook de status van de bewerking en andere relevante eigenschappen begrijpen. 

Het activiteitenlogboek bevat geen lees (GET)-bewerkingen of bewerkingen voor resources die gebruikmaken van het model Klassiek/RDFE.

## <a name="comparison-to-diagnostic-logs"></a>Vergelijking van diagnostische logboeken
Er is een eenmalige activiteitenlogboek voor elke Azure-abonnement. Het biedt gegevens over de bewerkingen op een resource van buitenaf (de "controlelaag"). [Diagnostische logboeken](diagnostic-logs-overview.md) worden gegenereerd door een resource en geef informatie op over de werking van die resource (de "gegevenslaag'). Diagnostische instellingen voor elke bron, moet u inschakelen.

![Activiteitenlogboeken in vergelijking met diagnostische logboeken](./media/activity-logs-overview/Activity_Log_vs_other_logs_v5.png)


> [!NOTE]
> De Azure-activiteitenlogboek is voornamelijk bedoeld voor activiteiten die in Azure Resource Manager plaatsvinden. Resources met behulp van het model Klassiek/RDFE worden niet bijgehouden. Sommige resourcetypen klassiek hebben een proxy-resourceprovider in Azure Resource Manager (bijvoorbeeld Microsoft.ClassicCompute). Als u met een resourcetype klassiek via Azure Resource Manager met behulp van deze proxy-resourceproviders werken, is de bewerkingen worden weergegeven in het activiteitenlogboek. Als u met een resourcetype klassiek buiten de Azure Resource Manager-proxy's communiceert, zijn alleen uw acties in het logboek geregistreerd. Het logboek kan worden gebladerd in een apart gedeelte van de portal.

## <a name="activity-log-retention"></a>Logboekbehoud activiteit
Nadat u hebt gemaakt, worden logboekvermeldingen activiteit niet gewijzigd of verwijderd door het systeem. U kunt geen ze ook wijzigen in de gebruikersinterface of via een programma. Gebeurtenissen in het activiteitenlogboek worden 90 dagen bewaard. Voor het opslaan van deze gegevens voor langere tijd [verzamelen in Azure Monitor](activity-log-collect.md) of [exporteren naar opslag of Event Hubs](activity-log-export.md).

## <a name="view-the-activity-log"></a>Raadpleeg het activiteitenlogboek
Raadpleeg het activiteitenlogboek voor alle resources uit de **Monitor** in het menu in de Azure-portal. Raadpleeg het activiteitenlogboek voor een bepaalde resource uit de **activiteitenlogboek** optie in het menu van die resource. U kunt ook activiteitenlogboek records met PowerShell, CLI of REST-API ophalen.  Zie [weergeven en ophalen van Azure-activiteit logboekgebeurtenissen](activity-log-view.md).

![Het activiteitenlogboek weergeven](./media/activity-logs-overview/view-activity-log.png)

## <a name="collect-activity-log-in-azure-monitor"></a>Verzamelen van activiteitenlogboek in Azure Monitor
Het activiteitenlogboek verzamelen in Log Analytics-werkruimte in Azure Monitor om te analyseren met andere bewakingsgegevens en om de gegevens langer dan 90 dagen te bewaren. Zie [verzamelen en analyseren van Azure-activiteitenlogboeken in Log Analytics-werkruimte in Azure Monitor](activity-log-collect.md).

![Query-activiteitenlogboek](./media/activity-logs-overview/query-activity-log.png)

## <a name="export-activity-log"></a>Activiteitenlogboek exporteren
Het activiteitenlogboek exporteren naar Azure Storage voor het archiveren of stream het naar een Event Hub voor opname door een service van derden of aangepaste analytics-oplossing. Zie [exporteren van de Azure-activiteitenlogboek](activity-log-export.md). U kunt ook gebeurtenissen in activiteitenlogboeken in met behulp van Power BI analyseren de [ **Power BI-inhoudspakket**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).

## <a name="alert-on-activity-log"></a>Waarschuwing voor activiteitenlogboek
U kunt een waarschuwing maken wanneer bepaalde gebeurtenissen worden gemaakt in het activiteitenlogboek met een [waarschuwing voor activiteitenlogboek](activity-log-alerts.md). U kunt ook maken een waarschuwing met een [logboekquery](alerts-log-query.md) wanneer uw activiteitenlogboek is verbonden met een Log Analytics-werkruimte, maar er zijn kosten verbonden aan te melden query uitvoeren op waarschuwingen. Er zijn geen kosten verbonden voor waarschuwingen voor activiteitenlogboek.

## <a name="categories-in-the-activity-log"></a>Categorieën in het activiteitenlogboek
Elke gebeurtenis in het activiteitenlogboek heeft een bepaalde categorie die in de volgende tabel worden beschreven. Zie voor volledige informatie over de schema's van deze categorieën vallen [gebeurtenisschema in het Azure-activiteitenlogboek](activity-log-schema.md). 

| Category | Description |
|:---|:---|
| Administratief | Bevat de record van alle maken, bijwerken, verwijderen en actie bewerkingen uitgevoerd via Resource Manager. Voorbeelden van administratieve gebeurtenissen zijn _maken van virtuele machine_ en _netwerkbeveiligingsgroep verwijderen_.<br><br>Elke actie op die door een gebruiker of een toepassing met behulp van Resource Manager is gemodelleerd als een bewerking op een bepaald resourcetype. Als het bewerkingstype _schrijven_, _verwijderen_, of _actie_, de records van de begin- en het slagen of mislukken van deze bewerking worden vastgelegd in de beheercategorie. Eventuele wijzigingen in de op rollen gebaseerd toegangsbeheer ook met beheerdersrechten gebeurtenissen in een abonnement. |
| Service Health | Bevat de record van de service health incidenten die hebben plaatsgevonden in Azure. Een voorbeeld van een Service-statusgebeurtenis _SQL Azure in VS-Oost ondervindt uitvaltijd_. <br><br>Er zijn vijf soorten service Health-gebeurtenissen: _Actie vereist_, _Recovery telefonische_, _Incident_, _onderhoud_, _informatie_, of  _Beveiliging_. Deze gebeurtenissen worden alleen gemaakt als er een resource in het abonnement dat wordt beïnvloed door de gebeurtenis.
| Status van resources | Bevat de record van een resource health-gebeurtenissen die hebben plaatsgevonden naar uw Azure-resources. Een voorbeeld van een gebeurtenis met Resource Health is _de integriteitsstatus van de virtuele Machine gewijzigd in niet beschikbaar_.<br><br>Resource Health-gebeurtenissen kunnen gelden voor een van de vier health-statussen: _Beschikbare_, _niet beschikbaar_, _gedegradeerd_, en _onbekende_. Resource Health-gebeurtenissen kunnen ook worden gecategoriseerd als _Platform gestart_ of _door de gebruiker geïnitieerde_. |
| Waarschuwing | Bevat de record van activeringen voor Azure-waarschuwingen. Een voorbeeld van een waarschuwingsgebeurtenis is _CPU-percentage op myVM is meer dan 80 voor de afgelopen vijf minuten_.|
| Automatisch schalen | Bevat de record van alle gebeurtenissen die betrekking hebben op de werking van de engine voor automatisch schalen op basis van alle instellingen voor automatisch schalen die u hebt gedefinieerd in uw abonnement. Een voorbeeld van een gebeurtenis voor automatisch schalen is _schalen via automatisch schalen actie kan niet_. |
| Aanbeveling | Bevat aanbevelingsgebeurtenissen die van Azure Advisor. |
| Beveiliging | Bevat de record van alle waarschuwingen die worden gegenereerd door Azure Security Center. Een voorbeeld van een beveiligingsgebeurtenis is _bestand met verdachte dubbele extensie uitgevoerd_. |
| Beleid | Bevat records voor alle bewerkingen met effect actie uitgevoerd door Azure Policy. Voorbeelden van beleid voor gebeurtenissen zijn _Audit_ en _weigeren_. Elke actie op die door het beleid is gemodelleerd als een bewerking op een resource. |


## <a name="next-steps"></a>Volgende stappen

* [Maken van een logboekprofiel voor het exporteren van de Azure-activiteitenlogboek](activity-log-export.md)
* [Stream het Azure-activiteitenlogboek naar Eventhubs](activity-logs-stream-event-hubs.md)
* [Archiveren van de Azure-activiteitenlogboek naar opslag](archive-activity-log.md)

