---
title: Beheren van Azure SQL-databases met Azure Automation | Microsoft Docs
description: Meer informatie over hoe Azure Automation-service kan worden gebruikt voor het beheren van Azure SQL-databases op schaal.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
manager: craigg
ms.date: 12/19/2018
ms.openlocfilehash: f462940f652f568168c4d0b1e8012c7196d59b13
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57314157"
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a>Azure SQL-databases met Azure Automation beheren

Deze handleiding vindt u de Azure Automation-service en hoe deze kan worden gebruikt voor het vereenvoudigen van beheer van uw Azure SQL-databases.

## <a name="what-is-azure-automation"></a>Wat is Azure Automation?

[Azure Automation](https://azure.microsoft.com/services/automation/) is een Azure-service voor cloudbeheer via procesautomatisering te vereenvoudigen. Met Azure Automation, worden langlopende, handmatige, foutgevoelige en regelmatig herhaalde taken geautomatiseerd om de betrouwbaarheid, efficiëntie en tijd tot waarde voor uw organisatie te verhogen.

Azure Automation biedt een engine voor werkstroomuitvoering met hoge betrouwbaarheid en hoge beschikbaarheid en die kan worden geschaald om te voldoen aan uw behoeften wanneer uw organisatie groeit. In Azure Automation, kunnen processen worden gestart handmatig, door systemen van derden, of met regelmatige tussenpozen zodat taken gebeuren precies wanneer dat nodig is.

Lagere operationele overhead en vrij IT / DevOps-personeel te concentreren op werk dat wordt toegevoegd business value door te verplaatsen van uw cloud-beheertaken automatisch wordt uitgevoerd door Azure Automation.

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Hoe u kunt Azure Automation beheren van Azure SQL-databases?

Azure SQL-Database kunnen worden beheerd in Azure Automation met behulp van de [Azure SQL Database PowerShell-cmdlets](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#sql) die beschikbaar zijn in de [Azure PowerShell-hulpprogramma's](/powershell/azure/overview). Azure Automation heeft deze Azure SQL Database PowerShell-cmdlets beschikbaar gebruiksklaar, zodat u al uw SQL-database-beheertaken in de service kunt uitvoeren. U kunt ook deze cmdlets in Azure Automation met de cmdlets voor andere Azure-services voor het automatiseren van complexe taken tussen Azure-services en systemen van derden worden gekoppeld.

Azure Automation biedt ook de mogelijkheid om te communiceren met de SQL-servers rechtstreeks met behulp van SQL-opdrachten met behulp van PowerShell.

De [Azure Automation runbook gallery](https://azure.microsoft.com/blog/20../../introducing-the-azure-automation-runbook-gallery/) bevat tal van product-team en community-runbooks om te beginnen met het automatiseren van beheer van Azure SQL-databases, andere Azure-services en systemen van derden. Runbooks uit de galerie zijn onder andere:

- [SQL-query's uitvoeren op een SQL Server-database](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
- [Verticaal schalen (omhoog of omlaag) een Azure SQL-Database volgens een schema](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
- [Een SQL-tabel worden afgekapt als de database bijna de maximale grootte](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
- [Tabellen in een Azure SQL Database indexeren als ze zijn zeer gefragmenteerd](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Volgende stappen

Nu dat u de basisprincipes van Azure Automation en hoe deze kan worden gebruikt voor het beheren van Azure SQL-databases hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure Automation.

- [Overzicht van Azure Automation](../automation/automation-intro.md)
- [Mijn eerste runbook](../automation/automation-first-runbook-graphical.md)
- [Azure Automation: Uw SQL-Agent in de Cloud](https://azure.microsoft.com/blog/20../../azure-automation-your-sql-agent-in-the-cloud/) 