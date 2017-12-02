---
title: Zelfstudie voor multitenant SaaS - Azure SQL Database | Microsoft Docs
description: "Implementeren en een zelfstandige één tenant SaaS-toepassing, die gebruikmaakt van Azure SQL Database verkennen."
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2017
ms.author: genemi
ms.openlocfilehash: d38cd108821bce05824732bbdbdd322ae8563bde
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Implementeren en een zelfstandige één tenant-toepassing die gebruikmaakt van Azure SQL Database verkennen

In deze zelfstudie, implementeren en verkennen Wingtip Tickets SaaS zelfstandige toepassing. De toepassing is ontworpen om hier functies van Azure SQL Database die inschakelen SaaS-scenario's vereenvoudigen.

Het patroon van de zelfstandige toepassing implementeert een Azure-resourcegroep met een één-tenant-toepassing en een database voor één tenant voor elke tenant.  Meerdere exemplaren van de toepassing kunnen worden ingericht om aan te bieden een oplossing voor meerdere tenants.

In deze zelfstudie maakt implementeren u resourcegroepen voor meerdere tenants in uw Azure-abonnement.  Dit patroon kunt de brongroepen worden geïmplementeerd in een tenant Azure-abonnement. Azure heeft de partner-programma's, waardoor deze brongroepen worden beheerd door een serviceprovider namens de tenant. De serviceprovider is een beheerder in de tenant-abonnement.

In het gedeelte implementatie, zijn er drie blauw **implementeren in Azure** knoppen. Elke knop implementeert u een ander exemplaar van de toepassing. Elk exemplaar kan worden aangepast voor een specifieke tenant. Wanneer elke knop wordt ingedrukt, wordt de betreffende toepassing volledig geïmplementeerd binnen vijf minuten.  De apps zijn geïmplementeerd in uw Azure-abonnement.  U hebt volledige toegang om te verkennen en werken met de afzonderlijke toepassingsonderdelen.

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
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Contoso overleg Hall**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Kornoelje Dojo**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Fabrikam Jazz vereniging**

2. Geef de vereiste parameterwaarden voor elke implementatie.

    > [!IMPORTANT]
    > Sommige firewalls verificatie en de server zijn opzettelijk onbeveiligde voor demonstratiedoeleinden. **Maak een nieuwe resourcegroep** voor elke implementatie van de toepassing.  Gebruik een bestaande resourcegroep niet. Gebruik deze toepassing of alle bronnen die wordt gemaakt, niet voor productie. Verwijder alle brongroepen wanneer u klaar met de toepassingen bent te stoppen gerelateerde facturering.

    Het is raadzaam gebruik alleen kleine letters, cijfers en afbreekstreepjes in uw resourcenamen.
    * Voor **resourcegroep** : Selecteer **nieuw**, en geef vervolgens een kleine letter **naam** voor de resourcegroep.
        * Het is raadzaam dat u een streepje, gevolgd door uw initialen, gevolgd door een cijfer toevoegen: bijvoorbeeld *wingtip-sa-af1*.
        * Selecteer een **locatie** uit de vervolgkeuzelijst.

    * Voor **gebruiker** -het is raadzaam dat u een korte waarde, zoals uw initialen plus een cijfer kiezen: bijvoorbeeld *af1*.


3. **Implementeer de toepassing**.

    * Klik om de voorwaarden en bepalingen te accepteren.
    * Klik op **Kopen**.

4. Implementatiestatus van alle drie de implementaties controleren door te klikken op **meldingen** (het belpictogram rechts van het zoekvak). De app implementeren duurt vijf minuten.


## <a name="run-the-application"></a>De toepassing uitvoeren

De app gepresenteerd plaatsen die als host fungeren van gebeurtenissen. U wilt typen zijn concertzalen jazz clubs en sportverenigingen. Plaatsen zijn de klanten van de app Wingtip Tickets. In Wingtip-Tickets plaatsen worden geregistreerd als *tenants*. Een tenant heeft een u wilt een eenvoudige manier aan de lijst met gebeurtenissen en tickets verkopen aan hun klanten. Elke locatie vast Hiermee haalt u een aangepaste website voor een lijst met hun gebeurtenissen en tickets verkopen. Elke tenant wordt geïsoleerd van andere tenants en onafhankelijk is van deze. Achter de opgehaald elke tenant een afzonderlijke toepassing-exemplaar met een eigen zelfstandige SQL-database.

1. Open de pagina gebeurtenissen voor elk van de drie tenants in afzonderlijke browsertabbladen:

    - http://events.contosoconcerthall. &lt;Gebruiker&gt;. trafficmanager.net
    - http://events.dogwooddojo. &lt;Gebruiker&gt;. trafficmanager.net
    - http://events.fabrikamjazzclub. &lt;Gebruiker&gt;. trafficmanager.net

    (In elke URL vervangt &lt;gebruiker&gt; met de waarde van de gebruiker van uw implementatie.)

   ![Gebeurtenissen](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)

Om te bepalen van de distributie van inkomende aanvragen en het gebruik van de app [ *Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md). Elk exemplaar van tenant-specifieke app bevat de naam van de tenant als onderdeel van de domeinnaam in de URL. De tenant URL's opnemen in uw specifieke **gebruiker** waarde. De URL's volgt u de volgende indeling:
- http://events. &lt;venuename&gt;.&lt; GEBRUIKER&gt;. trafficmanager.net

Elke tenant database **locatie** is opgenomen in de app-instellingen van de bijbehorende geïmplementeerde app.

In een productieomgeving, maakt u gewoonlijk een DNS CNAME-record [ *het internetdomein van een bedrijf wijzen* ](../traffic-manager/traffic-manager-point-internet-domain.md) naar de URL van traffic manager-profiel.


## <a name="explore-the-servers-and-tenant-databases"></a>Verken de servers en tenant-databases

Bekijken we enkele van de bronnen die zijn geïmplementeerd:

1. In de [Azure-portal](http://portal.azure.com), blader naar de lijst met resourcegroepen.
2. Zie de **wingtip-sa-catalogus -&lt;gebruiker&gt;**  resourcegroep.
    - In deze resourcegroep de **catalogus-sa -&lt;gebruiker&gt;**  server wordt geïmplementeerd. De server bevat de **tenantcatalog** database.
    - U ziet ook de resourcegroepen drie tenant.
3. Open de **wingtip-sa-fabrikam -&lt;gebruiker&gt;**  resourcegroep, die de resources voor de implementatie van Fabrikam Jazz vereniging bevat.  De **fabrikamjazzclub -&lt;gebruiker&gt;**  server bevat de **fabrikamjazzclub** database.

Elke tenant-database is een 50 DTU *zelfstandige* database.

## <a name="additional-resources"></a>Aanvullende bronnen

<!--
* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
-->

- Zie voor meer informatie over multitenant SaaS-toepassingen, [ontwerppatronen voor multitenant SaaS-toepassingen](saas-tenancy-app-design-patterns.md).


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Klik hier voor meer informatie over het implementeren van de zelfstandige Wingtip Tickets SaaS-toepassing.
> * Over de servers en databases die gezamenlijk de app.
> * Klik hier voor meer informatie over het verwijderen van de voorbeeldresources om te stoppen gerelateerde facturering.

