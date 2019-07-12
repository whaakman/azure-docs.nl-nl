---
title: Azure Functions instellen voor Azure aangepaste Providers
description: In deze zelfstudie gaat over het maken van een Azure-functie en instellen voor gebruik met Azure aangepaste Providers
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: d7e4de43659db88bfd9aad40cc3b9f1753189bba
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799992"
---
# <a name="setup-azure-functions-for-azure-custom-providers"></a>Azure Functions instellen voor Azure aangepaste Providers

Aangepaste providers kunnen u werkstromen in Azure aanpassen. Een aangepaste provider is een overeenkomst tussen Azure en een `endpoint`. In deze zelfstudie doorloopt het proces van het instellen van een Azure-functie om te werken als een aangepaste provider `endpoint`.

In deze zelfstudie is onderverdeeld in de volgende stappen uit:

- Het maken van de Azure-functie
- Installeren van Azure Table-bindingen
- Bijwerken van de RESTful HTTP-methoden
- Azure Resource Manager NuGet-pakketten toevoegen

In deze zelfstudie bouwt op de volgende zelfstudies:

- [Het maken van uw eerste Azure-functie via Azure portal](../azure-functions/functions-create-first-azure-function.md)

## <a name="creating-the-azure-function"></a>Het maken van de Azure-functie

> [!NOTE]
> In deze zelfstudie, wordt er een eenvoudige service-eindpunt met behulp van een Azure-functie, maar een aangepaste provider kunt een openbaar toegankelijke `endpoint`. Azure Logic Apps, Azure API Management en Azure Web Apps zijn een goede alternatieven.

Voor deze zelfstudie begint, moet u de zelfstudie volgen [het maken van uw eerste Azure-functie in Azure portal](../azure-functions/functions-create-first-azure-function.md). De zelfstudie maakt een .NET core-webhookfunctie die kan worden gewijzigd in de Azure-portal.

## <a name="install-azure-table-bindings"></a>Installeren van Azure Table-bindingen

In deze sectie gaat via snelle stappen voor het installeren van de Azure Table storage-bindingen.

1. Navigeer naar de `Integrate` tabblad voor de HttpTrigger.
2. Klik op de `+ New Input`.
3. Selecteer `Azure Table Storage`.
4. Installeer de `Microsoft.Azure.WebJobs.Extensions.Storage` als deze nog niet is geïnstalleerd.
5. Update de `Table parameter name` naar 'tableStorage' en de `Table name` naar 'myCustomResources'.
6. Sla de bijgewerkte invoerparameter.

![Overzicht van de aangepaste provider](./media/create-custom-providers/azure-functions-table-bindings.png)

## <a name="update-restful-http-methods"></a>Bijwerken van de RESTful HTTP-methoden

In deze sectie gaat via snelle stappen voor het instellen van de Azure-functie om op te nemen van de RESTful-aanvraagmethoden van aangepaste provider.

1. Navigeer naar de `Integrate` tabblad voor de HttpTrigger.
2. Update de `Selected HTTP methods` aan: GET, POST, DELETE en PUT.

![Overzicht van de aangepaste provider](./media/create-custom-providers/azure-functions-http-methods.png)

## <a name="modifying-the-csproj"></a>De csproj wijzigen

> [!NOTE]
> Als de csproj ontbreekt uit de map, wordt deze kan handmatig worden toegevoegd of worden één keer wordt weergegeven de `Microsoft.Azure.WebJobs.Extensions.Storage` extensie is geïnstalleerd op de functie.

Vervolgens wordt het csproj-bestand bevatten handige NuGet-bibliotheken die het maakt het eenvoudiger om te parseren van binnenkomende aanvragen van aangepaste providers bijgewerkt. Volg de stappen in [extensies toevoegen via de portal](../azure-functions/install-update-binding-extensions-manual.md) en het csproj om op te nemen van de volgende verwijzingen van pakket bijwerken:

```xml
<PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
<PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
<PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
```

Voorbeeld csproj-bestand:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <WarningsAsErrors />
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
    <PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
    <PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
  </ItemGroup>
</Project>
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een Azure-functie om te werken als een aangepaste Azure Provider setup `endpoint`. Ga naar het volgende artikel voor informatie over het ontwerpen van een RESTful aangepaste provider `endpoint`.

- [Zelfstudie: Het ontwerpen van een aangepaste provider RESTful-eindpunt](./tutorial-custom-providers-function-authoring.md)
