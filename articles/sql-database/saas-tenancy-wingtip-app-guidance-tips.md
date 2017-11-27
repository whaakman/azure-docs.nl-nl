---
title: Richtlijnen voor het voorbeeld in de SQL-Database-multitenant-app - Wingtip SaaS | Microsoft Docs
description: Bevat de stappen en richtlijnen voor het installeren en uitvoeren van meerdere tenants met de voorbeeldtoepassing die gebruikmaakt van Azure SQL Database, het Wingtip Tickets SaaS-voorbeeld.
keywords: zelfstudie sql-database
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2017
ms.author: genemi
ms.openlocfilehash: ebf1c4948816746fa02b867d1ea66afc64b882b0
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="general-guidance-for-working-with-wingtip-tickets-sample-saas-apps"></a>Algemene richtlijnen voor het werken met Wingtip Tickets steekproef SaaS-apps

Dit artikel bevat algemene richtlijnen voor het uitvoeren van de Wingtip Tickets voorbeeld SaaS-toepassingen die gebruikmaken van Azure SQL Database. 

## <a name="download-and-unblock-the-wingtip-tickets-saas-scripts"></a>Downloaden en de scripts Wingtip Tickets SaaS deblokkeren

Uitvoerbare inhoud (scripts, dll-bestanden) mogelijk geblokkeerd door Windows als zip-bestanden van een externe bron wordt gedownload en uitgepakt. Bij het uitpakken van de scripts van een zip-bestand **Volg onderstaande stappen voor het deblokkeren van het ZIP-bestand voor het uitpakken van**. Dit zorgt ervoor dat de scripts mogen worden uitgevoerd.

1. Blader naar de Wingtip Tickets SaaS GitHub-repo voor de database tenancymodus patroon dat u wilt verkennen: 
    - [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp)
    - [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant)
    - [WingtipTicketsSaaS MultiTenantDb](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb)
2. Klik op **klonen of downloaden**.
3. Klik op **zip downloaden** en sla het bestand.
4. Met de rechtermuisknop op het zip-bestand en selecteer **eigenschappen**. De naam van het zip-bestand komt overeen met de naam van de opslagplaats. (ex. _WingtipTicketsSaaS-DbPerTenant-master.zip_)
5. Op de **algemene** tabblad **blokkering**.
6. Klik op **OK**.
7. Pak de bestanden.

Scripts bevinden zich in de *... \\Learning-Modules* map.


## <a name="working-with-the-wingtip-tickets-powershell-scripts"></a>Werken met de Wingtip Tickets PowerShell-scripts

