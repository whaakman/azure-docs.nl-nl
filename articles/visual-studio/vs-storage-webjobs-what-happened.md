---
title: Wat is er gebeurd met mijn webtaak-project (Visual Studio Azure Storage verbonden service)? | Microsoft Docs
description: Hierin wordt beschreven wat er gebeurd is in een project Azure webtaak nadat verbinding te maken met een opslagaccount met Visual Studio services verbonden
services: storage
author: ghogen
manager: douge
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 056ccd572a04a436ff53dda6ae967711c9f9903d
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Wat is er gebeurd met mijn webtaak-project (Visual Studio Azure Storage verbonden service)?
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

## <a name="connection-string-for-azure-storage-added"></a>Verbindingsreeks voor Azure Storage toegevoegd
In het bestand App.config van uw project de **AzureWebJobsStorage** en **AzureWebJobsDashboard** -items zijn bijgewerkt met de verbindingsreeks en de sleutel van het geselecteerde opslagaccount.

Zie voor meer informatie [documentatiebronnen Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).

