---
title: Wat is er gebeurd met mijn Cloud service project? | Microsoft Docs
description: Beschrijft wat er gebeurt in een Cloud Services-project nadat u verbinding hebt gemaakt met een Azure-opslag account met behulp van Visual Studio Connected Services
services: storage
author: ghogen
manager: jillfra
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: d96e523b6e4e060eaef33de0f961e7020e5dfb57
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69510631"
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Wat is er gebeurd met mijn Cloud Services-project (Visual Studio Azure Storage Connected service)?
## <a name="references-added"></a>Toegevoegde verwijzingen
Het NuGet-pakket van Azure Storage is toegevoegd aan uw Visual Studio-project.  
Dit pakket voegt de volgende .NET-verwijzingen toe:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Verbindings reeks voor toegevoegde Azure Storage
Elementen zijn gemaakt met de connection string en sleutel van het geselecteerde opslag account. Er zijn wijzigingen aangebracht in de volgende bestanden:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**

