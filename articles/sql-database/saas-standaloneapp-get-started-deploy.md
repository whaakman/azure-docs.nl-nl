---
title: Zelfstudie voor één tenant SaaS - Azure SQL Database | Microsoft Docs
description: Implementeren en een zelfstandige één tenant SaaS-toepassing die gebruikmaakt van Azure SQL Database verkennen.
keywords: zelfstudie sql-database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: genemi
ms.openlocfilehash: 806cea75a9050c5d74f4c6b929d76712e51dd9d1
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/20/2018
ms.locfileid: "46497770"
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Een zelfstandige één tenant-toepassing die gebruikmaakt van Azure SQL Database implementeren en verkennen

In deze zelfstudie, implementeren en verkennen van de Wingtip Tickets SaaS-voorbeeldtoepassing die is ontwikkeld met behulp van het patroon zelfstandige toepassing of app-per-tenant.  De toepassing is ontworpen om u te presenteren functies van Azure SQL Database die het eenvoudiger multitenant SaaS-scenario's inschakelen.

De zelfstandige toepassing of het patroon van de app-per-tenant wordt geïmplementeerd exemplaar van een toepassing voor elke tenant.  Elke toepassing is geconfigureerd voor een specifieke tenant en geïmplementeerd in een afzonderlijke Azure-resourcegroep. Meerdere exemplaren van de toepassing worden om toegang te bieden een oplossing voor meerdere tenants ingericht. Dit patroon is het meest geschikt voor kleinere getallen, van tenants waar de isolatie van tenants een topprioriteit is. Azure heeft de partner-programma's die worden gebruikt om resources te worden geïmplementeerd in van een tenant-abonnement en worden beheerd door een serviceprovider voor andere gebruikers van de tenant. 

In deze zelfstudie implementeert u drie zelfstandige toepassingen voor drie tenants in uw Azure-abonnement.  U hebt volledige toegang om te verkennen en werken met de afzonderlijke toepassingsonderdelen.

De toepassing source code en scripts zijn beschikbaar in de [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) GitHub-opslagplaats.


In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]
> * Hoe u de zelfstandige Wingtip Tickets SaaS-toepassing implementeert.
> * Waar de broncode van de toepassing en scripts voor downloaden.
> * Over de servers en databases waaruit de app.

Aanvullende zelfstudies wordt uitgebracht. Hiermee kunt u een scala aan scenario's voor beheer op basis van dit toepassingspatroon verkennen.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>De Wingtip Tickets zelfstandige SaaS-toepassing implementeren

Implementeer de app voor de drie opgegeven tenants:

