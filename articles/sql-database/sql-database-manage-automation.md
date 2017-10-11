---
title: Azure SQL-Databases met behulp van Azure Automation beheren | Microsoft Docs
description: Meer informatie over hoe Azure Automation-service kan worden gebruikt voor het beheren van Azure SQL-databases op grote schaal.
services: sql-database, automation
documentationcenter: 
author: jodoglevy
manager: jhubbard
editor: monicar
ms.assetid: 77c262a1-9b93-456d-b3c7-b2f23bdfcd61
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: jhubbard
ms.openlocfilehash: 7f45b8b654691063823c13bee61e9bb874a6a13a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a>Azure SQL-Databases met behulp van Azure Automation beheren
Deze handleiding vindt u de Azure Automation-service en hoe deze kan worden gebruikt voor het vereenvoudigen van beheer van uw Azure SQL-databases.

## <a name="what-is-azure-automation"></a>Wat is Azure Automation?
[Azure Automation](https://azure.microsoft.com/services/automation/) is een Azure-service voor het cloudbeheer via procesautomatisering vereenvoudigen. Met behulp van Azure Automation, worden langlopende, handmatige, foutgevoelige en regelmatig herhaalde taken geautomatiseerd om betrouwbaarheid, efficiëntie en tijd voor de waarde voor uw organisatie te verhogen.

Azure Automation biedt een engine voor het uitvoeren van maximaal betrouwbaar en maximaal beschikbare werkstroom die schaalbaar is om te voldoen aan de behoeften van uw wanneer uw organisatie groeit. In Azure Automation kunnen processen worden gestarte handmatig, door systemen van derden 3rd of met regelmatige tussenpozen zodat taken gebeuren precies wanneer deze nodig is.

Lagere operationele overhead en vrijmaken IT / DevOps-personeel te concentreren op het werk dat zakelijke voegt waarde door uw cloud-beheertaken automatisch wordt uitgevoerd door Azure Automation te verplaatsen.

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Hoe kan Azure Automation helpen Azure SQL-databases beheren?
Azure SQL Database in Azure Automation kunnen worden beheerd met behulp van de [Azure SQL Database PowerShell-cmdlets](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) die beschikbaar zijn in de [hulpprogramma's van Azure PowerShell](/powershell/azure/overview). Azure Automation heeft deze Azure SQL Database PowerShell-cmdlets beschikbaar gebruiksklaar, zodat u al uw taken voor het beheer van SQL-database in de service kunt uitvoeren. U kunt ook deze cmdlets in Azure Automation met de cmdlets voor andere Azure-services, complexe om taken te automatiseren via Azure-services en 3e systemen van derden koppelen.

Azure Automation heeft ook de mogelijkheid om te communiceren met de SQL-servers rechtstreeks, door uitgifte van SQL-opdrachten met behulp van PowerShell.

De [galerie van Azure Automation-runbook](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) bevat een verscheidenheid aan product team en community runbooks om te beginnen met het automatiseren van beheer van Azure SQL-Databases, andere Azure-services en 3e systemen van derden. Galerie-runbooks zijn onder andere:

* [SQL-query's uitvoeren op een SQL Server-database](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
* [(Omhoog of omlaag) verticaal te schalen een Azure SQL Database volgens een schema](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
* [Een SQL-tabel afkappen als de database de maximale grootte nadert](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
* [Indexeren van tabellen in een Azure SQL Database als ze zijn sterk gefragmenteerd zijn](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van Azure Automation en hoe deze kan worden gebruikt voor het beheren van Azure SQL-databases hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure Automation.

* [Overzicht van Azure Automation](../automation/automation-intro.md)
* [Mijn eerste runbook](../automation/automation-first-runbook-graphical.md)
* [Azure Automation-leeroverzicht](https://azure.microsoft.com/documentation/learning-paths/automation/)
* [Azure Automation: Uw SQL-Agent in de Cloud](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 

