---
title: Management-oplossingen in Azure | Microsoft Docs
description: Management-oplossingen in Azure omvatten een verzameling van logica, visualisatie en gegevensverzameling gegevensverzamelingsregels waarmee metrische gegevens gedraaid rondom een specifiek probleemgebied.  In dit artikel bevat informatie over het installeren en gebruiken van oplossingen voor beheer.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: bwren
ms.openlocfilehash: 53020eeb96927143b2fb7394aee64f9b7ea55df9
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248959"
---
# <a name="management-solutions-in-azure"></a>Management-oplossingen in Azure
Beheeroplossingen gebruikmaken van services in Azure om meer inzicht in de werking van een bepaalde toepassing of service. In dit artikel geeft een kort overzicht van oplossingen in Azure en meer informatie over het gebruiken en ze te installeren.

Beheeroplossingen doorgaans informatie verzamelen in Log Analytics en zoekopdrachten in Logboeken en weergaven om verzamelde gegevens te analyseren. Ze kunnen ook gebruikmaken van andere services zoals Azure Automation om uit te voeren met betrekking tot de toepassing of service.

U kunt oplossingen toevoegen aan uw Azure-abonnement voor alle toepassingen en services die u gebruikt. Ze zijn doorgaans beschikbaar er worden geen kosten, maar verzamelen gegevens kosten voor het gebruik kan worden aangeroepen. Naast de oplossingen die worden geleverd door Microsoft, partners en klanten kunnen [beheeroplossingen maken](../monitoring/monitoring-solutions-creating.md) moet worden gebruikt in hun eigen omgeving of beschikbaar gesteld aan klanten via de community.

## <a name="using-management-solutions"></a>Met behulp van oplossingen voor beheer
De **overzicht** pagina voor elke Log Analytics-werkruimte wordt weergegeven voor een tegel voor elke oplossing in de werkruimte geïnstalleerd. Klik op de tegel voor de oplossing om de weergave waarin meer gedetailleerde analyse te openen de verzamelde gegevens.

![Overzicht](media/monitoring-solutions/overview.png)

Oplossingen voor het beheer kunnen bevatten meerdere typen Azure-resources, en u kunt alle resources die deel uitmaakt van een oplossing net als bij een andere bron weergeven. Bijvoorbeeld, een zoekopdrachten in Logboeken opgenomen in de oplossing zijn opgenomen in **opgeslagen zoekopdrachten** in de werkruimte. U kunt deze zoekopdrachten gebruiken bij het uitvoeren van ad hoc-analyses in Log Analytics.

## <a name="list-installed-management-solutions"></a>Lijst met geïnstalleerde beheeroplossingen 
Gebruik de volgende procedure om de beheeroplossingen die is geïnstalleerd in uw abonnement weer te geven.

1. Meld u aan bij Azure Portal.
2. Selecteer in het linkerdeelvenster **alle services**.
3. Een Schuif omlaag naar **oplossingen** of type *oplossingen* in de **Filter** dialoogvenster.
4. Oplossingen die zijn geïnstalleerd in al uw werkruimten worden weergegeven. De naam van de oplossing wordt gevolgd door de naam van de Log Analytics-werkruimte die het wordt geïnstalleerd.
1. Gebruik de vervolgkeuzelijsten boven aan het scherm om te filteren op abonnement of resourcegroep.


![Vermeld alle oplossingen](media/monitoring-solutions/list-solutions-all.png)

Klik op de naam van een oplossing voor het openen van de pagina overzicht. Deze pagina toont alle Log Analytics-weergaven opgenomen in de oplossing en biedt verschillende opties voor de oplossing zelf en de bijbehorende werkruimte. De overzichtspagina voor een oplossing met behulp van een van de procedures boven aan de lijst met oplossingen weergeven en klik vervolgens op de naam van de oplossing.

![Eigenschappen van oplossing](media/monitoring-solutions/solution-properties.png)



