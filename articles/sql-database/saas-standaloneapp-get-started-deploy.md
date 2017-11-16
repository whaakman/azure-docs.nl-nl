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
ms.date: 11/14/2017
ms.author: sstein
ms.openlocfilehash: 2ca290dfcb23215ffd727500e76076ae8b14fa6f
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Implementeren en een zelfstandige één tenant-toepassing die gebruikmaakt van Azure SQL Database verkennen

In deze zelfstudie, implementeren en verkennen Wingtip Tickets SaaS zelfstandige toepassing. De toepassing is ontworpen om hier functies van Azure SQL Database die inschakelen SaaS-scenario's vereenvoudigen.

Het patroon van de zelfstandige toepassing implementeert een Azure-resourcegroep met een één-tenant-toepassing en een database voor één tenant voor elke tenant.  Meerdere exemplaren van de toepassing kunnen worden ingericht om aan te bieden een oplossing voor meerdere tenants.

Hoewel in deze zelfstudie u resourcegroepen voor meerdere tenants in uw Azure-abonnement implementeert, kan dit patroon de brongroepen worden geïmplementeerd in een tenant Azure-abonnement.  Azure heeft de partner-programma's, waardoor deze brongroepen worden beheerd door een serviceprovider namens de tenant als een beheerder in de tenant-abonnement.

In het onderstaande gedeelte voor implementatie zijn er drie implementeren in Azure knoppen, elk met een ander exemplaar van de toepassing worden aangepast voor een specifieke tenant implementeert. Wanneer elke knop wordt ingedrukt wordt de betreffende toepassing volledig vijf minuten later geïmplementeerd.  De apps zijn geïmplementeerd in uw Azure-abonnement.  U hebt volledige toegang om te verkennen en werken met de afzonderlijke toepassingsonderdelen.

De bron en het beheer toepassingsscripts zijn beschikbaar in de [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) GitHub-opslagplaats.


In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]

> * De zelfstandige Wingtip Tickets SaaS-toepassing implementeren
> * Waar de broncode van de toepassing en scripts voor downloaden
> * Over de servers en databases die gezamenlijk de app.

Aanvullende zelfstudies worden uitgebracht zijner waarmee u wilt experimenteren met een aantal scenario's voor beheer op basis van dit patroon van een toepassing.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>De Wingtip Tickets zelfstandige SaaS-toepassing implementeren

Implementeer de app voor de drie opgegeven tenants:

1. Klik op elk **implementeren in Azure** te openen van de sjabloon voor de implementatie in de Azure portal. Elke sjabloon vereist twee parameterwaarden; een naam voor een nieuwe resourcegroep en de naam van een gebruiker die deze implementatie van andere implementaties van de app onderscheidt. De volgende stap biedt details voor het instellen van deze waarden.<br><br>
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>     &nbsp;&nbsp;**Contoso overleg Hall**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; &nbsp; **Kornoelje Dojo**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>    &nbsp;&nbsp;**Fabrikam Jazz vereniging**

2. Geef de vereiste parameterwaarden voor elke implementatie.

    > [!IMPORTANT]
    > Om het overzichtelijk te houden zijn bepaalde verificatieprocessen weggelaten. Ook zijn de firewalls op servers uitgeschakeld voor deze zelfstudie. **Maak een nieuwe resourcegroep** voor elke implementatie van de toepassing.  Gebruik een bestaande resourcegroep niet. Gebruik deze toepassing of alle bronnen die wordt gemaakt, niet voor productie. Verwijder alle brongroepen wanneer u klaar met de toepassingen bent te stoppen gerelateerde facturering.

    Het is raadzaam gebruik alleen kleine letters, cijfers en afbreekstreepjes in uw resourcenamen.
    * Voor **resourcegroep** - Selecteer Nieuw en geef een naam op voor de resourcegroep (hoofdlettergevoelig).
        * Het is raadzaam dat alle letters in de naam van de resourcegroep kleine letters zijn.
        * Het is raadzaam dat u een streepje, gevolgd door uw initialen, gevolgd door een cijfer toevoegen: bijvoorbeeld _wingtip-sa-af1_.
        * Selecteer een locatie in de vervolgkeuzelijst.

    * Voor **gebruiker** -het is raadzaam dat u een korte waarde van de gebruiker, zoals uw initialen plus een cijfer kiezen: bijvoorbeeld _af1_.


