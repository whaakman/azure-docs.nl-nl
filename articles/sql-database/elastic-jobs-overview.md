---
title: Taak voor Elastic Database voor Azure SQL-databases | Microsoft Docs
description: Taken voor Elastic Database als u wilt uitvoeren (T-SQL) Transact-SQL-scripts in een set van een of meer Azure SQL-databases configureren
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srinia
ms.author: srinia
ms.reviewer: sstein
manager: craigg
ms.date: 01/22/2018
ms.openlocfilehash: ee9e9389962cd454f9861e561dd76c96d019a1f0
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469329"
---
# <a name="create-configure-and-manage-elastic-jobs"></a>Maken, configureren en beheren van elastische taken

In dit artikel leert u hoe u kunt maken, configureren en beheren van elastische taken. Als u hebt geen elastische taken gebruikt [meer informatie over de concepten van de taak automation in Azure SQL Database](sql-database-job-automation-overview.md).

## <a name="create-and-configure-the-agent"></a>De agent maken en configureren

1. Maak of identificeer een lege SQL-database op servicelaag S0 of hoger. Deze database wordt gebruikt als de *taak database* tijdens het maken van een elastische taak agent.
2. Maak een elastische-taakagent in de [portal](https://portal.azure.com/#create/Microsoft.SQLElasticJobAgent) of met [PowerShell](elastic-jobs-powershell.md#create-the-elastic-job-agent).

   ![Het maken van de agent voor elastische taken](media/elastic-jobs-overview/create-elastic-job-agent.png)

## <a name="create-run-and-manage-jobs"></a>Taken maken, uitvoeren en beheren

1. Maak een referentie voor taakuitvoering in de *taakdatabase* met [PowerShell](elastic-jobs-powershell.md#create-job-credentials-so-that-jobs-can-execute-scripts-on-its-targets) of [T-SQL](elastic-jobs-tsql.md#create-a-credential-for-job-execution).
2. Definieer de doelgroep (de databases waarvoor u de taak wilt uitvoeren) met behulp van [PowerShell](elastic-jobs-powershell.md#define-the-target-databases-you-want-to-run-the-job-against) of [T-SQL](elastic-jobs-tsql.md#create-a-target-group-servers).
3. Maak een taakagentreferentie in elke database waarin de taak wordt uitgevoerd [(voeg de gebruiker (of rol) toe aan elke database in de groep)](sql-database-control-access.md). Zie de [PowerShell-zelfstudie](elastic-jobs-powershell.md#create-job-credentials-so-that-jobs-can-execute-scripts-on-its-targets) voor een voorbeeld.
4. Maak een taak met [PowerShell](elastic-jobs-powershell.md#create-a-job) of [T-SQL](elastic-jobs-tsql.md#deploy-new-schema-to-many-databases).
5. Voeg taakstappen toe met [PowerShell](elastic-jobs-powershell.md#create-a-job-step) of [T-SQL](elastic-jobs-tsql.md#deploy-new-schema-to-many-databases).
6. Voer een taak uit met [PowerShell](elastic-jobs-powershell.md#run-the-job) of [T-SQL](elastic-jobs-tsql.md#begin-ad-hoc-execution-of-a-job).
7. Bewaak de uitvoeringsstatus van de taak met de portal, [PowerShell](elastic-jobs-powershell.md#monitor-status-of-job-executions) of [T-SQL](elastic-jobs-tsql.md#monitor-job-execution-status).

   ![Portal](media/elastic-jobs-overview/elastic-job-executions-overview.png)

## <a name="credentials-for-running-jobs"></a>Referenties voor het uitvoeren van taken

Taken gebruiken [databasereferenties](/sql/t-sql/statements/create-database-scoped-credential-transact-sql) om verbinding te maken met de databases die tijdens de uitvoering zijn opgegeven door de doelgroep. Als een doelgroep servers of pools bevat, worden deze databasereferenties gebruikt om verbinding te maken met de hoofddatabase en de beschikbare databases op te sommen.

Het instellen van de juiste referenties voor het uitvoeren van een taak kan enigszins verwarrend zijn. Houd daarom de volgende punten in gedachten:

- De databasereferenties moeten worden gemaakt in de *taakdatabase*.
- **Alle doeldatabase moeten hebben een aanmelding met [voldoende machtigingen](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) voor de taak is voltooid** (`jobuser` in het diagram hieronder).
- Referenties kunnen worden hergebruikt binnen taken, en de referentie-wachtwoorden worden versleuteld en beveiligd is tegen gebruikers die alleen-lezen toegang tot taakobjecten hebben.

De volgende afbeelding is gemaakt als hulp bij het begrijpen en instellen van de juiste taakreferenties. **Denk eraan dat de gebruiker moet worden gemaakt in elke database (alle *doelgebruikersdatabases*) waarin de taak moet worden uitgevoerd**.

![Referenties voor Elastic Jobs](media/elastic-jobs-overview/job-credentials.png)

## <a name="security-best-practices"></a>Aanbevolen procedures voor beveiliging

Een paar aandachtspunten voor best practices voor het werken met elastische taken:

- Beperk het gebruik van de API's tot vertrouwde personen.
- Referenties moeten slechts de minimale bevoegdheden hebben die nodig zijn om de taakstap uit te voeren. Zie voor meer informatie, [autorisatie en machtigingen voor SQL Server](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/authorization-and-permissions-in-sql-server).
- Wanneer u een server en/of lid van groep doel, is het zeer een afzonderlijke referentie maken met machtigingen op de hoofddatabase voor databases die wordt gebruikt voor het uitbreiden van de databaselijsten met de server (s) en/of pool(s) voordat de taakuitvoering/weergavelijst nodig.

## <a name="agent-performance-capacity-and-limitations"></a>Agentprestaties, - capaciteit en -beperkingen

Elastische taken gebruiken minimale rekenresources tijdens het wachten tot langlopende taken zijn voltooid.

Afhankelijk van de grootte van de doelgroep van databases en de gewenste uitvoeringstijd voor een taak (aantal gelijktijdige werkrollen), vraagt de agent meer of minder rekenkracht en prestaties van de *taakdatabase* (hoe meer doelen en taken, hoe hoger de vereiste hoeveelheid rekenkracht).

Op dit moment is de preview beperkt tot 100 gelijktijdige taken.

### <a name="prevent-jobs-from-reducing-target-database-performance"></a>Voorkomen dat taken de prestaties van de doeldatabase doen afnemen

Om ervoor te zorgen dat resources niet worden overbelast tijdens het uitvoeren van taken voor databases in een elastische SQL-pool, kunnen taken zo worden geconfigureerd dat het aantal databases waarvoor een taak tegelijkertijd wordt uitgevoerd, wordt beperkt.

## <a name="best-practices-for-creating-jobs"></a>Best practices voor het maken van taken

### <a name="idempotent-scripts"></a>Idempotente scripts
De T-SQL-scripts van een taak moeten [idempotent](https://en.wikipedia.org/wiki/Idempotence) zijn. **Idempotent** betekent dat de resultaten van een geslaagd script hetzelfde zijn wanneer dit script opnieuw wordt uitgevoerd. Een script kan mislukken door tijdelijke netwerkproblemen. In dat geval wordt automatisch een vooraf ingesteld aantal keer opnieuw geprobeerd om de taak uit te voeren alvorens het wordt afgebroken. Een idempotent script heeft hetzelfde resultaat, zelfs als het twee keer (of vaker) wordt uitgevoerd.

Een eenvoudige tactiek is te testen of een object bestaat voordat u het maakt.


```sql
IF NOT EXIST (some_object)
    -- Create the object
    -- If it exists, drop the object before recreating it.
```

Op dezelfde manier moet een script met succes kunnen worden uitgevoerd door logisch te testen op problemen en hierop te reageren.



## <a name="next-steps"></a>Volgende stappen

- [Elastische taken maken en beheren met PowerShell](elastic-jobs-powershell.md)
- [Elastische taken maken en beheren met Transact-SQL (T-SQL)](elastic-jobs-tsql.md)