## <a name="install-a-management-solution"></a>Installeren van een oplossing voor beheer
Management-oplossingen van Microsoft als partners beschikbaar zijn op de [Azure Marketplace](https://azuremarketplace.microsoft.com). U kunt zoeken naar beschikbare oplossingen en installeren met behulp van de volgende procedure.

1. Uit de [lijst met oplossingen voor uw abonnement](#list-installed-management-solutions), klikt u op **toevoegen**. 
1. Aan de rechterkant van **beheeroplossingen**, klikt u op **meer**. 
1. Ga naar de beheeroplossing die u wilt gebruiken en lees de beschrijving.
1. Klik op **maken** om het installatieproces te starten.
1. Wanneer het installatieproces wordt gestart, wordt u gevraagd voor de vereiste configuratie die varieert voor elke oplossing. Al deze moet u een Log Analytics-werkruimte te selecteren waarop u de oplossing wordt geïnstalleerd en waar de gegevens worden verzameld. 

![Installeren van een oplossing](media/monitoring-solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>Installeren van een oplossing van de community
Leden van de community kunnen oplossingen voor gegevensbeheer voor Azure-Snelstartsjablonen indienen. U kunt deze oplossingen rechtstreeks installeren of download deze sjablonen later installeren.

1. Volg de procedure beschreven in [Log Analytics-werkruimte en het Automation-account](#log-analytics-workspace-and-automation-account) om een werkruimte en account te koppelen.
2. Ga naar [Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/). 
3. Zoeken naar een oplossing waarin u geïnteresseerd bent.
4. Selecteer de oplossing in de resultaten om de details ervan weer te geven.
5. Klik op de **implementeren in Azure** knop.
6. U wordt gevraagd om informatie te geven, zoals de resourcegroep en locatie samen met waarden voor parameters die in de oplossing.
7. Klik op **aankoop** voor het installeren van de oplossing.


## <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics-werkruimte en het Automation-account
Alle oplossingen vereisen een [Log Analytics-werkruimte](../log-analytics/log-analytics-manage-access.md) voor het opslaan van gegevens die zijn verzameld door de oplossing en voor het hosten van de logboeken en weergaven. Sommige oplossingen ook vereisen een [Automation-account](../automation/automation-security-overview.md#automation-account-overview) runbooks en verwante resources bevat. De werkruimte en account moeten voldoen aan de volgende vereisten.

* Elke installatie van een oplossing kan alleen een Log Analytics-werkruimte en een Automation-account gebruiken. U kunt de oplossing afzonderlijk installeren in meerdere werkruimten.
* Als een oplossing een Automation-account vereist, moeten klikt u vervolgens de Log Analytics-werkruimte en het Automation-account worden gekoppeld aan elkaar. Een Log Analytics-werkruimte kan alleen worden gekoppeld aan een Automation-account en een Automation-account kan slechts aan één Log Analytics-werkruimte zijn gekoppeld.
* De Log Analytics-werkruimte en het Automation-account om te worden gekoppeld, moeten zich in dezelfde resourcegroep en regio. De uitzondering is een werkruimte in de regio VS-Oost en Automation-account in VS-Oost 2.

### <a name="creating-a-link-between-a-log-analytics-workspace-and-automation-account"></a>Het maken van een koppeling tussen een Log Analytics-werkruimte en een Automation-account
Hoe u de Log Analytics-werkruimte en het Automation-account opgeven, is afhankelijk van de installatiemethode voor uw oplossing.

* Wanneer u een oplossing via Azure Marketplace installeert, wordt u gevraagd voor een werkruimte en het Automation-account. De koppeling tussen beide wordt gemaakt als ze niet al is gekoppeld.
* Voor oplossingen buiten de Azure Marketplace, moet u de Log Analytics-werkruimte en het Automation-account koppelen voordat de installatie van de oplossing. U kunt dit doen door het selecteren van een oplossing in de Azure Marketplace en de Log Analytics-werkruimte en het Automation-account te selecteren. U hebt geen daadwerkelijk de oplossing installeren omdat de koppeling wordt gemaakt als de Log Analytics-werkruimte en het Automation-account zijn geselecteerd. Nadat de koppeling is gemaakt, kunt u die Log Analytics-werkruimte en het Automation-account gebruiken voor elke oplossing.

### <a name="verifying-the-link-between-a-log-analytics-workspace-and-automation-account"></a>De koppeling tussen een Log Analytics-werkruimte en een Automation-account controleren
U kunt controleren of de koppeling tussen een Log Analytics-werkruimte en een Automation-account met behulp van de volgende procedure.

1. Selecteer het Automation-account in Azure portal.
1. Schuif naar de **gerelateerde Resources** gedeelte van het menu.
1. Als de **werkruimte** instelling is ingeschakeld, wordt deze account is gekoppeld aan een Log Analytics-werkruimte. U kunt klikken op **werkruimte** om de details van de werkruimte weer te geven.

## <a name="remove-a-management-solution"></a>Verwijderen van een oplossing voor beheer
Als u wilt verwijderen van een oplossing voor is geïnstalleerd, zoekt u deze in de [lijst met geïnstalleerde oplossingen](#list-installed-management-solutions). Klik op de naam van de oplossing voor het openen van de pagina Samenvatting en klik vervolgens op **verwijderen**.




## <a name="next-steps"></a>Volgende stappen
* Krijgen een [lijst met oplossingen van Microsoft](monitoring-solutions-inventory.md).
* Meer informatie over het [query's maken](../log-analytics/log-analytics-queries.md) voor het analyseren van gegevens die zijn verzameld door oplossingen voor beheer.

