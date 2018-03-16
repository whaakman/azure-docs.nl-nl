---
title: Zelfstudie voor multitenant SaaS - Azure SQL Database | Microsoft Docs
description: "Implementeren en een zelfstandige één tenant SaaS-toepassing, die gebruikmaakt van Azure SQL Database verkennen."
keywords: zelfstudie sql-database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 11/30/2017
ms.author: genemi
ms.openlocfilehash: e30f096a9f02214839550c2dc143ab665e1cd85c
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Implementeren en een zelfstandige één tenant-toepassing die gebruikmaakt van Azure SQL Database verkennen

In deze zelfstudie, implementeren en Verken de Wingtip Tickets SaaS-voorbeeldtoepassing die is ontwikkeld met behulp van de zelfstandige toepassing of de app per tenant-patroon.  De toepassing is ontworpen om hier functies van Azure SQL Database die u inschakelt multitenant SaaS-scenario's vereenvoudigen.

De zelfstandige toepassing of het patroon van de app per tenant implementeert een toepassingsexemplaar voor elke tenant.  Elke toepassing is geconfigureerd voor een specifieke tenant en geïmplementeerd in een afzonderlijke Azure-resourcegroep. Meerdere exemplaren van de toepassing ingericht voor bieden een oplossing voor meerdere tenants. Dit patroon is het meest geschikt voor kleinere aantallen tenants waarbij isolatie van tenants een topprioriteit is. Azure heeft partner programma's die toestaan dat resources kunnen worden geïmplementeerd in een tenant-abonnement en worden beheerd door een serviceprovider namens de tenant. 

In deze zelfstudie implementeert u drie zelfstandige toepassingen voor drie tenants in uw Azure-abonnement.  U hebt volledige toegang om te verkennen en werken met de afzonderlijke toepassingsonderdelen.

De bron en het beheer toepassingsscripts zijn beschikbaar in de [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) GitHub-opslagplaats.


In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]
> * Klik hier voor meer informatie over het implementeren van de zelfstandige Wingtip Tickets SaaS-toepassing.
> * Waar u de broncode van de toepassing en beheerscripts schrijven.
> * Over de servers en databases die gezamenlijk de app.

Aanvullende zelfstudies worden uitgebracht. Hiermee kunt u een scala aan scenario's voor beheer op basis van dit patroon toepassing verkennen.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>De Wingtip Tickets zelfstandige SaaS-toepassing implementeren

Implementeer de app voor de drie opgegeven tenants:

