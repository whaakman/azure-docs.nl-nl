---
title: Taken voor elastische database installeren | Microsoft Docs
description: Installatie van de functie elastische taak doorlopen.
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: cbe0aa2b-17e3-4b6f-a16f-6ebc1f5a66af
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: ef5a8931eeda0f7ddc485632acaf2d76b71dccde
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="installing-elastic-database-jobs-overview"></a>Overzicht van de taken installeren elastische Database
[**Elastische Database taken** ](sql-database-elastic-jobs-overview.md) kan worden geïnstalleerd via PowerShell of via de Azure portal. U kunt toegang krijgen als u wilt maken en beheren van taken met de PowerShell-API alleen als u het PowerShell-pakket installeert. De PowerShell APIs Geef ook aanzienlijk meer functionaliteit dan de portal op dit moment.

Als u al hebt geïnstalleerd **elastische Database taken** via de Portal van een bestaand **elastische pool**, de meest recente Powershell voorbeeldweergave scripts voor uw bestaande installatie upgraden. Het is raadzaam een upgrade naar de meest recente **elastische Database taken** onderdelen om te profiteren van nieuwe functionaliteit beschikbaar gesteld via de PowerShell APIs.

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement. Zie voor een gratis proefversie [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* Azure PowerShell. Installeer de nieuwste versie met de [Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376). Zie voor gedetailleerde informatie [Installeren en configureren van Azure PowerShell](/powershell/azure/overview).
* [NuGet-opdrachtregelprogramma](https://nuget.org/nuget.exe) wordt gebruikt om de taken elastische Database pakket te installeren. Zie http://docs.nuget.org/docs/start-here/installing-nuget voor meer informatie.

## <a name="download-and-import-the-elastic-database-jobs-powershell-package"></a>Downloaden en importeren van de elastische Database taken PowerShell-pakket
1. Start Microsoft Azure PowerShell-opdrachtvenster en navigeer naar de map waar u NuGet-opdrachtregelprogramma (nuget.exe) gedownload.
2. Downloaden en importeren **elastische Database taken** pakket in de huidige map met de volgende opdracht:
   
        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease
   
    De **elastische Database taken** bestanden worden geplaatst in de lokale map in een map met de naam **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** waar *x.x.xxxx.x* weerspiegelt het versienummer. De PowerShell-cmdlets (met inbegrip van de vereiste client-dll's) bevinden zich in de **tools\ElasticDatabaseJobs** submappen en de PowerShell-scripts te installeren, upgraden en verwijderen ook bevinden zich in de **extra** onderliggende map.
3. Navigeer naar de submap van de hulpprogramma's onder de map Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x door hulpprogramma's voor een cd, bijvoorbeeld in te voeren:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4. Voer het script.\InstallElasticDatabaseJobsCmdlets.ps1 om te kopiëren van de map ElasticDatabaseJobs naar $home\Documents\WindowsPowerShell\Modules. Hiermee wordt de module voor gebruik, bijvoorbeeld ook automatisch geïmporteerd:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-the-elastic-database-jobs-components-using-powershell"></a>De elastische taken databaseonderdelen installeren met behulp van PowerShell
1. Start een Microsoft Azure PowerShell-opdrachtvenster en navigeer naar de submap \tools onder de map Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x: Typ cd \tools
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2. De.\InstallElasticDatabaseJobs.ps1 PowerShell-script uitvoeren en waarden voor de aangevraagde variabelen opgeven. Dit script maakt de onderdelen beschreven in [elastische Database taken onderdelen en prijzen](sql-database-elastic-jobs-overview.md#components-and-pricing) samen met de Azure-Cloudservice voor het gebruik van de afhankelijke onderdelen op de juiste wijze configureren.

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

Wanneer u deze opdracht een venster uitvoert weergegeven waarin een **gebruikersnaam** en **wachtwoord**. Dit is niet uw Azure-referenties, voer de gebruikersnaam en wachtwoord op dat de administrator-aanmeldgegevens die u wilt maken voor de nieuwe server.

De parameters die worden geleverd op voor dit voorbeeld aanroepen kunnen worden gewijzigd voor de gewenste instellingen. Het volgende bevat meer informatie over het gedrag van elke parameter:

<table style="width:100%">
  <tr>
    <th>Parameter</th>
    <th>Beschrijving</th>
  </tr>

<tr>
    <td>resourceGroupName</td>
    <td>Bevat de naam van de Azure-resourcegroep gemaakt om de zojuist gemaakte Azure onderdelen bevatten. Deze parameter wordt standaard ingesteld op '__ElasticDatabaseJob'. Het is niet raadzaam om deze waarde te wijzigen.</td>
    </tr>

</tr>

    <tr>
    <td>ResourceGroupLocation</td>
    <td>Biedt de Azure-locatie moet worden gebruikt voor de zojuist gemaakte Azure-onderdelen. Deze parameter wordt standaard ingesteld op de locatie VS-midden.</td>
</tr>

<tr>
    <td>ServiceWorkerCount</td>
    <td>Geeft het aantal werknemers van de service te installeren. Deze parameter wordt standaard ingesteld op 1. Een hoger aantal werknemers kan worden gebruikt voor het schalen van de service en voor maximale beschikbaarheid. Het verdient aanbeveling gebruik '2' voor implementaties die hoge beschikbaarheid van de service vereist.</td>
    </tr>

</tr>
    <tr>
    <td>ServiceVmSize</td>
    <td>De VM-grootte biedt voor gebruik in de Cloud-Service. Deze parameter wordt standaard ingesteld op A0. Parameterwaarden van A0/A1/A2/A3, waardoor de werkrol de grootte van een extra klein/klein/gemiddeld/groot, respectievelijk gebruiken worden geaccepteerd. Voor meer informatie over grootten voor worker-rol, Zie [elastische Database taken onderdelen en prijzen](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerDatabaseSlo</td>
    <td>Biedt de serviceniveaudoelstelling voor een Standard-editie. Deze parameter de standaardwaarde S0. Parameterwaarden van S0/S1/S2/S3/S4/S6/S9/S12, waardoor de Azure SQL Database te gebruiken van de respectieve SLO worden geaccepteerd. Zie voor meer informatie over SQL Database Slo's [elastische Database taken onderdelen en prijzen](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorUserName</td>
    <td>Biedt de beheerder gebruikersnaam op voor de nieuwe Azure SQL Database-server. Wanneer niet wordt opgegeven, wordt een PowerShell-venster referenties geopend om de referenties gevraagd.</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorPassword</td>
    <td>Biedt het beheerderswachtwoord voor de nieuwe Azure SQL Database-server. Wanneer niet wordt opgegeven, wordt een PowerShell-venster referenties geopend om de referenties gevraagd.</td>
</tr>
</table>

Voor systemen die zijn gericht op met een groot aantal taken op basis van een groot aantal databases parallel worden uitgevoerd, wordt aanbevolen, zoals parameters opgeven: - ServiceWorkerCount 2 - ServiceVmSize A2 - SqlServerDatabaseSlo S2.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a>Bijwerken van een bestaande onderdelen installatie voor elastische Database taken met behulp van PowerShell
**Elastische Database taken** binnen een bestaande installatie van de schaal en hoge beschikbaarheid kunnen worden bijgewerkt. Dit proces kan bij toekomstige upgrades van servicecode zonder te verwijderen en opnieuw maken van de database. Dit proces kan ook worden gebruikt binnen dezelfde versie om de VM-grootte van de service of het aantal worker-server te wijzigen.

Voer het volgende script met de parameters die zijn bijgewerkt met de waarden van uw keuze voor het bijwerken van de installatie van een VM-grootte.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th>Parameter</th>
  <th>Beschrijving</th>
</tr>

  <tr>
    <td>resourceGroupName</td>
    <td>Geeft de naam van de Azure-resourcegroep gebruikt wanneer de onderdelen van de taak elastische Database in eerste instantie zijn geïnstalleerd. Deze parameter wordt standaard ingesteld op '__ElasticDatabaseJob'. Aangezien het niet aangeraden wordt om deze waarde te wijzigen, mag u geen om op te geven van deze parameter.</td>
    </tr>
</tr>

</tr>

  <tr>
    <td>ServiceWorkerCount</td>
    <td>Geeft het aantal werknemers van de service te installeren.  Deze parameter wordt standaard ingesteld op 1.  Een hoger aantal werknemers kan worden gebruikt voor het schalen van de service en voor maximale beschikbaarheid.  Het verdient aanbeveling gebruik '2' voor implementaties die hoge beschikbaarheid van de service vereist.</td>
</tr>

</tr>

    <tr>
    <td>ServiceVmSize</td>
    <td>De VM-grootte biedt voor gebruik in de Cloud-Service. Deze parameter wordt standaard ingesteld op A0. Parameterwaarden van A0/A1/A2/A3, waardoor de werkrol de grootte van een extra klein/klein/gemiddeld/groot, respectievelijk gebruiken worden geaccepteerd. Voor meer informatie over grootten voor worker-rol, Zie [elastische Database taken onderdelen en prijzen](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</table>

## <a name="install-the-elastic-database-jobs-components-using-the-portal"></a>De elastische taken databaseonderdelen installeren via de Portal
Zodra u hebt [een elastische pool gemaakt](sql-database-elastic-pool-manage-portal.md), kunt u installeren **elastische Database taken** onderdelen waarmee het uitvoeren van beheertaken voor elke database in de elastische groep. In tegenstelling tot wanneer met behulp van de **elastische Database taken** APIs PowerShell, de interface van de portal is momenteel beperkt tot alleen uitvoeren op basis van een bestaande toepassingen.

**Geschatte duur:** 10 minuten.

1. Uit de dashboardweergave van de elastische groep via de [Azure-portal](https://portal.azure.com/#) , klikt u op **maken taak**.
2. Als u een taak voor het eerst maakt, moet u **elastische Database taken** door te klikken op **PREVIEW-voorwaarden**.
3. De voorwaarden accepteren door te klikken op het selectievakje in.
4. Klik in de weergave 'Services installeren' **taak REFERENTIES**.
   
    ![De services te installeren][1]
5. Typ een gebruikersnaam en wachtwoord voor een database-beheerder. Een nieuwe Azure SQL Database-server is gemaakt als onderdeel van de installatie. In deze nieuwe server bekend als de database, een nieuwe database gemaakt en gebruikt voor de metagegevens voor elastische Database taken bevatten. De gebruikersnaam en wachtwoord hier gemaakte worden omwille van de logboekregistratie in voor de database gebruikt. Een afzonderlijke referentie wordt gebruikt voor de uitvoering van het script op basis van de databases in de pool.
   
    ![Maak een gebruikersnaam en wachtwoord][2]
6. Klik op de knop OK. De onderdelen voor u gemaakt in een paar minuten in een nieuw [resourcegroep](../azure-resource-manager/resource-group-overview.md). De nieuwe resourcegroep is vastgemaakt aan de start board, zoals hieronder wordt weergegeven. Taken nadat deze is gemaakt, elastische database (Cloudservice, SQL-Database, Service Bus en opslag) worden gemaakt in de groep.
   
    ![resourcegroep in start board][3]
7. Als u probeert te maken of beheren van een taak, terwijl de taken van de elastische database wordt geïnstalleerd bij het opgeven van **referenties** ziet u het volgende bericht.
   
    ![Implementatie nog in voortgang][4]

Als het verwijderen vereist is, moet u de resourcegroep verwijderen. Zie [verwijderen van de onderdelen van de taak elastische Database](sql-database-elastic-jobs-uninstall.md).

## <a name="next-steps"></a>Volgende stappen
Zorg ervoor dat een referentie met de juiste rechten heeft voor uitvoering van het script wordt gemaakt op elke database in de groep, Zie voor meer informatie [SQL Database beveiligen](sql-database-manage-logins.md).
Zie [maken en beheren van de taken voor een elastische Database](sql-database-elastic-jobs-create-and-manage.md) aan de slag.

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png
