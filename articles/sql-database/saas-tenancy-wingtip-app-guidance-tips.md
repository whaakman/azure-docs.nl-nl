---
title: Richtlijnen voor het voorbeeld in de SQL-Database-app met meerdere tenants - Wingtip SaaS | Microsoft Docs
description: Biedt instructies en richtlijnen voor het installeren en uitvoeren van meerdere tenants met de voorbeeldtoepassing die gebruikmaakt van Azure SQL Database, de Wingtip Tickets SaaS-voorbeeld.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: 758cb47760f4a15e262a4d682089ac7d9fee64e8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57900502"
---
# <a name="general-guidance-for-working-with-wingtip-tickets-sample-saas-apps"></a>Algemene richtlijnen voor het werken met Wingtip Tickets voorbeeld-SaaS-apps

In dit artikel bevat algemene richtlijnen voor het uitvoeren van de Wingtip Tickets voorbeeld van SaaS-toepassingen die gebruikmaken van Azure SQL Database. 

## <a name="download-and-unblock-the-wingtip-tickets-saas-scripts"></a>Downloaden en de blokkering opheffen van de Wingtip Tickets SaaS-scripts

Uitvoerbare inhoud (scripts, dll's) mogelijk geblokkeerd door Windows als zip-bestanden worden gedownload vanuit een externe bron en uitgepakt. Bij het uitpakken van de scripts in een zip-bestand, **Volg de stappen hieronder om de blokkering van het ZIP-bestand voor het uitpakken van**. Dit zorgt ervoor dat de scripts mogen worden uitgevoerd.

1. Blader naar de Wingtip Tickets SaaS GitHub-opslagplaats voor de database tenants patroon dat u wilt verkennen: 
    - [WingtipTicketsSaaS-StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp)
    - [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant)
    - [WingtipTicketsSaaS-MultiTenantDb](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb)
2. Klik op **klonen of downloaden**.
3. Klik op **zip downloaden** en sla het bestand.
4. Met de rechtermuisknop op het zip-bestand en selecteer **eigenschappen**. De naam van het zip-bestand komt overeen met de naam van de opslagplaats. (ex.) _WingtipTicketsSaaS-DbPerTenant-master.zip_)
5. Op de **algemene** tabblad **opheffen van blokkeringen**.
6. Klik op **OK**.
7. Pak de bestanden.

Scripts bevinden zich in de *... \\Learning Modules* map.


## <a name="working-with-the-wingtip-tickets-powershell-scripts"></a>Werken met de Wingtip Tickets PowerShell-scripts