1. Klik op elke blauw **implementeren in Azure** knop voor de implementatiesjabloon in de [Azure-portal](https://portal.azure.com). Elke sjabloon vereist twee parameterwaarden; een naam voor een nieuwe resourcegroep en de naam van een gebruiker die deze implementatie van andere implementaties van de app onderscheidt. De volgende stap biedt details voor het instellen van deze waarden.<br><br>
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Contoso overleg Hall**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Dogwood Dojo**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Fabrikam Jazz Club**

2. Geef de vereiste parameterwaarden voor elke implementatie.

    > [!IMPORTANT]
    > Sommige firewalls verificatie en de server zijn opzettelijk onbeveiligde voor demonstratiedoeleinden. **Maak een nieuwe resourcegroep** voor elke implementatie van de toepassing.  Gebruik een bestaande resourcegroep niet. Gebruik deze toepassing of alle bronnen die wordt gemaakt, niet voor productie. Verwijder alle brongroepen wanneer u klaar met de toepassingen bent te stoppen gerelateerde facturering.

    Het is raadzaam gebruik alleen kleine letters, cijfers en afbreekstreepjes in uw resourcenamen.
    * Voor **resourcegroep**, selecteer maken nieuw en geef een naam op voor de resourcegroep in kleine letters. **Wingtip-sa -\<venueName\>-\<gebruiker\>**  is het aanbevolen patroon.  Voor \<venueName\>, vervangen door de naam wilt zonder spaties. Voor \<gebruiker\>, vervang de waarde van de gebruiker uit het volgende.  Met dit patroon worden de namen van resourcegroepen mogelijk *wingtip sa-contosoconcerthall af1*, *wingtip sa-dogwooddojo af1*, *wingtip sa-fabrikamjazzclub af1*.
    * Selecteer een **locatie** uit de vervolgkeuzelijst.

    * Voor **gebruiker** -wordt aangeraden een korte gebruiker-waarde, zoals uw initialen plus een cijfer: bijvoorbeeld *af1*.


3. **Implementeer de toepassing**.

    * Klik om de voorwaarden en bepalingen te accepteren.
    * Klik op **Kopen**.

4. De status van alle drie de implementaties controleren door te klikken op **meldingen** (het belpictogram rechts van het zoekvak). De apps te implementeren, duurt het ongeveer vijf minuten is.


## <a name="run-the-applications"></a>De toepassingen uitvoeren

De app gepresenteerd plaatsen die als host fungeren van gebeurtenissen.  De plaatsen zijn de tenants van de toepassing. Elke locatie vast Hiermee haalt u een aangepaste website lijst met hun gebeurtenissen en tickets verkopen. U wilt typen zijn concertzalen jazz clubs en sportverenigingen. In het voorbeeld wilt is afhankelijk van de achtergrondfoto weergegeven op de plaats-website.   In het zelfstandige app-model heeft elke u wilt een afzonderlijke toepassing-exemplaar met een eigen zelfstandige SQL-database.

1. Open de pagina gebeurtenissen voor elk van de drie tenants in afzonderlijke browsertabbladen:

    - http://events.contosoconcerthall.&lt;user&gt;.trafficmanager.net
    - http://events.dogwooddojo.&lt;user&gt;.trafficmanager.net
    - http://events.fabrikamjazzclub.&lt;user&gt;.trafficmanager.net

    (In elke URL vervangt &lt;gebruiker&gt; met de waarde van de gebruiker van uw implementatie.)

   ![Gebeurtenissen](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)

Om te bepalen van de distributie van inkomende aanvragen en het gebruik van de app [ *Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md). Elk exemplaar van tenant-specifieke app bevat de naam van de tenant als onderdeel van de domeinnaam in de URL. De tenant URL's opnemen in uw specifieke **gebruiker** waarde. De URL's volgt u de volgende indeling:
- http://events.&lt;venuename&gt;.&lt;user&gt;.trafficmanager.net

Elke tenant database **locatie** is opgenomen in de app-instellingen van de bijbehorende geïmplementeerde app.

In een productieomgeving, maakt u gewoonlijk een DNS CNAME-record [ *het internetdomein van een bedrijf wijzen* ](../traffic-manager/traffic-manager-point-internet-domain.md) naar de URL van traffic manager-profiel.


## <a name="explore-the-servers-and-tenant-databases"></a>Verken de servers en tenant-databases

Bekijken we enkele van de bronnen die zijn geïmplementeerd:

1. In de [Azure-portal](http://portal.azure.com), blader naar de lijst met resourcegroepen.
2. U ziet de resourcegroepen drie tenant.
3. Open de **wingtip-sa-fabrikam -&lt;gebruiker&gt;**  resourcegroep, die de resources voor de implementatie van Fabrikam Jazz vereniging bevat.  De **fabrikamjazzclub -&lt;gebruiker&gt;**  server bevat de **fabrikamjazzclub** database.

Elke tenant-database is een 50 DTU *zelfstandige* database.

## <a name="additional-resources"></a>Aanvullende resources

<!--
* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
-->

- Zie voor meer informatie over multitenant SaaS-toepassingen, [ontwerppatronen voor multitenant SaaS-toepassingen](saas-tenancy-app-design-patterns.md).

 
## <a name="delete-resource-groups-to-stop-billing"></a>Resourcegroepen gebruiken om te stoppen facturering verwijderen ##

Wanneer u klaar bent met het voorbeeld gebruikt, verwijdert u alle resourcegroepen die u hebt gemaakt om de bijbehorende facturering stoppen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Klik hier voor meer informatie over het implementeren van de zelfstandige Wingtip Tickets SaaS-toepassing.
> * Over de servers en databases die gezamenlijk de app.
> * Klik hier voor meer informatie over het verwijderen van de voorbeeldresources om te stoppen gerelateerde facturering.

Probeer vervolgens de [inrichten en catalogus](saas-standaloneapp-provision-and-catalog.md) zelfstudie waarin u het gebruik van een catalogus met tenants waarmee een reeks cross-tenant-scenario's zoals schema beheer- en tenantverkeer analytics verkennen.
 

