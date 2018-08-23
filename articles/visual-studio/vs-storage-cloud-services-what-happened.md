---
title: Wat is er gebeurd met mijn cloudserviceproject? | Microsoft Docs
description: Hierin wordt beschreven wat er gebeurt in een cloud services-project nadat de verbinding te maken met Azure storage-account met behulp van Visual Studio verbonden services
services: storage
author: ghogen
manager: douge
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: e823bc16c500dfee44312a774d5e3bd6622e5fae
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054706"
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Wat is er gebeurd met mijn cloudserviceproject (Visual Studio Azure Storage verbonden service)?
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

## <a name="connection-string-for-azure-storage-added"></a>Verbindingsreeks voor Azure Storage is toegevoegd
Elementen zijn gemaakt met de verbindingsreeks en de sleutel van het geselecteerde opslagaccount. Wijzigingen zijn aangebracht in de volgende bestanden:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**

