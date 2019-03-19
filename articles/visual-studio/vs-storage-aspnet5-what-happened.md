---
title: Wat is er gebeurd met mijn ASP.NET 5-project (Visual Studio verbonden services) | Microsoft Docs
description: Hierin wordt beschreven wat er gebeurt nadat de verbinding te maken met Azure storage-account in een Visual Studio ASP.NET 5-project met behulp van Visual Studio verbonden services
services: storage
author: ghogen
manager: douge
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 71a95e1974cbcec9afcc3337eb37275532e1b527
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57999719"
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Wat is er gebeurd met mijn ASP.NET 5-project (Visual Studio Azure Storage verbonden services)?
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

## <a name="connection-string-for-azure-storage-added"></a>Verbindingsreeks voor Azure Storage is toegevoegd
Een element is in het config.json-bestand van uw project gemaakt met de verbindingsreeks en de sleutel van het geselecteerde opslagaccount.

Zie voor meer informatie, [ASP.NET 5](https://www.asp.net/vnext).

