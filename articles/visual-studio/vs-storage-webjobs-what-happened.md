---
title: Wat is er gebeurd met mijn webtaak-project (Visual Studio Azure Storage verbonden service)? | Microsoft Docs
description: Hierin wordt beschreven wat er gebeurd is in een project Azure webtaak nadat verbinding te maken met een opslagaccount met Visual Studio services verbonden
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 8891685a99c5ba366b74af0a21396d4a5e499835
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
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
* **Systeem.gegevens**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Verbindingsreeks voor Azure Storage toegevoegd
In het bestand App.config van uw project de **AzureWebJobsStorage** en **AzureWebJobsDashboard** -items zijn bijgewerkt met de verbindingsreeks en de sleutel van het geselecteerde opslagaccount.

Zie voor meer informatie [documentatiebronnen Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).

