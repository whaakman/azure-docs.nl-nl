---
title: Een oplossing in OMS opzetten | Microsoft Docs
description: De functionaliteit van Operations Management Suite (OMS) uitbreiden beheeroplossingen door verpakte beheerscenario die klanten aan hun OMS-werkruimte toevoegen kunnen.  Dit artikel biedt details over hoe u oplossingen voor het beheer moet worden gebruikt in uw eigen omgeving kunt maken of beschikbaar gesteld aan uw klanten.
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6ea959f1e95ea46c07eec3afa9d46bfeb72ca3e4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="design-and-build-a-management-solution-in-operations-management-suite-oms-preview"></a>Ontwerpen en bouwen van een beheeroplossing in Operations Management Suite (OMS) (Preview)
> [!NOTE]
> Dit is voorlopige documentatie voor het maken van oplossingen voor het beheer in OMS die zich momenteel in preview. De hieronder beschreven schema kan worden gewijzigd.

[Oplossingen voor](operations-management-suite-solutions.md) breiden de functionaliteit van Operations Management Suite (OMS) doordat verpakte beheerscenario die klanten aan hun OMS-werkruimte toevoegen kunnen.  Dit artikel biedt een basic-proces voor het ontwerpen en bouwen van een beheersysteem dat geschikt is voor de meest voorkomende vereisten.  Als u niet bekend bent met het bouwen van oplossingen voor het beheer kunt u dit proces als uitgangspunt gebruiken en vervolgens gebruikmaken van de concepten voor complexere oplossingen zoals ontwikkelen van uw vereisten.

## <a name="what-is-a-management-solution"></a>Wat is een oplossing voor het beheer?

Beheeroplossingen bevatten OMS en Azure-resources die samenwerken voor een bepaald scenario bewaking.  Ze worden geïmplementeerd als [Resource Management-sjablonen](../azure-resource-manager/resource-manager-template-walkthrough.md) die bevatten meer informatie over het installeren en configureren van hun ingesloten bronnen als de oplossing is geïnstalleerd.

De basis-strategie is uw beheeroplossing voor starten door het bouwen van de afzonderlijke onderdelen in uw Azure-omgeving.  Wanneer u beschikt over de functionaliteit correct werkt, klikt u vervolgens kunt u beginnen verpakken in een [management oplossingsbestand](operations-management-suite-solutions-solution-file.md). 


## <a name="design-your-solution"></a>Ontwerp uw oplossing
De meest voorkomende patroon voor een oplossing wordt weergegeven in het volgende diagram.  De verschillende onderdelen in dit patroon worden besproken in de onderstaande.

![Overzicht van de OMS-oplossing](media/operations-management-suite-solutions/solution-overview.png)


### <a name="data-sources"></a>Gegevensbronnen
De gegevens die u nodig uit de opslagplaats voor logboekanalyse hebt is het vaststellen van de eerste stap bij het ontwerpen van een oplossing.  Deze gegevens kan worden verzameld door een [gegevensbron](../log-analytics/log-analytics-data-sources.md) of [een andere oplossing](operations-management-suite-solutions.md), of uw oplossing moet het proces voor het verzamelen van deze bieden.

Er zijn een aantal manieren gegevensbronnen die kunnen worden verzameld in de opslagplaats Log Analytics, zoals beschreven in [gegevensbronnen in logboekanalyse](../log-analytics/log-analytics-data-sources.md).  Dit omvat de gebeurtenissen in het gebeurtenislogboek van Windows of door Syslog naast prestatiemeteritems voor Windows- en Linux-clients wordt gegenereerd.  U kunt ook gegevens verzamelen van Azure-resources die door Azure Monitor worden verzameld.  

Als u gegevens die niet toegankelijk zijn via een van de beschikbare gegevensbronnen nodig hebt, dan kunt u de [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md) waarmee u gegevens schrijven naar de opslagplaats logboekanalyse vanaf elke client die een REST-API kan aanroepen.  De meest voorkomende middelen van de verzameling aangepaste gegevens in een beheersysteem is het maken van een [runbook in Azure Automation](../automation/automation-runbook-types.md) die de vereiste gegevens verzamelt van Azure of externe bronnen en de API van Data Collector gebruikt om te schrijven naar de opslagplaats.  

### <a name="log-searches"></a>Logboek zoekopdrachten
[Meld u zoekopdrachten](../log-analytics/log-analytics-log-searches.md) worden gebruikt voor het ophalen en analyseren van gegevens in de opslagplaats logboekanalyse.  Ze worden gebruikt door weergaven en waarschuwingen naast de mogelijkheid van de gebruiker voor ad-hoc analyse van gegevens in de opslagplaats.  

