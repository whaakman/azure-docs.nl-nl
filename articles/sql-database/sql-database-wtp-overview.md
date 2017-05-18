---
title: SaaS als pakket (voorbeeld van een SaaS-toepassing met Azure SQL Database) | Microsoft-documenten
description: Ontwikkel SaaS-toepassingen met SQL Database
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: bf5745a788cd9ab6bf2ea8d5d97b8c04f083fc5d
ms.contentlocale: nl-nl
ms.lasthandoff: 05/10/2017


---
# <a name="introduction-to-the-wingtip-tickets-platform-wtp-sample-saas-application"></a>Inleiding tot Wingtip Tickets Platform (WTP), een voorbeeld van een SaaS-toepassing

De SaaS-toepassing Wingtip Tickets Platform (WTP) is een voorbeeld van een multitenant-toepassing die de unieke voordelen van SQL Database duidelijk maakt. Deze SaaS-app maakt gebruik van een toepassingspatroon met een database-per-tenant om zodoende meerdere tenants te kunnen bedienen. De WTP-app is ontworpen om de functies van Azure SQL Database die SaaS-scenario’s ondersteunen, optimaal onder de aandacht te brengen, zoals ontwerp- en -beheerpatronen voor SaaS. U kunt heel snel aan de slag: [de WTP-app kan in minder dan vijf minuten worden geïmplementeerd](sql-database-saas-tutorial.md)!