Als u optimaal buiten het voorbeeld moet u Duik in de opgegeven scripts. Gebruik onderbrekingspunten en doorloop de scripts als ze worden uitgevoerd en bekijk hoe de andere SaaS-patronen worden geïmplementeerd. Om het eenvoudig stap via de opgegeven scripts en modules voor het beste begrijpen, wordt u aangeraden de [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-the-configuration-file-for-your-deployment"></a>Het configuratiebestand voor uw implementatie bijwerken

Bewerk de **UserConfig.psm1** -bestand met de resource-groep en gebruiker waarde die u hebt ingesteld tijdens de implementatie:

1. Open de *PowerShell ISE* en laden... \\Learning-Modules\\*UserConfig.psm1* 
2. Update *ResourceGroupName* en *naam* met de specifieke waarden voor uw implementatie (op regels 10 en 11 alleen).
3. De wijzigingen opslaan!

Instellen van deze waarden hier simpelweg voorkomt u moet deze implementatie-specifieke waarden in elk script bijwerken.

### <a name="execute-the-scripts-by-pressing-f5"></a>De scripts worden uitgevoerd op F5 te drukken

Gebruik van meerdere scripts *$PSScriptRoot* om mappen te bladeren en *$PSScriptRoot* worden alleen geëvalueerd als scripts worden uitgevoerd door te drukken **F5**.  Syntaxismarkering en een selectie actief (**F8**) kunnen leiden tot fouten, indrukt **F5** wanneer het uitvoeren van scripts.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Doorloop de scripts stapsgewijs om de implementatie te kunnen bekijken

De beste manier om te begrijpen van de scripts is door doorlopen zien wat ze doen. Bekijk de opgenomen **Demo -** scripts die een eenvoudig te volgen werkstroom op hoog niveau. De **Demo -** scripts tonen de stappen die nodig zijn voor elke taak, dus onderbrekingspunten instellen en inzoomen dieper in de afzonderlijke aanroepen voor implementatie-informatie voor de verschillende SaaS-patronen.

Tips voor het verkennen en PowerShell-scripts doorlopen:

- Open **Demo -** scripts in de PowerShell ISE.
- Of Ga door met uitvoeren **F5** (met behulp van **F8** wordt niet aanbevolen omdat *$PSScriptRoot* wordt niet geëvalueerd wanneer selecties van een script wordt uitgevoerd).
- Plaats onderbrekingspunten door op een regel te klikken of een regel te selecteren en op **F9** te drukken.
- Stap over een functie of scriptaanroep heen met **F10**.
- Stap in een functie of scriptaanroep met **F11**.
- Stap uit de huidige functie of scriptaanroep met **Shift + F11**.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Bekijk het databaseschema en voer SQL-query’s uit met SSMS

Gebruik [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) verbinding maken en blader door de toepassingsservers en databases.

De implementatie is in eerste instantie tenants en SQL Database-catalogusservers kunt koppelen aan. De naamgeving van de servers, is afhankelijk van de database tenancymodus patroon (Zie hieronder voor details). 

   - **Zelfstandige toepassing:** servers voor elke tenant (ex. *contosoconcerthall -&lt;gebruiker&gt;*  server) en *catalogus-sa -&lt;gebruiker&gt;*
   - **Database per tenant:** *tenants1-dpt -&lt;gebruiker&gt;*  en *catalogus-dpt -&lt;gebruiker&gt;*  servers
   - **Multitenant-database:** *tenants1-mt -&lt;gebruiker&gt;*  en *catalogus-mt -&lt;gebruiker&gt;*  servers

Een geslaagde demo-verbinding, zodat alle servers hebben een [firewallregel](sql-database-firewall-configure.md) toestaan via alle IP-adressen.


1. Open *SSMS* en maak verbinding met de de tenants. Naam van de server is afhankelijk van de database tenancymodus patroon dat u hebt geselecteerd (Zie hieronder voor details):
    - **Zelfstandige toepassing:** servers van individuele tenants (ex. *contosoconcerthall -&lt;gebruiker&gt;. database.windows.net*) 
    - **Database per tenant:** *tenants1-dpt -&lt;gebruiker&gt;. database.windows.net*
    - **Multitenant-database:** *tenants1-mt -&lt;gebruiker&gt;. database.windows.net* 
2. Klik op **Verbinding maken**  > **Database-engine...**:

   ![catalogusserver](media/saas-tenancy-wingtip-app-guidance-tips/connect.png)

3. Referenties voor Demo zijn: Gebruikersnaam = *developer*, Wachtwoord = *P@ssword1*

    De onderstaande afbeelding ziet u de aanmelding voor de *Database per tenant* patroon. 
    ![verbinding](media/saas-tenancy-wingtip-app-guidance-tips/tenants1-connect.png)
    
   

4. Herhaal stappen 2-3 en maak verbinding met de GC-server (Zie hieronder voor specifieke servernamen op basis van de database tenancymodus geselecteerd)
    - **Zelfstandige toepassing:** *catalogus-sa -&lt;gebruiker&gt;. database.windows.net*
    - **Database per tenant:** *catalogus-dpt -&lt;gebruiker&gt;. database.windows.net*
    - **Multitenant-database:** *catalogus-mt -&lt;gebruiker&gt;. database.windows.net*


Nadat de verbinding tot stand ziet u alle servers. Uw lijst met databases mogelijk verschillen, afhankelijk van de tenants die u hebt ingericht.

De onderstaande afbeelding ziet u het logboek in voor de *Database per tenant* patroon.

![objectverkenner](media/saas-tenancy-wingtip-app-guidance-tips/object-explorer.png)



## <a name="next-steps"></a>Volgende stappen
- [De Wingtip Tickets zelfstandige SaaS-toepassing implementeren](saas-standaloneapp-get-started-deploy.md)
- [De Database Wingtip Tickets SaaS per Tenant-toepassing implementeren](saas-dbpertenant-get-started-deploy.md)
- [De Database Wingtip Tickets SaaS-multitenant-toepassing implementeren](saas-multitenantdb-get-started-deploy.md)

