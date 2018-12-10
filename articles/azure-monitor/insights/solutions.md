---
title: De beheeroplossingen in Azure Monitor | Microsoft Docs
description: Management-oplossingen in Azure Monitor omvatten een verzameling van logica, visualisatie en gegevensverzameling gegevensverzamelingsregels waarmee metrische gegevens gedraaid rondom een specifiek probleemgebied.  In dit artikel bevat informatie over het installeren en gebruiken van oplossingen voor beheer.
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
ms.date: 12/07/2018
ms.author: bwren
ms.openlocfilehash: 1eac238c2379abf0b46d0975889a71ac7aa6a9ab
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53141557"
---
# <a name="management-solutions-in-azure-monitor"></a>De beheeroplossingen in Azure Monitor
Beheeroplossingen gebruikmaken van services in Azure om meer inzicht in de werking van een bepaalde toepassing of service. In dit artikel geeft een kort overzicht van oplossingen in Azure en meer informatie over het gebruiken en ze te installeren.

Beheeroplossingen doorgaans logboekgegevens verzamelen en query's en weergaven om verzamelde gegevens te analyseren. Ze kunnen ook gebruikmaken van andere services zoals Azure Automation om uit te voeren met betrekking tot de toepassing of service.

U kunt oplossingen toevoegen aan Azure Monitor voor alle toepassingen en services die u gebruikt. Ze zijn doorgaans beschikbaar er worden geen kosten, maar verzamelen gegevens kosten voor het gebruik kan worden aangeroepen. Naast de oplossingen die worden geleverd door Microsoft, partners en klanten kunnen [beheeroplossingen maken](solutions-creating.md) moet worden gebruikt in hun eigen omgeving of beschikbaar gesteld aan klanten via de community.

## <a name="use-management-solutions"></a>Oplossingen voor beheer gebruiken
Open de **overzicht** een tegel voor elke oplossing in de werkruimte geïnstalleerd op de pagina voor uw Log Analytics-werkruimte weergegeven. 

1. Meld u aan bij Azure Portal.
1. Open **alle services** en zoek **Monitor**.
1. Onder de **Insights** in het menu **meer**.
1. Gebruik de vervolgkeuzelijsten boven aan het scherm om de werkruimte of het tijdsbereik gebruikt voor de tegels te wijzigen.
1. Klik op de tegel voor een oplossing voor het openen van de weergave waarin meer gedetailleerde analyse de verzamelde gegevens.

![Overzicht](media/solutions/overview.png)

