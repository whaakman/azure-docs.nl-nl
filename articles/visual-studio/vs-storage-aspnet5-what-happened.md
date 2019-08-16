---
title: Wat is er gebeurd met mijn ASP.NET 5-project (Visual Studio Connected Services) | Microsoft Docs
description: Beschrijft wat er gebeurt nadat u verbinding hebt gemaakt met een Azure-opslag account in een Visual Studio ASP.NET 5-project met Visual Studio Connected Services
services: storage
author: ghogen
manager: jillfra
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: e29c0302ecd703cb02199df95892e24917baf8e8
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69510770"
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Wat is er gebeurd met mijn ASP.NET 5-project (Visual Studio Azure Storage Connected Services)?
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

Daarnaast is het NuGet-pakket **micro soft. Framework. Configuration. json** toegevoegd.

## <a name="connection-string-for-azure-storage-added"></a>Verbindings reeks voor toegevoegde Azure Storage
In het bestand config. json van uw project is een element gemaakt met de connection string en sleutel van het geselecteerde opslag account.

Zie [ASP.net 5](https://www.asp.net/vnext)voor meer informatie.

