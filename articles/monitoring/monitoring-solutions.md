---
title: Oplossingen in Azure | Microsoft Docs
description: Oplossingen in Azure is een verzameling van logica, visualiseren en gegevens overname regels waarmee de metrische gegevens om een bepaald probleemgebied gedraaid.  In dit artikel bevat informatie over het installeren en gebruiken van oplossingen voor het beheer.
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
ms.date: 04/20/2018
ms.author: bwren
ms.openlocfilehash: 0df54d1758693bce5fb5fd74c3be9c4cfd7dccb6
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="management-solutions-in-azure"></a>Oplossingen in Azure
Beheeroplossingen gebruikmaken van services in Azure om meer inzicht in de werking van een bepaalde toepassing of service. Dit artikel bevat een kort overzicht van oplossingen in Azure en meer informatie over het gebruiken en ze installeren.

Oplossingen voor het beheer wordt doorgaans gegevens verzamelen in Log Analytics en geef logboek zoekopdrachten en weergaven om verzamelde gegevens te analyseren. Ze kunnen ook gebruikmaken van andere services zoals Azure Automation voor het uitvoeren van acties met betrekking tot de toepassing of service.

U kunt beheeroplossingen toevoegen aan uw Azure-abonnement voor alle toepassingen en services die u gebruikt. Ze zijn normaal gesproken op geen kosten maar verzamelen gegevens die gebruikskosten kan worden aangeroepen. Naast oplossingen geleverd door Microsoft, partners en klanten kunnen [beheeroplossingen maken](../monitoring/monitoring-solutions-creating.md) moet worden gebruikt in hun eigen omgeving of beschikbaar gesteld via de community aan klanten.

## <a name="using-management-solutions"></a>Met behulp van oplossingen voor het beheer
De **overzicht** pagina voor elke werkruimte voor logboekanalyse een tegel voor elke oplossing geïnstalleerd in de werkruimte wordt weergegeven. Klik op de tegel voor de oplossing voor de weergave waaronder meer gedetailleerde analyse opent de verzamelde gegevens.

![Overzicht](media/monitoring-solutions/overview.png)

Oplossingen voor meerdere typen van Azure-resources kunnen bevatten en u kunt alle resources die deel uitmaakt van een oplossing net als elke andere bron weergeven. Bijvoorbeeld, zoekopdrachten logboek opgenomen in de oplossing zijn opgenomen in **opgeslagen zoekacties** in de werkruimte. U kunt deze zoekopdrachten gebruiken bij het uitvoeren van ad-hoc analyse in logboekanalyse.

## <a name="list-installed-management-solutions"></a>Lijst met geïnstalleerde beheeroplossingen 
Gebruik de volgende procedure voor een lijst met de beheeroplossingen geïnstalleerd in uw abonnement.

1. Meld u aan bij Azure Portal.
2. Selecteer in het linkerdeelvenster **alle services**.
3. Een Schuif omlaag naar **oplossingen** of type *oplossingen* in de **Filter** dialoogvenster.
4. Oplossingen die zijn geïnstalleerd in al uw werkruimten worden vermeld. De naam van de oplossing wordt gevolgd door de naam van de werkruimte voor logboekanalyse die is geïnstalleerd in.
1. Gebruik de vervolgkeuzelijsten aan de bovenkant van het scherm om te filteren op abonnement of resourcegroep.


![Lijst van alle oplossingen](media/monitoring-solutions/list-solutions-all.png)

Klik op de naam van een oplossing voor het openen van de pagina overzicht. Deze pagina geeft alle weergaven logboekanalyse is opgenomen in de oplossing en biedt verschillende opties voor de oplossing zelf en de bijbehorende werkruimte. De overzichtspagina voor een oplossing met behulp van een van de procedures boven aan de lijst met oplossingen weergeven en klik vervolgens op de naam van de oplossing.

![Eigenschappen van oplossing](media/monitoring-solutions/solution-properties.png)


## <a name="find-management-solutions"></a>Oplossingen voor het beheer zoeken
U kunt bladeren en het installeren van beschikbare oplossingen voor het beheer van Microsoft en partners in de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace). Voer een [zoeken naar *beheeroplossingen* ](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions) filteren voor oplossingen voor het beheer en klik vervolgens op een item voor meer informatie.

![Marketplace](media/monitoring-solutions/marketplace.png)

## <a name="install-a-management-solution"></a>Een oplossing voor het beheer installeren

### <a name="install-a-management-solution-from-the-azure-marketplace"></a>Installeren van een beheeroplossing vanuit Azure Marketplace
U kunt een van de volgende methoden gebruiken om te zoeken en start de installatie van een oplossing.

