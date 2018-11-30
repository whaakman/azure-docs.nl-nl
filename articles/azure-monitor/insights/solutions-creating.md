---
title: Een oplossing in Azure bouwen | Microsoft Docs
description: Beheeroplossingen omvatten verpakte beheerscenario's in Azure die klanten aan de Log Analytics-werkruimte toevoegen kunnen.  In dit artikel vindt u informatie over hoe u oplossingen voor moet worden gebruikt in uw eigen omgeving kunt maken of beschikbaar wordt gesteld aan uw klanten.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4fd36d58574b60e3e6351cba03c68b9217bc703d
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52632463"
---
# <a name="design-and-build-a-management-solution-in-azure-preview"></a>Ontwerpen en bouwen van een oplossing in Azure (Preview)
> [!NOTE]
> Dit is een voorlopige documentatie voor het maken van oplossingen in Azure die zich momenteel in preview. Er is geen schema die hieronder worden beschreven kan worden gewijzigd.

[Beheeroplossingen]( solutions.md) bieden verpakte beheerscenario's die klanten aan de Log Analytics-werkruimte toevoegen kunnen.  In dit artikel geeft een basic-proces voor het ontwerpen en bouwen van een beheeroplossing die geschikt is voor de meest voorkomende vereisten.  Als u niet bekend bent met het bouwen van oplossingen voor het beheer kunt u dit proces gebruiken als een beginpunt en vervolgens gebruikmaken van de concepten voor meer complexe oplossingen als uw vereisten veranderen.

## <a name="what-is-a-management-solution"></a>Wat is een beheeroplossing?

Oplossingen voor bevatten Azure-resources die samenwerken om een bepaald beheerscenario.  Ze worden geïmplementeerd als [Resource Management-sjablonen](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md) die bevatten meer informatie over het installeren en configureren van hun ingesloten resources, wanneer de oplossing is geïnstalleerd.

De basic-strategie is om te beginnen de uw beheeroplossing voor het bouwen van de afzonderlijke onderdelen in uw Azure-omgeving.  Zodra u de functionaliteit die goed werkt hebt, klikt u vervolgens kunt u starten verpakking ze naar een [management oplossingsbestand]( solutions-solution-file.md). 


## <a name="design-your-solution"></a>Ontwerp uw oplossing
De meest algemene patroon voor een oplossing voor beheer wordt in het volgende diagram weergegeven.  De verschillende onderdelen van dit patroon worden beschreven de hieronder.

![Overzicht van de Management-oplossing](media/solutions-creating/solution-overview.png)


### <a name="data-sources"></a>Gegevensbronnen
De eerste stap bij het ontwerpen van een oplossing is het vaststellen van de gegevens die u nodig van de opslagplaats van Log Analytics hebt.  Deze gegevens kan worden verzameld door een [gegevensbron](../../azure-monitor/platform/agent-data-sources.md) of [een andere oplossing]( solutions.md), of uw oplossing moet mogelijk opgeven van het proces voor het verzamelen van deze.

Er zijn een aantal manieren om gegevensbronnen die kunnen worden verzameld in de Log Analytics-opslagplaats, zoals beschreven in [gegevensbronnen in Log Analytics](../../azure-monitor/platform/agent-data-sources.md).  Dit omvat gebeurtenissen in het Windows-gebeurtenislogboek of die worden gegenereerd door Syslog naast prestatiemeteritems voor zowel Windows als Linux-clients.  U kunt ook gegevens verzamelen van Azure-resources die door Azure Monitor worden verzameld.  

Als u gegevens die niet toegankelijk zijn via een van de beschikbare gegevensbronnen nodig hebt, dan kunt u de [HTTP Data Collector API](../../log-analytics/log-analytics-data-collector-api.md) waarmee u gegevens schrijven naar de Log Analytics-opslagplaats van een willekeurige client die een REST-API kunt aanroepen.  Aangepaste gegevensverzameling in een oplossing voor de meest voorkomende manier is het maken van een [runbook in Azure Automation](../../automation/automation-runbook-types.md) die de vereiste gegevens verzamelt van Azure of externe resources en de Collector-API gebruikt om te schrijven naar de opslagplaats.  

### <a name="log-searches"></a>Zoekopdrachten in Logboeken
[Zoekopdrachten](../../log-analytics/log-analytics-queries.md) worden gebruikt om te extraheren en analyseren van gegevens in de Log Analytics-opslagplaats.  Ze worden gebruikt door weergaven en waarschuwingen naast de mogelijkheid van de gebruiker voor het uitvoeren van ad hoc-analyse van gegevens in de opslagplaats.  

