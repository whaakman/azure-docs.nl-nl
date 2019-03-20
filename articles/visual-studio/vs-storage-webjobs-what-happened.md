---
title: Wat is er gebeurd met mijn WebJob-project (Visual Studio Azure Storage verbonden service)? | Microsoft Docs
description: Hierin wordt beschreven wat er gebeurd in een Azure WebJob-project nadat de verbinding te maken met een opslagaccount met behulp van Visual Studio verbonden services
services: storage
author: ghogen
manager: douge
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: fa152d8b88254a35d00b91537bf1001ea1130e57
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57884639"
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Wat is er gebeurd met mijn WebJob-project (Visual Studio Azure Storage verbonden service)?
## <a name="references-added"></a>Verwijzingen die zijn toegevoegd
Het Azure Storage NuGet-pakket is toegevoegd aan of bijgewerkt in Visual Studio-project.  
Dit pakket voegt de volgende .NET verwijzingen toe:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Verbindingsreeks voor Azure Storage is toegevoegd
In het bestand App.config van uw project, de **AzureWebJobsStorage** en **AzureWebJobsDashboard** vermeldingen zijn bijgewerkt met de verbindingsreeks en de sleutel van het geselecteerde opslagaccount.

Zie voor meer informatie, [documentatiebronnen voor Azure WebJobs](https://go.microsoft.com/fwlink/?linkid=390226).

