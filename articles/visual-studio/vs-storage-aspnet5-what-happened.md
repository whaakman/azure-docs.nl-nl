---
title: Wat is er gebeurd met mijn 5 ASP.NET-project (Visual Studio verbonden services) | Microsoft Docs
description: Hierin wordt beschreven wat er gebeurt nadat verbinding te maken met een Azure storage-account in een Visual Studio-5 voor ASP.NET-project met Visual Studio services verbonden
services: storage
author: ghogen
manager: douge
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: f99ba4b6c954ae9faa87b9604c06e94c56e4f631
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Wat is er gebeurd met mijn 5 ASP.NET-project (Visual Studio Azure Storage verbonden services)?
## <a name="references-added"></a>Verwijzingen die zijn toegevoegd
Het Azure Storage NuGet-pakket is toegevoegd aan uw Visual Studio-project.  
Dit pakket voegt de volgende .NET verwijzingen toe:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

Ook het NuGet-pakket **Microsoft.Framework.Configuration.Json** is toegevoegd.

## <a name="connection-string-for-azure-storage-added"></a>Verbindingsreeks voor Azure Storage toegevoegd
In het bestand config.json van uw project, is een element gemaakt met de verbindingsreeks en de sleutel van het geselecteerde opslagaccount.

Zie voor meer informatie [ASP.NET 5](http://www.asp.net/vnext).