U moet alle query's waarvan u denkt is handig om de gebruiker dat, zelfs als ze niet worden gebruikt door alle weergaven of waarschuwingen definiëren.  Deze worden weergegeven die voor hen beschikbaar als opgeslagen zoekopdrachten in de portal en u kunt ook opnemen in een [lijstquery's visualisatie onderdeel](../../azure-monitor/platform/view-designer-parts.md#list-of-queries-part) in uw aangepaste weergave.

### <a name="alerts"></a>Waarschuwingen
[Waarschuwingen in Log Analytics](../../monitoring-and-diagnostics/monitoring-overview-alerts.md) geven aan welke problemen via [zoekopdrachten](#log-searches) ten opzichte van de gegevens in de opslagplaats.  Ze een melding van de gebruiker of automatisch een actie uitvoeren als reactie. U moet verschillende waarschuwing voorwaarden voor uw toepassing te identificeren en bijbehorende waarschuwingsregels opnemen in uw oplossingsbestand.

Als het probleem kan mogelijk worden opgelost met een geautomatiseerd proces, zult u doorgaans een runbook maken in Azure Automation om uit te voeren van dit herstel.  De meeste Azure-services kunnen worden beheerd met [cmdlets](/powershell/azure/overview) die het runbook wilt gebruikmaken van dergelijke functies kunnen worden uitgevoerd.

Als uw oplossing externe functionaliteit in reactie op een waarschuwing vereist, dan kunt u een [antwoord voor webhook](../../monitoring-and-diagnostics/alert-metric.md).  Hiermee kunt u een externe webservice verzenden van gegevens uit de waarschuwing niet aanroepen.

### <a name="views"></a>Weergaven
Weergaven in Log Analytics worden gebruikt voor het visualiseren van gegevens uit de opslagplaats van Log Analytics.  Elke oplossing bevatten doorgaans één weergave met een [tegel](../../azure-monitor/platform/view-designer-tiles.md) die wordt weergegeven op de belangrijkste dashboard van de gebruiker.  De weergave mag een willekeurig aantal [visualisatie delen](../../azure-monitor/platform/view-designer-parts.md) voor verschillende visualisaties van de verzamelde gegevens voor de gebruiker.

U [aangepaste weergaven maken met de ontwerper](../../azure-monitor/platform/view-designer.md) die u kunt later exporteren voor opname in het oplossingsbestand van uw.  


## <a name="create-solution-file"></a>Oplossingsbestand maken
Nadat u hebt geconfigureerd en de onderdelen die deel van uw oplossing uitmaken getest, kunt u [maken uw oplossingsbestand]( solutions-solution-file.md).  U implementeert de oplossingsonderdelen in een [Resource Manager-sjabloon](../../azure-resource-manager/resource-group-authoring-templates.md) die bevat een [oplossing resource]( solutions-solution-file.md#solution-resource) met een relatie tot de andere resources in het bestand.  


## <a name="test-your-solution"></a>Testen van uw oplossing
Terwijl u uw oplossing ontwikkelt, moet u het installeren en testen in uw werkruimte.  U kunt dit doen met behulp van een van de beschikbare methoden aan [testen en installeren van Resource Manager-sjablonen](../../azure-resource-manager/resource-group-template-deploy.md).

## <a name="publish-your-solution"></a>Publiceer uw oplossing
Nadat u hebt voltooid en uw oplossing getest, kunt u deze beschikbaar maken via de volgende bronnen aan klanten.

- **Azure Quickstart-sjablonen**.  [Azure Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/) is een set van Resource Manager-sjablonen bijgedragen door de community via GitHub.  U kunt uw oplossing beschikbaar maken door de volgende informatie in de [handleiding voor bijdragen](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE).
- **Azure Marketplace**.  De [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) kunt u distribueren en verkoop uw oplossing met andere ontwikkelaars, ISV's, en IT-professionals.  U kunt informatie over het publiceren van uw oplossing op Azure Marketplace op [over het publiceren en beheren van een aanbieding op Azure Marketplace](../../marketplace/marketplace-publishers-guide.md).



## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [maken van een oplossingsbestand]( solutions-solution-file.md) voor uw oplossing.
* Lees meer over van [Authoring Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-authoring-templates.md).
* Search [Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates) voor voorbeelden van verschillende Resource Manager-sjablonen.
