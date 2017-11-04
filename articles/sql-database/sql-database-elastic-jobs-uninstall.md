---
title: Het hulpprogramma voor elastische database taken verwijderen
description: Informatie over het verwijderen van de elastische database taken onderdelen met de Azure portal van PowerShell.
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: bfc9d820-edbd-4fca-bfbf-1f339cfcc448
ms.service: sql-database
ms.workload: Inactive
ms.custom: scale out apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 5e665ee8cc9efacbd31111dc0458ad6096e457c0
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="uninstall-elastic-database-jobs-components"></a>Elastische Database taken onderdelen verwijderen
**Elastische Database taken** onderdelen kunnen worden verwijderd met de Azure-portal of PowerShell.

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a>Elastische taken databaseonderdelen met de Azure portal verwijderen
1. Open de [Azure Portal](https://portal.azure.com/).
2. Navigeer naar het abonnement dat bevat **elastische Database taken** onderdelen, namelijk het abonnement in welke elastische Database taken onderdelen zijn geïnstalleerd.
3. Klik op **Bladeren** en klik op **resourcegroepen**.
4. Selecteer de resourcegroep met de naam '__ElasticDatabaseJob'.
5. De resourcegroep verwijderen.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>Elastische taken databaseonderdelen met behulp van PowerShell verwijderen
1. Start een Microsoft Azure PowerShell-opdrachtvenster en navigeer naar de submap van de hulpprogramma's onder de map Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x: Type **cd extra**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > cd-hulpprogramma's
2. De.\UninstallElasticDatabaseJobs.ps1 PowerShell-script uitvoeren.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > deblokkeren bestand.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >.\UninstallElasticDatabaseJobs.ps1

Of gewoon, voer het volgende script, ervan uitgaande dat standaard waarden waar gebruikt bij installatie van de onderdelen:

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager

        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }

        Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a>Volgende stappen
Zie opnieuw installeren van de elastische Database taken [installeren van de service van de taak elastische Database](sql-database-elastic-jobs-service-installation.md)

Zie voor een overzicht van de elastische Database taken [elastische Database taken overzicht](sql-database-elastic-jobs-overview.md).

<!--Image references-->