U haalt het meeste uit het voorbeeld moet u Duik in de geleverde scripts. Gebruik onderbrekingspunten en doorloop de scripts uitvoeren en controleren hoe de verschillende SaaS-patronen worden geïmplementeerd. Om eenvoudig de geleverde scripts en modules voor het beste begrijpen, wordt u aangeraden de [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-the-configuration-file-for-your-deployment"></a>Het configuratiebestand voor uw implementatie bijwerken

Bewerk de **UserConfig.psm1** -bestand met de resource-groep en gebruiker de waarde die u hebt ingesteld tijdens de implementatie:

1. Open de *PowerShell ISE* en laden... \\Learning Modules\\*UserConfig.psm1* 
2. Update *ResourceGroupName* en *naam* met de specifieke waarden voor uw implementatie (op regel 10 en 11 alleen).
3. De wijzigingen niet opslaan.

Instellen van deze waarden hier gewoon zorgt ervoor dat u niet met deze implementatie-specifieke waarden in elk script bijwerken.

### <a name="execute-the-scripts-by-pressing-f5"></a>De scripts uit te voeren door op F5 te drukken

Verschillende scripts maken gebruik *$PSScriptRoot* om te navigeren van mappen, en *$PSScriptRoot* wordt pas geëvalueerd wanneer scripts worden uitgevoerd door te drukken **F5**.  Markeren en uitvoeren van een selectie (**F8**) kan fouten veroorzaken, dus druk op **F5** wanneer het uitvoeren van scripts.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Doorloop de scripts stapsgewijs om de implementatie te kunnen bekijken

De beste manier om te begrijpen van de scripts is door het stapsgewijs zien wat ze doen. Bekijk de opgenomen **Demo -** scripts die een eenvoudige werkstroom op hoog niveau volgen. De **Demo -** scripts tonen de stappen die nodig zijn voor elke taak, dus Stel onderbrekingspunten en inzoomen dieper in de afzonderlijke aanroepen voor implementatie-informatie voor de verschillende SaaS-patronen.

Tips voor het verkennen en PowerShell-scripts doorlopen:

- Open **Demo -** scripts in PowerShell ISE.
- Uitvoeren of doorgaan met **F5** (met behulp van **F8** wordt niet aanbevolen omdat *$PSScriptRoot* wordt niet geëvalueerd wanneer selecties van een script wordt uitgevoerd).
- Plaats onderbrekingspunten door op een regel te klikken of een regel te selecteren en op **F9** te drukken.
- Stap over een functie of scriptaanroep heen met **F10**.
- Stap in een functie of scriptaanroep met **F11**.
- Stap uit de huidige functie of scriptaanroep met **Shift + F11**.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Bekijk het databaseschema en voer SQL-query’s uit met SSMS

Gebruik [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) verbinding kunt maken en blader door de toepassingsservers en -databases.

De implementatie heeft in eerste instantie tenants en SQL Database-catalogusservers kunt koppelen aan. De naamgeving van de servers, is afhankelijk van het patroon voor tenants in de database (Zie hieronder voor meer informatie). 

   - **Zelfstandige toepassing:** servers voor elke tenant (ex.) *contosoconcerthall -&lt;gebruiker&gt;*  server) en *catalogus-sa -&lt;gebruiker&gt;*
   - **Per tenant-database:** *tenants1-dpt -&lt;gebruiker&gt;*  en *catalogus-dpt -&lt;gebruiker&gt;*  servers
   - **Multitenant-database:** *tenants1-mt -&lt;gebruiker&gt;*  en *catalogus-mt -&lt;gebruiker&gt;*  servers

Een geslaagde demo-verbinding, zodat alle servers hebben een [firewallregel](sql-database-firewall-configure.md) zodat alle IP-adressen via.


1. Open *SSMS* en maak verbinding met de tenants. Naam van de server, is afhankelijk van de database tenants patroon dat u hebt geselecteerd (Zie hieronder voor meer informatie):
    - **Zelfstandige toepassing:** servers van afzonderlijke tenants (ex.) *contosoconcerthall-&lt;User&gt;.database.windows.net*) 
    - **Per tenant-database:** *tenants1-dpt -&lt;gebruiker&gt;. database.windows.net*
    - **Multitenant-database:** *tenants1-mt -&lt;gebruiker&gt;. database.windows.net* 
2. Klik op **Verbinding maken**  > **Database-engine...**:

   ![catalogusserver](media/saas-tenancy-wingtip-app-guidance-tips/connect.png)

3. Referenties voor demo zijn: Aanmelding = *developer*, wachtwoord = *P\@ssword1*

    De onderstaande afbeelding ziet u de aanmelding voor de *per tenant-Database* patroon. 
    ![verbinding](media/saas-tenancy-wingtip-app-guidance-tips/tenants1-connect.png)
    
   

4. Herhaal stappen 2-3 en maak verbinding met de catalog-server (Zie hieronder voor specifieke servernamen op basis van de database tenants patroon geselecteerd)
    - **Zelfstandige toepassing:** *catalogus-sa -&lt;gebruiker&gt;. database.windows.net*
    - **Per tenant-database:** *catalogus-dpt -&lt;gebruiker&gt;. database.windows.net*
    - **Multitenant-database:** *catalogus-mt -&lt;gebruiker&gt;. database.windows.net*


De verbinding is geslaagd ziet u alle servers. Uw lijst met databases mogelijk verschillen, afhankelijk van de tenants die u hebt ingericht.

De onderstaande afbeelding ziet u het logboek in voor de *per tenant-Database* patroon.

![objectverkenner](media/saas-tenancy-wingtip-app-guidance-tips/object-explorer.png)



## <a name="next-steps"></a>Volgende stappen
- [De Wingtip Tickets zelfstandige SaaS-toepassing implementeren](saas-standaloneapp-get-started-deploy.md)
- [De Wingtip Tickets SaaS-Database per Tenant-toepassing implementeren](saas-dbpertenant-get-started-deploy.md)
- [De toepassing Wingtip Tickets SaaS multitenant-Database implementeren](saas-multitenantdb-get-started-deploy.md)