- Klik op **nu downloaden** op een beheeroplossing voor in de [Azure Marketplace](#find-management-solutions).
- Van de [lijst met oplossingen voor uw abonnement](#list-installed-management-solutions), klikt u op **toevoegen**. Aan de rechterkant van **beheeroplossingen**, klikt u op **meer**. De beheeroplossing die u wilt en klikt u op zoek **maken**.
- Selecteer in de Azure-portal **maken van een resource** > **bewaking + Management** > **alle**. Aan de rechterkant van **beheeroplossingen**, klikt u op **meer**. De beheeroplossing die u wilt en klikt u op zoek **maken**.

Wanneer het installatieproces wordt gestart, wordt u gevraagd om vereiste configuratie die varieert voor elke oplossing. Deze moet u een werkruimte voor logboekanalyse selecteren waar de oplossing moet worden geïnstalleerd en waar de gegevens worden verzameld. U moet mogelijk ook [een Automation-account opgeven](#log-analytics-workspace-and-automation-account) als dit vereist door de oplossing.

### <a name="install-a-solution-from-the-community"></a>Installeren van een oplossing van de community
Leden van de community kunnen beheeroplossingen voor Azure-Snelstartsjablonen verzenden. U kunt deze oplossingen rechtstreeks installeren of downloaden van sjablonen later installeren.

1. Volg de procedure beschreven in [werkruimte voor logboekanalyse en Automation-account](#log-analytics-workspace-and-automation-account) om een werkruimte en de account te koppelen.
2. Ga naar [Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/). 
3. Zoeken naar een oplossing waarmee u geïnteresseerd bent in.
4. Selecteer de oplossing in de resultaten om de details ervan weer te geven.
5. Klik op de **implementeren in Azure** knop.
6. U wordt gevraagd naar informatie zoals de resourcegroep en locatie naast de waarden voor alle parameters in de oplossing.
7. Klik op **aankoop** voor het installeren van de oplossing.


## <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics-werkruimte en de Automation-account
Alle oplossingen vereisen een [werkruimte voor logboekanalyse](../log-analytics/log-analytics-manage-access.md) voor het opslaan van gegevens die door de oplossing verzameld en voor het hosten van de logboekbestanden zoekopdrachten en weergaven. Sommige oplossingen moeten echter ook een [Automation-account](../automation/automation-security-overview.md#automation-account-overview) runbooks en verwante resources bevat. De werkruimte en de account moeten voldoen aan de volgende vereisten.

* Elke installatie van een oplossing kan alleen een werkruimte voor logboekanalyse en een Automation-account gebruiken. U kunt de oplossing afzonderlijk installeren in meerdere werkruimten.
* Als een oplossing een Automation-account vereist, moeten klikt u vervolgens de werkruimte voor logboekanalyse en de Automation-account worden gekoppeld aan elkaar. Een werkruimte voor logboekanalyse kan alleen worden gekoppeld aan een Automation-account en een Automation-account kan alleen worden gekoppeld aan een werkruimte voor logboekanalyse.
* De werkruimte voor logboekanalyse en de Automation-account om te worden gekoppeld, moeten zich in dezelfde resourcegroep en regio. De uitzondering is een werkruimte in de regio VS-Oost en Automation-account in VS-Oost 2.

### <a name="creating-a-link-between-a-log-analytics-workspace-and-automation-account"></a>Maken van een koppeling tussen een werkruimte voor logboekanalyse en een Automation-account
Hoe u de werkruimte voor logboekanalyse en de Automation-account opgeven, is afhankelijk van de installatiemethode voor uw oplossing.

* Wanneer u een oplossing via de Azure Marketplace installeert, wordt u gevraagd om een werkruimte en de Automation-account. De koppeling tussen beide wordt gemaakt als ze niet al is gekoppeld.
* Voor oplossingen buiten Azure Marketplace, moet u de werkruimte voor logboekanalyse en de Automation-account koppelen voordat de installatie van de oplossing. U kunt dit doen door een oplossing in Azure Marketplace selecteren en klikken op de werkruimte voor logboekanalyse en de Automation-account. U moet niet de oplossing daadwerkelijk worden geïnstalleerd omdat de koppeling is gemaakt als de werkruimte voor logboekanalyse en de Automation-account zijn geselecteerd. Zodra de koppeling is gemaakt, kunt u voor een oplossing die werkruimte voor logboekanalyse en de Automation-account gebruiken.

### <a name="verifying-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Verifiëren van de koppeling tussen een werkruimte voor logboekanalyse en een Automation-account
U kunt controleren of de koppeling tussen een werkruimte voor logboekanalyse en een Automation-account met de volgende procedure.

1. Selecteer het Automation-account in de Azure-portal.
1. Schuif naar de **verwante Resources** gedeelte van het menu.
1. Als de **werkruimte** instelling is ingeschakeld, moet dit account is gekoppeld aan een werkruimte voor logboekanalyse. U kunt klikken op **werkruimte** om de details van de werkruimte te bekijken.

## <a name="remove-a-management-solution"></a>Een oplossing voor het beheer verwijderen
Voor het verwijderen van een geïnstalleerde oplossing zoeken in de [lijst met geïnstalleerde oplossingen](#list-installed-management-solutions). Klik op de naam van de oplossing voor het openen van de pagina Samenvatting en klik vervolgens op **verwijderen**.




## <a name="next-steps"></a>Volgende stappen
* Ophalen van een [lijst met oplossingen voor het beheer van Microsoft](monitoring-solutions-inventory.md).
* Meer informatie over hoe [query's maken](../log-analytics/log-analytics-log-searches.md) om gegevens verzameld door oplossingen te analyseren.