Oplossingen voor het beheer kunnen bevatten meerdere typen Azure-resources, en u kunt alle resources die deel uitmaakt van een oplossing net als bij een andere bron weergeven. Bijvoorbeeld, alle logboeken-query's die deel uitmaken van de oplossing worden vermeld in **oplossing query's** in [queryverkenner](../log-query/get-started-portal.md#load-queries) kunt u deze query's bij het uitvoeren van ad-hoc-analyse met Log Analytics.

## <a name="list-installed-management-solutions"></a>Lijst met geïnstalleerde beheeroplossingen 
Gebruik de volgende procedure om de beheeroplossingen die is geïnstalleerd in uw abonnement weer te geven.

1. Meld u aan bij Azure Portal.
1. Open **alle services** en zoek **oplossingen**.
4. Oplossingen die zijn geïnstalleerd in al uw werkruimten worden weergegeven. De naam van de oplossing wordt gevolgd door de naam van de Log Analytics-werkruimte die het wordt geïnstalleerd.
1. Gebruik de vervolgkeuzelijsten boven aan het scherm om te filteren op abonnement of resourcegroep.


![Vermeld alle oplossingen](media/solutions/list-solutions-all.png)

Klik op de naam van een oplossing voor het openen van de pagina overzicht. Deze pagina wordt weergegeven van alle weergaven die zijn opgenomen in de oplossing en biedt verschillende opties voor de oplossing zelf en de bijbehorende werkruimte. De overzichtspagina voor een oplossing met behulp van een van de procedures boven aan de lijst met oplossingen weergeven en klik vervolgens op de naam van de oplossing.

![Eigenschappen van oplossing](media/solutions/solution-properties.png)



## <a name="install-a-management-solution"></a>Installeren van een oplossing voor beheer
Management-oplossingen van Microsoft als partners beschikbaar zijn op de [Azure Marketplace](https://azuremarketplace.microsoft.com). U kunt zoeken naar beschikbare oplossingen en installeren met behulp van de volgende procedure. Wanneer u een oplossing installeert, moet u een [Log Analytics-werkruimte](../platform/manage-access.md) installatie van de oplossing en waar de gegevens worden verzameld.

1. Uit de [lijst met oplossingen voor uw abonnement](#list-installed-monitoring-solutions), klikt u op **toevoegen**. 
1. Aan de rechterkant van **beheeroplossingen**, klikt u op **meer**. 
1. Ga naar de beheeroplossing die u wilt gebruiken en lees de beschrijving.
1. Klik op **maken** om het installatieproces te starten.
1. Wanneer het installatieproces wordt gestart, wordt u gevraagd voor de vereiste configuratie die varieert voor elke oplossing.

![Installeren van een oplossing](media/solutions/install-solution.png)

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
Alle oplossingen vereisen een [Log Analytics-werkruimte](../platform/manage-access.md) voor het opslaan van gegevens die zijn verzameld door de oplossing en voor het hosten van de logboeken en weergaven. Sommige oplossingen ook vereisen een [Automation-account](../../automation/automation-security-overview.md#automation-account-overview) runbooks en verwante resources bevat. De werkruimte en account moeten voldoen aan de volgende vereisten.

* Elke installatie van een oplossing kan alleen een Log Analytics-werkruimte en een Automation-account gebruiken. U kunt de oplossing afzonderlijk installeren in meerdere werkruimten.
* Als een oplossing een Automation-account vereist, moeten klikt u vervolgens de Log Analytics-werkruimte en het Automation-account worden gekoppeld aan elkaar. Een Log Analytics-werkruimte kan alleen worden gekoppeld aan een Automation-account en een Automation-account kan slechts aan één Log Analytics-werkruimte zijn gekoppeld.
* De Log Analytics-werkruimte en het Automation-account om te worden gekoppeld, moeten zich in dezelfde resourcegroep en regio. De uitzondering is een werkruimte in de regio VS-Oost en Automation-account in VS-Oost 2.

### <a name="create-a-link-between-a-log-analytics-workspace-and-automation-account"></a>Een koppeling tussen een Log Analytics-werkruimte en een Automation-account maken
Hoe u de Log Analytics-werkruimte en het Automation-account opgeven, is afhankelijk van de installatiemethode voor uw oplossing.

* Wanneer u een oplossing via Azure Marketplace installeert, wordt u gevraagd voor een werkruimte en het Automation-account. De koppeling tussen beide wordt gemaakt als ze niet al is gekoppeld.
* Voor oplossingen buiten de Azure Marketplace, moet u de Log Analytics-werkruimte en het Automation-account koppelen voordat de installatie van de oplossing. U kunt dit doen door het selecteren van een oplossing in de Azure Marketplace en de Log Analytics-werkruimte en het Automation-account te selecteren. U hebt geen daadwerkelijk de oplossing installeren omdat de koppeling wordt gemaakt als de Log Analytics-werkruimte en het Automation-account zijn geselecteerd. Nadat de koppeling is gemaakt, kunt u die Log Analytics-werkruimte en het Automation-account gebruiken voor elke oplossing.

### <a name="verify-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Controleer of de koppeling tussen een Log Analytics-werkruimte en een Automation-account
U kunt controleren of de koppeling tussen een Log Analytics-werkruimte en een Automation-account met behulp van de volgende procedure.

1. Selecteer het Automation-account in Azure portal.
1. Schuif naar de **gerelateerde Resources** gedeelte van het menu.
1. Als de **werkruimte** instelling is ingeschakeld, wordt deze account is gekoppeld aan een Log Analytics-werkruimte. U kunt klikken op **werkruimte** om de details van de werkruimte weer te geven.

## <a name="remove-a-management-solution"></a>Verwijderen van een oplossing voor beheer
Als u wilt verwijderen van een oplossing voor is geïnstalleerd, zoekt u deze in de [lijst met geïnstalleerde oplossingen](#list-installed-monitoring-solutions). Klik op de naam van de oplossing voor het openen van de pagina Samenvatting en klik vervolgens op **verwijderen**.




## <a name="next-steps"></a>Volgende stappen
* Krijgen een [lijst met oplossingen van Microsoft](solutions-inventory.md).
* Meer informatie over het [query's maken](../../azure-monitor/log-query/log-query-overview.md) voor het analyseren van gegevens die zijn verzameld door oplossingen voor beheer.

