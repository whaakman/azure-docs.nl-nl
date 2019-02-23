---
title: Opschonen van uw Azure Stream Analytics-taak
description: In dit artikel ziet u verschillende methoden voor het verwijderen van uw Azure Stream Analytics-taken.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: cfc01667f1fee7e57929e1990b5cf8c99a0fd595
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737498"
---
# <a name="clean-up-your-azure-stream-analytics-job"></a>Opschonen van uw Azure Stream Analytics-taak

Azure Stream Analytics-taken kunnen eenvoudig worden verwijderd via de Azure portal, Azure PowerShell of Azure SDK voor .net of REST-API.

>[!NOTE] 
>Wanneer u de Stream Analytics-taak stopt, de gegevens zich blijft voordoen alleen in de invoer- en opslag, zoals Event Hubs of Azure SQL Database. Als u gegevens uit Azure verwijdert moet, moet u de verwijdering processen voor de invoer- en bronnen van uw Stream Analytics-taak volgen.

## <a name="stop-a-job-in-azure-portal"></a>Een taak in Azure portal stoppen

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Zoek uw actieve Stream Analytics-taak en selecteert u deze.

3. Selecteer op de pagina van de Stream Analytics-taak **stoppen** om de taak te stoppen. 

   ![Azure Stream Analytics-taak stoppen](./media/stream-analytics-clean-up-your-job/stop-stream-analytics-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Een taak in Azure portal verwijderen

1. Meld u aan bij Azure Portal. 

2. Zoek uw bestaande Stream Analytics-taak en selecteert u deze.

3. Selecteer op de pagina van de Stream Analytics-taak **verwijderen** om de taak te verwijderen. 

   ![Azure Stream Analytics-taak verwijderen](./media/stream-analytics-clean-up-your-job/delete-stream-analytics-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>Beëindigt of verwijdert u een taak met behulp van PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u wilt stoppen van een taak met behulp van PowerShell, gebruikt u de [Stop-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/stop-azstreamanalyticsjob) cmdlet. Als u wilt verwijderen van een taak met behulp van PowerShell, gebruikt u de [Remove-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/Remove-azStreamAnalyticsJob) cmdlet.

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>Beëindigt of verwijdert u een taak met Azure SDK voor .NET

Als u wilt stoppen van een taak met Azure SDK voor .NET, gebruiken de [StreamingJobsOperationsExtensions.BeginStop](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet) methode. Verwijderen van een taak met Azure SDK voor .NET, [StreamingJobsOperationsExtensions.BeginDelete](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet) methode.

## <a name="stop-or-delete-a-job-using-rest-api"></a>Beëindigt of verwijdert u een taak met behulp van REST-API

Als u wilt stoppen van een taak met behulp van REST-API, raadpleegt u de [stoppen](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#stop) methode. Als u wilt verwijderen van een taak met behulp van REST-API, raadpleegt u de [verwijderen](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#delete) methode.
