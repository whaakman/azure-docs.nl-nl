---
title: Wijzigingen in een ASP.NET-project als u verbinding naar Azure Key Vault | Microsoft Docs
description: Hierin wordt beschreven wat gebeurt er met uw ASP.NET-project als u toKey kluis verbinding maakt met behulp van Visual Studio verbonden services.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: a74e4e10681f6b91e028067d8985408b0745dcd2
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060630"
---
# <a name="what-happened-to-my-aspnet-project-visual-studio-key-vault-connected-service"></a>Wat is er gebeurd met mijn ASP.NET-project (Visual Studio Key Vault verbonden service)?

> [!div class="op_single_selector"]
> - [Aan de slag](vs-key-vault-aspnet-get-started.md)
> - [Wat is er gebeurd](vs-key-vault-aspnet-what-happened.md)

In dit artikel geeft de exacte wijzigingen aangebracht in een ASP.NET-project bij het toevoegen van de [Key Vault verbonden service met behulp van Visual Studio](vs-key-vault-add-connected-service.md).

Zie voor meer informatie over het werken met de gekoppelde service [aan de slag](vs-key-vault-aspnet-get-started.md).

## <a name="added-references"></a>Toegevoegde verwijzingen

Is van invloed op de projectverwijzingen voor bestand *.NET en `packages.config` (NuGet-verwijzingen).

| Type | Referentie |
| --- | --- |
| .NET; NuGet | Microsoft.Azure.KeyVault |
| .NET; NuGet | Microsoft.Azure.KeyVault.WebKey |
| .NET; NuGet | Microsoft.Rest.ClientRuntime |
| .NET; NuGet | Microsoft.Rest.ClientRuntime.Azure |

## <a name="added-files"></a>Toegevoegde bestanden

- ConnectedService.json toegevoegd, welke records enkele gegevens over de Connected Service-provider, versie en een koppeling naar de documentatie.

## <a name="project-file-changes"></a>Wijzigingen in bestanden

- Het bestand verbonden Services ItemGroup en ConnectedServices.json toegevoegd.
- Verwijzingen naar de .NET-assembly's die worden beschreven in de [referenties toegevoegd](#added-references) sectie.

## <a name="webconfig-or-appconfig-changes"></a>Web.config of app.config wijzigingen

- De volgende configuratie-items toegevoegd:

    ```xml
    <appSettings>
       <add key="vaultName" value="<your Key Vault name>" />
       <add key="vaultUri" value="<the URI to your Key Vault in Azure>" />
    </appSettings>
    ```

## <a name="changes-on-azure"></a>Wijzigingen in Azure

- Een resourcegroep gemaakt (of een bestaande resourcegroep gebruikt).
- Een Key Vault in de opgegeven resourcegroep gemaakt.

