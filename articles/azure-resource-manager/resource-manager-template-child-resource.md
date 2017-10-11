---
title: "Onderliggende resource definiëren in Azure-sjabloon | Microsoft Docs"
description: Laat zien hoe u het brontype en de naam voor onderliggende resource in een Azure Resource Manager-sjabloon is ingesteld
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: tomfitz
ms.openlocfilehash: 5b6ce5526f354008eb4a697deec737876f22391f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a>De naam en type voor onderliggende resource in het Resource Manager-sjabloon instellen
Wanneer u een sjabloon maakt, moet u vaak een onderliggende resource die is gerelateerd aan een bovenliggende resource bevatten. De sjabloon kan bijvoorbeeld een SQL-server en een database. De SQL-server is de bovenliggende resource en de database is de onderliggende resource. 

De indeling van het onderliggende brontype is:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

De indeling van de naam van de onderliggende resource is:`{parent-resource-name}/{child-resource-name}`

Echter, geeft u het type en de naam in een sjabloon anders op basis van of deze is genest binnen de bovenliggende resource, of op een eigen op het hoogste niveau. Dit onderwerp leest hoe u voor het afhandelen van beide benaderingen.

Bij het maken van een volledig gekwalificeerde verwijzing naar een resource wordt de volgorde te combineren segmenten van het type en de naam is niet gewoon een samenvoeging van de twee.  Gebruik in plaats daarvan na de naamruimte, een reeks *typenaam/* paren van minst specifiek voor het meest specifiek:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Bijvoorbeeld:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`juist `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` is niet correct

## <a name="nested-child-resource"></a>Geneste onderliggende resource
De eenvoudigste manier om te definiëren van een onderliggende resource is naar binnen de bovenliggende resource worden genest. Het volgende voorbeeld ziet een SQL-database genest in een SQL-Server.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

Voor de onderliggende resource, het type is ingesteld op `databases` , maar het type volledige resource `Microsoft.Sql/servers/databases`. U geen specifieke `Microsoft.Sql/servers/` omdat ervan wordt uitgegaan van het type van de bovenliggende resource. De naam van de onderliggende resource is ingesteld op `exampledatabase` , maar de naam van de volledige naam van het bovenliggende bevat. U geen specifieke `exampleserver` omdat ervan wordt uitgegaan van de bovenliggende resource.

## <a name="top-level-child-resource"></a>Op het hoogste niveau van onderliggende resource
U kunt de onderliggende resource op het hoogste niveau definiëren. U kunt deze benadering gebruiken als de bovenliggende resource niet is geïmplementeerd in dezelfde sjabloon of wilt gebruiken `copy` meerdere onderliggende resources te maken. Met deze methode moet u het volledige brontype bieden en de naam van de bovenliggende resource opnemen in de naam van de onderliggende resource.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

De database is een onderliggende resource met de server, zelfs als ze op hetzelfde niveau in de sjabloon zijn gedefinieerd.

## <a name="next-steps"></a>Volgende stappen
* Zie voor aanbevelingen over het maken van sjablonen [aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](resource-manager-template-best-practices.md).
* Zie voor een voorbeeld van het maken van meerdere onderliggende resources [implementeren van meerdere exemplaren van resources in Azure Resource Manager-sjablonen](resource-group-create-multiple.md).