1. Klik op elke blauw **implementeren in Azure** knop voor het openen van de sjabloon voor de implementatie in de [Azure-portal](https://portal.azure.com). Elke sjabloon is vereist twee parameterwaarden; een naam op voor een nieuwe resourcegroep en de naam van een gebruiker die deze implementatie van andere implementaties van de app onderscheidt. De volgende stap bevat details voor het instellen van deze waarden.<br><br>
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Contoso Concert Hall**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Dogwood Dojo**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a> &nbsp; **Fabrikam Jazz Club**

2. Voer de vereiste parameterwaarden voor elke implementatie.

    > [!IMPORTANT]
    > Sommige firewalls verificatie en de server zijn opzettelijk onbeveiligde voor demonstratiedoeleinden te gebruiken. **Maak een nieuwe resourcegroep** voor elke implementatie van de toepassing.  Gebruik niet een bestaande resourcegroep. Gebruik deze toepassing, of eventuele resources die zijn gemaakt, niet voor productie. Alle resourcegroepen verwijderen wanneer u klaar bent met de toepassingen gerelateerde facturering te stoppen.

    Het is raadzaam gebruik alleen kleine letters, cijfers en afbreekstreepjes bevatten in de resourcenamen van uw.
    * Voor **resourcegroep**, selecteer de nieuwe optie maken en geef vervolgens een kleine naam voor de resourcegroep. **Wingtip-sa -\<venueName\>-\<gebruiker\>**  is het aanbevolen patroon.  Voor \<venueName\>, vervangen door de naam van de venue zonder spaties. Voor \<gebruiker\>, vervang de gebruikerswaarde hieronder.  Met dit patroon resourcegroepnamen mogelijk *wingtip-sa-contosoconcerthall-af1*, *wingtip-sa-dogwooddojo-af1*, *wingtip-sa-fabrikamjazzclub-af1*.
    * Selecteer een **locatie** uit de vervolgkeuzelijst.

    * Voor **gebruiker** -wordt aangeraden een korte gebruikerswaarde, zoals uw initialen plus een cijfer: bijvoorbeeld *af1*.


3. **Implementeer de toepassing**.

    * Klik op om de voorwaarden en bepalingen te accepteren.
    * Klik op **Kopen**.

4. De status van alle drie implementaties controleren door te klikken op **meldingen** (het belpictogram aan de rechterkant van het zoekvak). Implementatie van de apps duurt ongeveer vijf minuten.


## <a name="run-the-applications"></a>De toepassingen uitvoeren

De app worden venues die als host gebeurtenissen fungeren gepresenteerd.  De venues zijn de tenants van de toepassing. Elke venue krijgt een aangepaste website te vermelden van hun evenementen en tickets kunnen verkopen. Locatietypen bevatten concertzalen, jazzclubs stadions en sport stadions. In het voorbeeld bepaalt het type venue de achtergrondfoto wordt weergegeven op de website van de venue.   In de zelfstandige app-model heeft elke locatie een afzonderlijke toepassing-exemplaar met een eigen zelfstandige SQL-database.

1. Open de pagina gebeurtenissen voor elk van de drie tenants in afzonderlijke browsertabbladen:

    - http://events.contosoconcerthall.&lt; gebruiker&gt;. trafficmanager.net
    - http://events.dogwooddojo.&lt; gebruiker&gt;. trafficmanager.net
    - http://events.fabrikamjazzclub.&lt; gebruiker&gt;. trafficmanager.net

    (In elke URL vervangen &lt;gebruiker&gt; met de waarde van de gebruiker van uw implementatie.)

   ![Gebeurtenissen](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)

Voor het beheren van de distributie van inkomende aanvragen, de app gebruikt [ *Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md). Elke tenant-specifieke app-instantie bevat de naam van de tenant als onderdeel van de domeinnaam in de URL. Alle tenant URL's bevatten uw specifieke **gebruiker** waarde. De URL volgt u de volgende indeling:
- http://events.&lt; venuename&gt;.&lt; gebruiker&gt;. trafficmanager.net

Van elke tenantdatabase **locatie** is opgenomen in de app-instellingen van de bijbehorende geïmplementeerde app.

In een productieomgeving, doorgaans maakt u een CNAME-DNS-record [ *internetdomein van een bedrijf* ](../traffic-manager/traffic-manager-point-internet-domain.md) naar de URL van het traffic manager-profiel.


## <a name="explore-the-servers-and-tenant-databases"></a>Verken de servers en tenant-databases

Laten we kijken enkele van de resources die zijn geïmplementeerd:

1. In de [Azure-portal](http://portal.azure.com), blader naar de lijst met resourcegroepen.
2. Hier ziet u de resourcegroepen drie tenant.
3. Open de **wingtip-sa-fabrikam -&lt;gebruiker&gt;**  resourcegroep, die de resources voor de implementatie van Fabrikam Jazz Club bevat.  De **fabrikamjazzclub -&lt;gebruiker&gt;**  server bevat de **fabrikamjazzclub** database.

Elke tenant-database is een 50 DTU *zelfstandige* database.

## <a name="additional-resources"></a>Aanvullende resources

<!--
* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
-->

- Zie voor meer informatie over SaaS-toepassingen met meerdere tenants, [ontwerppatronen voor SaaS-toepassingen met meerdere tenants](saas-tenancy-app-design-patterns.md).

 
## <a name="delete-resource-groups-to-stop-billing"></a>Resourcegroepen om te stoppen facturering verwijderen ##

Wanneer u klaar bent met het voorbeeld gebruikt, verwijdert alle resourcegroepen die u hebt gemaakt om de bijbehorende facturering te stoppen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Hoe u de zelfstandige Wingtip Tickets SaaS-toepassing implementeert.
> * Over de servers en databases waaruit de app.
> * Klik hier voor meer informatie over het verwijderen van de voorbeeldresources om gerelateerde facturering te stoppen.

Probeer vervolgens de [inrichten en catalogiseren](saas-standaloneapp-provision-and-catalog.md) zelfstudie waarin u het gebruik van een catalogus van tenants waarmee een scala aan scenario's voor cross-tenant, zoals schema management en tenant-analytics verkennen.
 

