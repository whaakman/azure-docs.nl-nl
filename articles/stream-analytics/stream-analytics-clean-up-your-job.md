---
title: Opschonen van uw Azure Stream Analytics-taak
description: Dit artikel is een handleiding voor het verwijderen van Azure Stream Analytics-taken.
services: stream-analytics
author: mamccrea
manager: kfile
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 4773f542f12ae1773e881106bc8948c663bfd1e3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660829"
---
# <a name="clean-up-your-azure-stream-analytics-job"></a>Opschonen van uw Azure Stream Analytics-taak

Azure Stream Analytics-taken kunnen eenvoudig worden verwijderd via de Azure portal, Azure PowerShell, Azure SDK voor .net of REST-API.

>[!NOTE] 
>Wanneer u uw Stream Analytics-taak stopt, de gegevens zich blijft voordoen alleen in de invoer- en opslag, zoals Event Hubs of Azure SQL Database. Als u vereist zijn om gegevens te verwijderen van Azure, zorg er dan voor dat het verwijderingsproces voor de invoer- en resources van uw Stream Analytics-taak volgen.

## <a name="stop-a-job-in-azure-portal"></a>Een Azure-portal-taak stoppen

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Zoek uw Stream Analytics-taak uitgevoerd en selecteer deze.

3. Selecteer op de pagina Stream Analytics-taak **stoppen** stoppen van de taak. 

   ![Taak stoppen](./media/stream-analytics-clean-up-your-job/stop-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Verwijderen van een taak in de Azure-portal

1. Meld u aan bij Azure Portal. 

2. Uw bestaande Stream Analytics-taak vinden en selecteren.

3. Selecteer op de pagina Stream Analytics-taak **verwijderen** verwijderen van de taak. 

   ![Taak verwijderen](./media/stream-analytics-clean-up-your-job/delete-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>Beëindigt of verwijdert u een taak met PowerShell

Voor het beëindigen van een taak met behulp van PowerShell, gebruikt de [Stop AzureRmStreamAnalyticsJob](https://docs.microsoft.com/en-us/powershell/module/azurerm.streamanalytics/stop-azurermstreamanalyticsjob?view=azurermps-5.7.0) cmdlet. Als u wilt verwijderen van een taak met behulp van PowerShell, gebruikt u de [verwijderen AzureRmStreamAnalyticsJob](https://docs.microsoft.com/en-us/powershell/module/azurerm.streamanalytics/Remove-AzureRmStreamAnalyticsJob?view=azurermps-5.7.0) cmdlet.

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>Beëindigt of verwijdert u een taak met Azure SDK voor .NET

Voor het beëindigen van een taak met Azure SDK voor .NET gebruikt de [StreamingJobsOperationsExtensions.BeginStop](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet) methode. Verwijderen van een taak met Azure SDK voor .NET, [StreamingJobsOperationsExtensions.BeginDelete](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet) methode.

## <a name="stop-or-delete-a-job-using-rest-api"></a>Beëindigt of verwijdert u een taak met de REST-API

Als u wilt stoppen van een taak met de REST-API, verwijzen naar de [stoppen](https://docs.microsoft.com/en-us/rest/api/streamanalytics/stream-analytics-job#stop) methode. Als u wilt verwijderen van een taak met de REST-API, verwijzen naar de [verwijderen](https://docs.microsoft.com/en-us/rest/api/streamanalytics/stream-analytics-job#delete) methode.