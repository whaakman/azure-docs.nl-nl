---
title: 'Overzicht: beheerhulpprogramma&quot;s voor SQL Database | Microsoft Docs'
description: Hiermee vergelijkt u hulpprogramma&quot;s en opties voor het beheer van Azure SQL Database
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 37767380-975f-4dee-a28d-80bc2036dda3
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 73cc9d1d02ed915466f0eac2d9a42fe8cf9e2fb9
ms.openlocfilehash: 32d0a1fd8671bbd9d450711dd686ca49c2178c16


---
# <a name="overview-management-tools-for-sql-database"></a>Overzicht: beheerhulpprogramma's voor SQL-database
In dit onderwerp worden de hulpprogramma's en opties voor het beheer van Azure SQL-databases verkend en vergeleken, zodat u het juiste hulpprogramma kunt kiezen voor de uit te voeren taak, uw bedrijf en uzelf. De keuze van het juiste hulpprogramma is afhankelijk van het aantal databases dat u beheert, de taak en hoe vaak de taak wordt uitgevoerd.

## <a name="azure-portal"></a>Azure Portal
[Azure Portal](https://portal.azure.com) is een webtoepassing waarin u databases en logische servers kunt maken, bijwerken en verwijderen, en databaseactiviteiten kunt bewaken. Dit hulpprogramma is heel handig als u pas bent gestart met Azure, voor het beheer van enkele databases of als u snel iets moet doen.

Zie [SQL-databases beheren met Azure Portal](sql-database-manage-portal.md) voor informatie over het gebruik van de portal.

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio en SQL Server Data Tools in Visual Studio
SQL Server Management Studio (SSMS) en SQL Server Data Tools (SSDT) zijn clienthulpprogramma's die worden uitgevoerd op de computer voor het beheren en ontwikkelen van uw database in de cloud. Als u toepassingsontwikkelaar bent en Visual Studio of andere geïntegreerde ontwikkelingsomgevingen (IDE's) kent, probeer dan eens [SSDT te gebruiken in Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). Veel databasebeheerders zijn bekend met SSMS. Dit programma kan worden gebruikt in combinatie met Azure SQL-databases. [Download de nieuwste versie van SSMS](https://msdn.microsoft.com/library/mt238290) en gebruik altijd de recentste versie als u met Azure SQL Database werkt. Zie [SQL-databases beheren met SSMS](sql-database-manage-azure-ssms.md) voor meer informatie over het beheren van uw Azure SQL-databases met SSMS.

> [!IMPORTANT]
> Gebruik altijd de nieuwste versie van [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) en [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx), zodat uw versie gesynchroniseerd blijft met updates voor Microsoft Azure en SQL Database.
>  

## <a name="powershell"></a>PowerShell
U kunt PowerShell gebruiken voor het beheren van databases en elastische pools en voor het automatiseren van de implementatie van Azure-resources. Microsoft raadt dit hulpprogramma aan voor het beheren van een groot aantal databases en voor het automatiseren van de implementatie - en resourcewijzigingen in een productieomgeving.

Zie [SQL Database beheren met PowerShell](sql-database-manage-powershell.md) voor meer informatie

## <a name="elastic-database-tools"></a>Hulpprogramma's voor elastische databases
Gebruik de hulpprogramma's voor elastische databases voor het uitvoeren van acties zoals 

* Een T-SQL-script uitvoeren voor een set van databases met een [elastische taak](sql-database-elastic-jobs-overview.md)
* Modeldatabases met meerdere tenants verplaatsen naar een model met één tenant met het [hulpprogramma voor splitsen en samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md)
* Databases beheren in een model met een of meer tenants met de [elastische schaal-clientbibliotheek](sql-database-elastic-database-client-library.md).

## <a name="additional-resources"></a>Aanvullende bronnen
* [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
* [Azure Automation](https://azure.microsoft.com/documentation/services/automation/)
* [Azure Scheduler](https://azure.microsoft.com/documentation/services/scheduler/)




<!--HONumber=Jan17_HO1-->


