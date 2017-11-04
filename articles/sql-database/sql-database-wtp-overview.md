---
title: Azure SQL Database multitenant app voorbeeld - Wingtip SaaS | Microsoft Docs
description: Meer informatie over met behulp van een multitenant voorbeeldtoepassing die gebruikmaakt van Azure SQL Database, het Wingtip SaaS-voorbeeld
keywords: zelfstudie sql-database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: sstein
ms.openlocfilehash: 46c9a3eadc2c23959b4d08649c6c0215d44b493e
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="introduction-to-a-sql-database-multi-tenant-saas-app-example"></a>Inleiding tot een voorbeeld van een SQL-Database multitenant SaaS-app

De *Wingtip SaaS* toepassing is een voorbeeld multitenant-app die u laat zien van de unieke voordelen van SQL-Database. Deze SaaS-app maakt gebruik van een toepassingspatroon met een database-per-tenant om zodoende meerdere tenants te kunnen bedienen. De app is ontworpen om de functies van Azure SQL Database waarmee SaaS-scenario's, met inbegrip van verschillende SaaS-ontwerp- en patronen presenteren. Als u snel gebruiksklaar, wordt de Wingtip SaaS-app implementeert in minder dan vijf minuten!

Bron en het beheer toepassingsscripts zijn beschikbaar in de [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-opslagplaats. De scripts uitvoeren [downloaden van de map Learning-Modules](#download-and-unblock-the-wingtip-saas-scripts) op uw lokale computer.

## <a name="sql-database-wingtip-saas-tutorials"></a>SQL Database Wingtip SaaS-zelfstudies

Nadat de app is geïmplementeerd, gebruik de volgende zelfstudies die voort op de eerste implementatie bouwen. Deze zelfstudies verkennen algemene SaaS-patronen die van de ingebouwde functies van SQL-Database, SQL Data Warehouse en andere Azure-services gebruikmaken. Zelfstudies bevatten PowerShell-scripts, met gedetailleerde uitleg die aanzienlijk te vereenvoudigen, begrijpen en implementeren van de dezelfde patronen voor SaaS-beheer in uw toepassingen.


| Zelfstudie | Beschrijving |
|:--|:--|
|[Implementeren en Verken de Wingtip SaaS-toepassing](sql-database-saas-tutorial.md)| **BEGIN HIER!** Implementeer en Verken de Wingtip SaaS-toepassing naar uw Azure-abonnement. |
|[Inrichten en catalogus tenants](sql-database-saas-tutorial-provision-and-catalog.md)| Informatie over hoe de toepassing verbinding maakt met tenants met behulp van een catalogusdatabase, en hoe tenants in de catalogus worden toegewezen aan hun gegevens. |
|[Bewaken en beheren van prestaties](sql-database-saas-tutorial-performance-monitoring.md)| Informatie over het gebruik van de controlemogelijkheden van SQL-Database en het instellen van meldingen wanneer drempelwaarden worden overschreden. |
|[Monitor met logboekanalyse (OMS)](sql-database-saas-tutorial-log-analytics.md) | Meer informatie over het gebruik van [logboekanalyse](../log-analytics/log-analytics-overview.md) voor het bewaken van grote hoeveelheden bronnen, tussen meerdere pools. |
|[Herstellen van een enkele tenant](sql-database-saas-tutorial-restore-single-tenant.md)| Ontdek hoe u een tenant-database naar een eerder tijdstip herstellen. Stappen om een parallelle database, waarbij de bestaande tenant-database online te herstellen, zijn ook opgenomen. |
|[Schema van de tenant beheren](sql-database-saas-tutorial-schema-management.md)| Informatie over het schema bijwerken en referentiegegevens, bijwerken op alle Wingtip SaaS-tenants. |
|[Ad-hoc-analyses uitvoeren](sql-database-saas-tutorial-adhoc-analytics.md) | Maakt de database van een ad-hoc analytics en realtime gedistribueerde query's uitvoeren op alle tenants.  |
|[Tenant-analyses uitvoeren](sql-database-saas-tutorial-tenant-analytics.md) | Tenant-gegevens ophalen naar een analytics database of de data warehouse voor het uitvoeren van offline analytische query's. |



## <a name="application-architecture"></a>Toepassingsarchitectuur

De app Wingtip SaaS maakt gebruik van de database per tenant en elastische pools SQL gebruikt om het te optimaliseren. Voor het inrichten en de toewijzing van tenants tot hun gegevens, wordt de catalogusdatabase van een gebruikt. De kern Wingtip SaaS-toepassing maakt gebruik van een groep met drie voorbeeld tenants, plus de catalogusdatabase. Veel van de Wingtip SaaS zelfstudies tot invoegtoepassingen voor de eerste implementatie leiden is voltooid, door de introductie van analytische databases tussen meerdere databases Schemabeheer, enzovoort.


![Wingtip SaaS-architectuur](media/sql-database-wtp-overview/app-architecture.png)


Tijdens het doorlopen van de zelfstudies en werken met de app, is het belangrijk dat u zich richten op de SaaS-patronen die betrekking heeft op de gegevenslaag. Met andere woorden, kijkt u vooral naar de gegevenslaag en niet zozeer naar de app zelf. Informatie over de implementatie van deze SaaS patronen essentieel is voor het implementeren van deze patronen in uw toepassingen bij het beoordelen van de benodigde wijzigingen voor uw specifieke bedrijfsvereisten.

## <a name="download-and-unblock-the-wingtip-saas-scripts"></a>Downloaden en de scripts Wingtip SaaS deblokkeren

Uitvoerbare inhoud (scripts, dll-bestanden) mogelijk geblokkeerd door Windows als zip-bestanden van een externe bron wordt gedownload en uitgepakt. Bij het uitpakken van de scripts van een zip-bestand ***Volg onderstaande stappen voor het deblokkeren van het ZIP-bestand voor het uitpakken van***. Dit zorgt ervoor dat de scripts mogen worden uitgevoerd.

1. Blader naar [de github-repo Wingtip SaaS](https://github.com/Microsoft/WingtipSaaS).
1. Klik op **klonen of downloaden**.
1. Klik op **ZIP downloaden** en sla het bestand.
1. Met de rechtermuisknop op de **WingtipSaaS master.zip** bestand en selecteer **eigenschappen**.
1. Op de **algemene** tabblad **blokkering**.
1. Klik op **OK**.
1. Pak de bestanden.

Scripts bevinden zich in de *... \\WingtipSaaS master\\Learning-Modules* map.


## <a name="working-with-the-wingtip-saas-powershell-scripts"></a>Werken met de Wingtip SaaS PowerShell-Scripts

Als u optimaal buiten het voorbeeld moet u Duik in de opgegeven scripts. Gebruik onderbrekingspunten en doorloop de scripts, in de details van hoe de andere SaaS-patronen worden geïmplementeerd. Om het eenvoudig stap via de opgegeven scripts en modules voor het beste begrijpen, wordt u aangeraden de [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-the-configuration-file-for-your-deployment"></a>Het configuratiebestand voor uw implementatie bijwerken

Bewerk de **UserConfig.psm1** -bestand met de resource-groep en gebruiker waarde die u hebt ingesteld tijdens de implementatie:

1. Open de *PowerShell ISE* en laden... \\Learning-Modules\\*UserConfig.psm1* 
1. Update *ResourceGroupName* en *naam* met de specifieke waarden voor uw implementatie (op regels 10 en 11 alleen).
1. De wijzigingen opslaan!

Instellen van deze waarden hier simpelweg voorkomt u moet deze implementatie-specifieke waarden in elk script bijwerken.

### <a name="execute-scripts-by-pressing-f5"></a>Voer scripts uit door op F5 te drukken

Gebruik van meerdere scripts *$PSScriptRoot* om mappen te bladeren en *$PSScriptRoot* worden alleen geëvalueerd als scripts worden uitgevoerd door te drukken **F5**.  Syntaxismarkering en een selectie actief (**F8**) kunnen leiden tot fouten, indrukt **F5** wanneer het uitvoeren van scripts.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Doorloop de scripts stapsgewijs om de implementatie te kunnen bekijken

De beste manier om te begrijpen van de scripts is door doorlopen zien wat ze doen. Bekijk de opgenomen **Demo -** scripts die een eenvoudig te volgen werkstroom op hoog niveau. De **Demo -** scripts tonen de stappen die nodig zijn voor elke taak, dus onderbrekingspunten instellen en inzoomen dieper in de afzonderlijke aanroepen voor implementatie-informatie voor de verschillende SaaS-patronen.

Tips voor het verkennen en PowerShell-scripts doorlopen:

* Open **Demo -** scripts in de PowerShell ISE.
* Of Ga door met uitvoeren **F5** (met behulp van **F8** wordt niet aanbevolen omdat *$PSScriptRoot* wordt niet geëvalueerd wanneer selecties van een script wordt uitgevoerd).
* Plaats onderbrekingspunten door op een regel te klikken of een regel te selecteren en op **F9** te drukken.
* Stap over een functie of scriptaanroep heen met **F10**.
* Stap in een functie of scriptaanroep met **F11**.
* Stap uit de huidige functie of scriptaanroep met **Shift + F11**.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Bekijk het databaseschema en voer SQL-query’s uit met SSMS

Gebruik [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) verbinding maken en blader door de toepassingsservers en databases.

De implementatie in eerste instantie heeft twee SQL Database-servers verbinding maken met de - de *tenants1 -&lt;gebruiker&gt;*  -server en de *catalogus -&lt;gebruiker&gt;*  server. Een geslaagde demo-verbinding, zodat beide servers hebben een [firewallregel](sql-database-firewall-configure.md) toestaan via alle IP-adressen.


1. open *SSMS* en maak verbinding met de *tenants1-&lt;User&gt;.database.windows.net*-server.
1. Klik op **Verbinding maken**  > **Database-engine...**:

   ![catalogusserver](media/sql-database-wtp-overview/connect.png)

1. Referenties voor Demo zijn: Gebruikersnaam = *developer*, Wachtwoord = *P@ssword1*

   ![verbinding](media\sql-database-wtp-overview\tenants1-connect.png)

1. Herhaal de stappen 2-3 en maak verbinding met de *catalog-&lt;User&gt;.database.windows.net*-server.

Als de verbinding is geslaagd, ziet u beide servers. Uw lijst met databases mogelijk verschillen, afhankelijk van de tenants die u hebt ingericht:

![objectverkenner](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a>Volgende stappen

[De Wingtip SaaS-toepassing implementeren](sql-database-saas-tutorial.md)