1. **Implementeer de toepassing**.

    * Klik om de voorwaarden en bepalingen te accepteren.
    * Klik op **Kopen**.

1. Implementatiestatus van alle drie de implementaties controleren door te klikken op **meldingen** (het belpictogram rechts van het zoekvak). De app implementeren duurt ongeveer vijf minuten.


## <a name="run-the-application"></a>De toepassing uitvoeren

De app gepresenteerd plaatsen, zoals concertzalen jazz clubs en sportverenigingen die als host fungeren van gebeurtenissen. Plaatsen registreren als klanten (of tenants) van Wingtip ticket, voor een eenvoudige manier om een lijst van gebeurtenissen en tickets verkopen. Elke locatie vast Hiermee haalt u een aangepaste website beheren en weergeven van de gebeurtenissen en verkopen tickets, onafhankelijke en geïsoleerd van andere tenants. Achter de krijgt elke tenant een exemplaar van de afzonderlijke en zelfstandige SQL-database.

1. Open de pagina gebeurtenissen voor elk van de drie tenants in afzonderlijke browsertabbladen:

    http://events.contosoconcerthall. &lt;Gebruiker&gt;. trafficmanager.net <br>
    http://events.dogwooddojo. &lt;Gebruiker&gt;. trafficmanager.net<br>
    http://events.fabrikamjazzclub. &lt;Gebruiker&gt;. trafficmanager.net

    (Vervang &lt;gebruiker&gt; met uw implementatie gebruiker waarde).

   ![Gebeurtenissen](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)


Om te bepalen van de distributie van inkomende aanvragen en het gebruik van de app [ *Azure Traffic Manager*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview). Elke app tenantspecifieke bevat de naam van de tenant als onderdeel van de domeinnaam in de URL. De tenant URL's opnemen in uw specifieke *gebruiker* waarde en volgt deze indeling: http://events.&lt; venuename&gt;.&lt; GEBRUIKER&gt;. trafficmanager.net. De locatie van de database van elke tenant is opgenomen in de app-instellingen van de bijbehorende geïmplementeerde app.

In een productieomgeving zou u meestal een DNS CNAME-record te maken [ *het internetdomein van een bedrijf wijzen* ](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) naar de URL van traffic manager-profiel.


## <a name="explore-the-servers-and-tenant-databases"></a>Verken de servers en tenant-databases

Bekijken we enkele van de bronnen die zijn geïmplementeerd:

1. In de [Azure-portal](http://portal.azure.com), blader naar de lijst met resourcegroepen.  U ziet de **wingtip-sa-catalogus -&lt;gebruiker&gt;**  resourcegroep waarin **catalogus-sa -&lt;gebruiker&gt;**  met de wordtgeïmplementeerd**tenantcatalog** database. U ziet ook de resourcegroepen drie tenant.

1. Open de **wingtip-sa-fabrikam -&lt;gebruiker&gt;**  resourcegroep, die de resources voor de implementatie van Fabrikam Jazz vereniging bevat.  De **fabrikamjazzclub -&lt;gebruiker&gt;**  server bevat de **fabrikamjazzclub** database.


Elke tenant-database is een 50 DTU _zelfstandige_ database.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * De zelfstandige Wingtip Tickets SaaS-toepassing implementeren
> * Over de servers en databases die gezamenlijk de app.
> * Voorbeeldresources verwijderen om gerelateerde facturering te stoppen


## <a name="additional-resources"></a>Aanvullende bronnen

<!--* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview) -->
* Informatie over SaaS-toepassingen voor meerdere tenants vindt u in [*Design patterns for multi-tenant SaaS applications*](saas-tenancy-app-design-patterns.md) (Ontwerppatronen voor SaaS-toepassingen voor meerdere tenants).