Zodra de WTP-app is geïmplementeerd, kunt u de [verzameling zelfstudies](#sql-database-saas-tutorials) bekijken die dienen als vervolg op de eerste implementatie. Elke zelfstudie is gericht op veelgebruikte taken die worden geïmplementeerd in SaaS-toepassingen. Taken worden geïmplementeerd volgens SaaS-patronen die gebruikmaken van de krachtige ingebouwde functies van SQL Database. De omschreven patronen omvatten het inrichten van nieuwe tenants, het herstellen van tenantdatabases, het uitvoeren van gedistribueerde query’s bij alle tenants en het uitrollen van schemawijzigingen in alle tenantdatabases. In elke zelfstudie vindt u herbruikbare scripts met gedetailleerde en begrijpelijke uitleg, waarmee u dezelfde SaaS-beheerpatronen in uw toepassingen kunt implementeren.

Hoewel de WTP-toepassing best wel volledig en nuttig is als voorbeeldtoepassing, is de bedoeling ervan meer het bestuderen van de principes van de SaaS-patronen en hun relatie met de gegevenslaag. Met andere woorden, kijkt u vooral naar de gegevenslaag en niet zozeer naar de app zelf. Als u begrijpt hoe deze principiële SaaS-patronen worden geïmplementeerd, helpt dat enorm bij het implementeren van deze patronen in uw eigen toepassingen. Tegelijk moet u natuurlijk overwegen welke wijzigingen nodig zijn voor de specifieke vereisten van uw bedrijf.



## <a name="application-architecture"></a>Toepassingsarchitectuur

De WTP-app maakt gebruik van het database-per-tenant-model en van elastische SQL-pools voor een maximale efficiency.
Gebruik van een tenantcatalogus voor beheer van de inrichting en connectiviteit.
Geïntegreerde app, pool, databasecontrole en waarschuwingen (OMS).
Schema- en referentiegegevensbeheer voor alle tenants (elastische databasetaken).
Query’s, operationele analytics voor alle tenants (elastische query).
Gebruik van geografisch gedistribueerde gegevens voor een wijder bereik.
Herstel van één tenant voor bedrijfscontinuïteit (PITR) DR op schaal (geo-herstel, geo-replicatie, auto-DR) self-servicebeheer voor tenants (via beheer-API’s) PITR voor herstel van zelfveroorzaakte fouten.

De Wingtip-toepassing zelf maakt gebruik van drie voorbeeldtenants en een catalogusdatabase.

![WTP-architectuur](media/sql-database-wtp-overview/wtp-architecture.png)


## <a name="sql-database-wtp-saas-tutorials"></a>SQL Database WTP SaaS-zelfstudies

De volgende zelfstudies bouwen voort op de eerste implementatie van de [SaaS-voorbeeldtoepassing Wingtip Tickets Platform](sql-database-saas-tutorial.md):

| Onderwerp | Beschrijving | Scriptlocatie |
|:--|:--|:--|
|[Zelfstudie Tenants inrichten en catalogiseren](sql-database-saas-tutorial-provision-and-catalog.md)| Nieuwe tenants inrichten en in de catalogus registreren | [Scripts op github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Provision%20and%20Catalog) |
|[Zelfstudie Prestaties controleren en beheren](sql-database-saas-tutorial-performance-monitoring.md)| De prestaties van database en pool controleren en beheren | [Scripts op github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Performance%20Monitoring%20and%20Management) |
|[Zelfstudie Een individuele tenant terugzetten](sql-database-saas-tutorial-restore-single-tenant.md)| Tenantdatabases terugzetten | [Scripts op github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Business%20Continuity%20and%20Disaster%20Recovery/RestoreTenant) |
|[Zelfstudie tenantschema’s beheren](sql-database-saas-tutorial-schema-management.md)| Query’s uitvoeren op alle tenants  | [Scripts op github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Schema%20Management) |
|[Zelfstudie Ad-hoc-analyses uitvoeren](sql-database-saas-tutorial-adhoc-analytics.md) | Een ad-hoc analytics-database maken en query’s uitvoeren op alle tenants  | [Scripts op github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Operational%20Analytics/Adhoc%20Analytics) |
|[Zelfstudie Beheer met Log Analytics (OMS)](sql-database-saas-tutorial-log-analytics.md) | Log Analytics configureren en bekijken | [Scripts op github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Performance%20Monitoring%20and%20Management/LogAnalytics) |
|[Zelfstudie Tenant-analytics uitvoeren](sql-database-saas-tutorial-tenant-analytics.md) | Query’s voor tenant-analytics instellen en uitvoeren | [Scripts op github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Operational%20Analytics/Tenant%20Analytics) |

## <a name="get-the-wingtip-application-scripts"></a>De scripts van de Wingtip-toepassing downloaden

De scripts en broncode van de toepassing Wingtip Tickets zijn beschikbaar in de [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-repo. U vindt de scriptbestanden in de map [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Download de map **Learning Modules** naar uw lokale computer en behoud de mapstructuur.

## <a name="working-with-the-wtp-powershell-scripts"></a>Werken met de WTP PowerShell-scripts

Werken met de WTP-toepassing biedt vooral als voordeel dat u meteen aan de slag kunt met de geleverde scripts en goed kunt zien hoe de verschillende SaaS-patronen zijn geïmplementeerd.

Gebruik de [Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise) om de ter beschikking gestelde scripts en modules te bekijken en stapsgewijs te doorlopen zodat u ze beter zult begrijpen. Omdat de meeste scripts met het voorvoegsel *Demo-* variabelen bevatten die u kunt aanpassen voordat u het script uitvoert, is het handig om PowerShell ISE te gebruiken voor het werken met deze scripts.

Voor elke implementatie van WTP is er een bestand **UserConfig.psm1** met daarin twee parameters voor het instellen van de waarden voor de resourcegroep en de gebruikersnaam die u tijdens de implementatie hebt gedefinieerd. Zodra de implementatie is voltooid, bewerkt u de module **UserConfig.psm1** door de parameters voor _ResourceGroupName_ en _Name_ in te stellen. Deze waarden zijn ook nodig voor een goede uitvoering van andere scripts, dus het wordt aanbevolen om deze waarden in te stellen zodra de implementatie is voltooid!



### <a name="execute-scripts-by-pressing-f5"></a>Voer scripts uit door op F5 te drukken

Verschillende scripts maken gebruik van *$PSScriptRoot* om door mappen te navigeren en deze variabele wordt pas geëvalueerd wanneer het script wordt uitgevoerd, dus nadat er op **F5** is gedrukt.  Het markeren en uitvoeren van een selectie (**F8**) kan fouten veroorzaken, dus druk op **F5** wanneer u WTP-scripts wilt uitvoeren.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Doorloop de scripts stapsgewijs om de implementatie te kunnen bekijken

De meeste waarde van het bekijken van scripts haalt u uit het stapsgewijs doorlopen van de scripts. Zo kunt u precies zien wat ze doen. Kijk eens naar de _Demo-_scripts op het eerste niveau. Deze bieden een gemakkelijk leesbare globale werkstroom inclusief de stappen die nodig zijn om elke taak te volbrengen. Bekijk de afzonderlijke aanroepen in meer detail om de implementatie voor de verschillende SaaS-patronen nader te bekijken.

Tips voor het werken met en [opsporen van fouten in PowerShell-scripts](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise):

* Open en configureer demo-scripts in de PowerShell ISE.
* Voer scripts uit of hervat ze met **F5**. Het gebruik van **F8** wordt afgeraden, omdat *$PSScriptRoot* niet wordt geëvalueerd wanneer slechts selecties van een script worden uitgevoerd.
* Plaats onderbrekingspunten door op een regel te klikken of een regel te selecteren en op **F9** te drukken.
* Stap over een functie of scriptaanroep heen met **F10**.
* Stap in een functie of scriptaanroep met **F11**.
* Stap uit de huidige functie of scriptaanroep met **Shift + F11**.




## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Bekijk het databaseschema en voer SQL-query’s uit met SSMS

Gebruik [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) om verbinding te maken en te bladeren in de WTP-servers en databases.

De WTP-voorbeeldapp heeft aanvankelijk twee SQL Database-servers waarmee u verbinding kunt maken: de *tenants1*-server en de *catalogus*server:


1. open *SSMS* en maak verbinding met de *tenants1-&lt;User&gt;.database.windows.net*-server.
2. Klik op **Verbinding maken**  > **Database-engine...**:

   ![catalogusserver](media/sql-database-wtp-overview/connect.png)

1. Referenties voor Demo zijn: Gebruikersnaam = *developer*, Wachtwoord = *P@ssword1*

   ![verbinding](media\sql-database-wtp-overview\tenants1-connect.png)

1. Herhaal de stappen 2-3 en maak verbinding met de *catalog-&lt;User&gt;.database.windows.net*-server.

Als de verbinding is geslaagd, ziet u beide servers. U ziet mogelijk meer of minder databases, afhankelijk van hoeveel tenants u hebt ingericht:

![objectverkenner](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a>Volgende stappen

[Wingtip Tickets implementeren, het voorbeeld van een SaaS-app](sql-database-saas-tutorial.md)