U moet alle query's die u denkt dat nuttig is voor de gebruiker worden zelfs als ze niet worden gebruikt door alle weergaven of waarschuwingen definiëren.  Deze worden weergegeven die voor hen beschikbaar als opgeslagen zoekacties in de portal en u kunt ook opnemen in een [lijst van query's visualisatie onderdeel](../log-analytics/log-analytics-view-designer-parts.md#list-of-queries-part) in uw aangepaste weergave.

### <a name="alerts"></a>Waarschuwingen
[Waarschuwingen in logboekanalyse](../log-analytics/log-analytics-alerts.md) geven aan welke problemen via [Meld zoekopdrachten](#log-searches) op basis van de gegevens in de opslagplaats.  Ze ofwel de gebruiker waarschuwen of automatisch een actie wordt uitgevoerd in het antwoord. U moet verschillende waarschuwing voorwaarden voor uw toepassing te identificeren en bijbehorende waarschuwingsregels opnemen in uw oplossingsbestand.

Als het probleem kan mogelijk worden opgelost met een geautomatiseerd proces, gaat u doorgaans een runbook maken in Azure Automation deze herstel moet worden uitgevoerd.  De meeste Azure-services kunnen worden beheerd met [cmdlets](/powershell/azure/overview) die het runbook zou gebruikmaken van dergelijke functies kunnen worden uitgevoerd.

Als uw oplossing voor externe functionaliteit in reactie op een waarschuwing is vereist, dan kunt u een [webhook antwoord](../log-analytics/log-analytics-alerts-actions.md).  Hiermee kunt u een externe webservice verzenden van informatie van de waarschuwing niet aanroepen.

### <a name="views"></a>Weergaven
Weergaven in logboekanalyse worden gebruikt om gegevens uit de opslagplaats voor logboekanalyse te visualiseren.  Elke oplossing bevatten doorgaans één weergave met een [tegel](../log-analytics/log-analytics-view-designer-tiles.md) die wordt weergegeven op het hoofddashboard van de gebruiker.  De weergave kan een onbeperkt aantal bevatten [visualisatie delen](../log-analytics/log-analytics-view-designer-parts.md) bieden verschillende visualisaties van de verzamelde gegevens aan de gebruiker.

U [aangepaste weergaven maken met de ontwerper](../log-analytics/log-analytics-view-designer.md) die u kunt later exporteren voor insluiting in uw oplossingsbestand.  


## <a name="create-solution-file"></a>Oplossingsbestand maken
Nadat u hebt geconfigureerd en de onderdelen die deel van uw oplossing uitmaken getest, kunt u [maken van uw oplossingsbestand](operations-management-suite-solutions-solution-file.md).  U implementeert de oplossingsonderdelen in een [Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md) die bevat een [oplossing resource](operations-management-suite-solutions-solution-file.md#solution-resource) relaties naar de andere bronnen in het bestand.  


## <a name="test-your-solution"></a>Testen van uw oplossing
Terwijl u uw oplossing ontwikkelt, moet u voor het installeren en testen in uw werkruimte.  U kunt dit doen met behulp van een van de beschikbare methoden voor [testen en installeren van Resource Manager-sjablonen](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="publish-your-solution"></a>Publiceren van uw oplossing
Zodra u hebt voltooid en uw oplossing getest, kunt u het beschikbaar maken voor klanten via de volgende bronnen.

- **Azure-snelstartsjablonen**.  [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/) is een set van Resource Manager-sjablonen die is bijgedragen door de community via GitHub.  U kunt uw oplossing beschikbaar maken door de volgende informatie in de [bijdrage handleiding](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE).
- **Azure Marketplace**.  De [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) kunt u voor de distributie en uw oplossing verkopen aan andere ontwikkelaars, onafhankelijke softwareleveranciers, en IT-professionals.  U kunt informatie over het publiceren van uw oplossing naar Azure Marketplace op [het publiceren en beheren van een aanbieding in Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).



## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [maken van een oplossingsbestand](operations-management-suite-solutions-solution-file.md) voor uw beheeroplossing.
* Lees meer over [Azure Resource Manager-sjablonen samenstellen](../azure-resource-manager/resource-group-authoring-templates.md).
* Search [Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates) voor voorbeelden van andere Resource Manager-sjablonen.