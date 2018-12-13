---
title: Taken voor elastic database installeren | Microsoft Docs
description: Helpen bij de installatie van de functie voor elastische taken.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: cc322f44760ddf0a7cd28751c895a7c4938dbbc0
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52867237"
---
# <a name="installing-elastic-database-jobs-overview"></a>Overzicht van de taken van Elastic Database installeren

[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]



[**Taken voor elastic Database** ](sql-database-elastic-jobs-overview.md) kan worden geïnstalleerd via PowerShell of via de Azure-portal. U kunt toegang krijgen als u wilt maken en beheren van taken met behulp van de PowerShell-API alleen als u het PowerShell-pakket installeert. Daarnaast bieden de PowerShell-APIs meer functionaliteit dan de portal op dit moment.

Als u al hebt geïnstalleerd **taken voor Elastic Database** via de Portal vanaf een bestaande **elastische pool**, de meest recente Powershell-voorbeeld bevat de scripts voor het upgraden van uw bestaande installatie. Het is raadzaam een upgrade naar de meest recente **taken voor Elastic Database** onderdelen om te profiteren van nieuwe functionaliteit die via de PowerShell-APIs.

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement. Zie voor een gratis proefversie [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* Azure PowerShell. Installeer de nieuwste versie met de [Web Platform Installer](https://go.microsoft.com/fwlink/p/?linkid=320376). Zie voor gedetailleerde informatie [Installeren en configureren van Azure PowerShell](/powershell/azure/overview).
* [NuGet-opdrachtregelprogramma](https://nuget.org/nuget.exe) wordt gebruikt om de Elastic Database jobs-pakket te installeren. Zie voor meer informatie, http://docs.nuget.org/docs/start-here/installing-nuget.

## <a name="download-and-import-the-elastic-database-jobs-powershell-package"></a>Downloaden en importeren van het pakket met elastische taken PowerShell
1. Start Microsoft Azure PowerShell-opdrachtvenster en navigeer naar de map waar u NuGet-opdrachtregelprogramma (nuget.exe) gedownload.
2. Downloaden en importeren **taken voor Elastic Database** pakket in de huidige map met de volgende opdracht:
   
        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease
   
    De **taken voor Elastic Database** bestanden worden geplaatst in de lokale map in een map met de naam **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** waar *x.x.xxxx.x* weerspiegelt de het versienummer. De PowerShell-cmdlets (met inbegrip van de vereiste client-dll's) bevinden zich in de **tools\ElasticDatabaseJobs** submappen en de PowerShell-scripts om te installeren, upgraden en verwijderen ook bevinden zich in de **extra** onderliggende map.
3. Navigeer naar de onderliggende map van de hulpprogramma's onder de map Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x door te typen cd-hulpprogramma's, bijvoorbeeld:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4. Voer het script.\InstallElasticDatabaseJobsCmdlets.ps1 om te kopiëren van de map ElasticDatabaseJobs naar $home\Documents\WindowsPowerShell\Modules. Dit wordt ook automatisch Importeer de module voor gebruik, bijvoorbeeld:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-the-elastic-database-jobs-components-using-powershell"></a>Installeer de onderdelen van Elastic Database-taken met behulp van PowerShell
1. Start een Microsoft Azure PowerShell-opdrachtvenster en navigeer naar de onderliggende map \tools onder de map Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x: Typ cd \tools
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2. De.\InstallElasticDatabaseJobs.ps1 PowerShell-script uitvoeren en waarden voor de aangevraagde variabelen opgeven. Met dit script maakt u de onderdelen die worden beschreven [elastische taken onderdelen en prijzen](sql-database-elastic-jobs-overview.md#components-and-pricing) samen met de Azure-Cloudservice voor het gebruik van de afhankelijke onderdelen op de juiste wijze configureren.

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

Bij het uitvoeren met deze opdracht een venster weergegeven waarin een **gebruikersnaam** en **wachtwoord**. Dit is niet uw Azure-referenties, voer de gebruikersnaam en wachtwoord op dat de referenties die u wilt maken voor de nieuwe server.

De parameters op voor dit voorbeeld-aanroepen die kunnen worden gewijzigd voor de gewenste instellingen. Hieronder vindt u meer informatie over het gedrag van elke parameter:

<table style="width:100%">
  <tr>
    <th>Parameter</th>
    <th>Beschrijving</th>
  </tr>

<tr>
    <td>ResourceGroupName</td>
    <td>Naam van de Azure-resourcegroep gemaakt waartoe de zojuist gemaakte Azure-onderdelen bevat. Deze parameter wordt standaard ingesteld op '__ElasticDatabaseJob'. Het wordt niet aanbevolen om deze waarde te wijzigen.</td>
    </tr>

</tr>

    <tr>
    <td>ResourceGroupLocation</td>
    <td>Biedt de Azure-locatie moet worden gebruikt voor de zojuist gemaakte Azure-onderdelen. Deze parameter wordt standaard ingesteld op de locatie VS-midden.</td>
</tr>

<tr>
    <td>ServiceWorkerCount</td>
    <td>Geeft het aantal werknemers van de service te installeren. Deze parameter wordt standaard ingesteld op 1. Een groter aantal werknemers kan worden gebruikt om uit de service te schalen en hoge beschikbaarheid. Het verdient "2" gebruiken voor implementaties die hoge beschikbaarheid van de service nodig hebt.</td>
    </tr>

</tr>
    <tr>
    <td>ServiceVmSize</td>
    <td>Biedt de VM-grootte voor gebruik binnen de Service in de Cloud. Deze parameter wordt standaard ingesteld op A0. Parameterwaarden van A0/A1/A2/A3, waardoor de werkrol te gebruiken een grootte ExtraSmall/kleine/gemiddeld/groot, respectievelijk worden geaccepteerd. Voor meer informatie over de rolgrootten werknemer, Zie [elastische taken onderdelen en prijzen](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerDatabaseSlo</td>
    <td>Biedt de compute-grootte voor een Standard-editie. Deze parameter standaard S0. Parameterwaarden van S0/S1/S2/S3/S4/S6/S9/S12 zijn geaccepteerd dit leidt ertoe de Azure SQL Database dat om de grootte van het respectieve compute gebruiken. Zie voor meer informatie over SQL Database-compute-grootten [elastische taken onderdelen en prijzen](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorUserName</td>
    <td>Biedt de beheerdersnaam voor de nieuwe Azure SQL Database-server. Als niet is opgegeven, wordt u vragen om de referenties een PowerShell-referenties-venster geopend.</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorPassword</td>
    <td>Biedt het beheerderswachtwoord voor de nieuwe Azure SQL Database-server. Als niet is opgegeven, wordt u vragen om de referenties een PowerShell-referenties-venster geopend.</td>
</tr>
</table>

Voor systemen die zijn gericht op met een groot aantal taken die op basis van een groot aantal databases parallel worden uitgevoerd, wordt aanbevolen, zoals parameters opgeven: - ServiceWorkerCount 2 - ServiceVmSize A2 - SqlServerDatabaseSlo S2.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a>Bijwerken van een bestaande onderdelen installatie voor Elastic Database-taken met behulp van PowerShell
**Taken voor elastic Database** binnen een bestaande installatie voor schaalbaarheid en hoge beschikbaarheid kunnen worden bijgewerkt. Dit proces kan bij toekomstige upgrades van de servicecode zonder dat u wilt verwijderen en opnieuw maken van de database. Dit proces kan ook worden gebruikt binnen dezelfde versie om de VM-grootte van de service of het aantal worker-server te wijzigen.

Voer het volgende script met de parameters die zijn bijgewerkt met de waarden van uw keuze voor het bijwerken van de VM-grootte van een installatie.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th>Parameter</th>
  <th>Beschrijving</th>
</tr>

  <tr>
    <td>ResourceGroupName</td>
    <td>Geeft de naam van de Azure-resource die wordt gebruikt wanneer de onderdelen van Elastic Database-taak in eerste instantie zijn geïnstalleerd. Deze parameter wordt standaard ingesteld op '__ElasticDatabaseJob'. Aangezien dit niet aangeraden wordt om deze waarde te wijzigen, moet u al dan niet mogen deze parameter opgeeft.</td>
    </tr>
</tr>

</tr>

  <tr>
    <td>ServiceWorkerCount</td>
    <td>Geeft het aantal werknemers van de service te installeren.  Deze parameter wordt standaard ingesteld op 1.  Een groter aantal werknemers kan worden gebruikt om uit de service te schalen en hoge beschikbaarheid.  Het verdient "2" gebruiken voor implementaties die hoge beschikbaarheid van de service nodig hebt.</td>
</tr>

</tr>

    <tr>
    <td>ServiceVmSize</td>
    <td>Biedt de VM-grootte voor gebruik binnen de Service in de Cloud. Deze parameter wordt standaard ingesteld op A0. Parameterwaarden van A0/A1/A2/A3, waardoor de werkrol te gebruiken een grootte ExtraSmall/kleine/gemiddeld/groot, respectievelijk worden geaccepteerd. Voor meer informatie over de rolgrootten werknemer, Zie [elastische taken onderdelen en prijzen](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</table>

## <a name="install-the-elastic-database-jobs-components-using-the-portal"></a>Installeer de onderdelen van Elastic Database-taken met behulp van de Portal
Als u eenmaal hebt [gemaakt van een elastische pool](sql-database-elastic-pool-manage-portal.md), u kunt installeren **taken voor Elastic Database** onderdelen voor het uitvoeren van beheertaken voor elke database in de elastische pool inschakelen. In tegenstelling tot wanneer met behulp van de **taken voor Elastic Database** PowerShell-APIs, de interface van de portal is momenteel beperkt tot alleen uitvoeren op basis van een bestaande pool.

**Geschatte duur:** 10 minuten.

1. In de dashboardweergave van de elastische groep via de [Azure-portal](https://portal.azure.com/#) , klikt u op **maken taak**.
2. Als u een taak voor het eerst maakt, moet u installeren **taken voor Elastic Database** door te klikken op **voorwaarden voor PREVIEW**.
3. De voorwaarden accepteren door te klikken op het selectievakje in.
4. Klik in de weergave "Services installeren" op **TAAKREFERENTIES**.
   
    ![Installeren van de services][1]
5. Typ een gebruikersnaam en wachtwoord voor de beheerder van een database. Als onderdeel van de installatie van wordt een nieuwe Azure SQL Database-server gemaakt. In deze nieuwe server is een nieuwe database, de database, ook wel gemaakt en gebruikt voor het bevat de metagegevens voor de taken voor Elastic Database. De gebruikersnaam en wachtwoord hier gemaakt worden gebruikt voor het aanmelden bij de database. Een afzonderlijke referentie wordt gebruikt voor de uitvoering van het script op basis van de databases in de groep.
   
    ![Maak een gebruikersnaam en wachtwoord][2]
6. Klik op de knop OK. De onderdelen worden voor u gemaakt in een paar minuten een nieuwe [resourcegroep](../azure-resource-manager/resource-group-overview.md). De nieuwe resourcegroep is vastgemaakt aan het begin-bord, zoals hieronder wordt weergegeven. Nadat deze is gemaakt, elastische databasetaken (Cloud Service, SQL-Database, Service Bus en opslag) worden gemaakt in de groep.
   
    ![resourcegroep in start board][3]
7. Als u probeert te maken of een taak beheren als taken voor elastic database wordt geïnstalleerd bij het opgeven van **referenties** ziet u het volgende bericht weergegeven.
   
    ![De implementatie wordt nog steeds wordt uitgevoerd][4]

Als verwijdering vereist is, verwijdert u de resourcegroep. Zie [verwijderen van de elastische taak databaseonderdelen](sql-database-elastic-jobs-uninstall.md).

## <a name="next-steps"></a>Volgende stappen
Zorg ervoor dat een referentie met de juiste rechten heeft voor de uitvoering van het script is gemaakt voor elke database in de groep, Zie voor meer informatie [beveiligen van uw SQL-Database](sql-database-manage-logins.md).
Zie [maken en beheren van een elastische databasetaken](sql-database-elastic-jobs-create-and-manage.md) aan de slag.

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png
