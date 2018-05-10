---
title: Wijzigingen in een ASP.NET-project wanneer verbindingen naar Azure Sleutelkluis | Microsoft Docs
description: Hierin wordt beschreven wat er gebeurt met uw ASP.NET-project wanneer u toKey kluis verbinding maakt met behulp van Visual Studio verbonden services.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: a43f893c7ee87ffb02179c06ea5786715547e93a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="what-happened-to-my-aspnet-project-visual-studio-key-vault-connected-service"></a>Wat is er gebeurd met mijn ASP.NET-project (Visual Studio-Sleutelkluis verbonden service)?

> [!div class="op_single_selector"]
> - [Aan de slag](vs-key-vault-aspnet-get-started.md)
> - [Wat is er gebeurd](vs-key-vault-aspnet-what-happened.md)

In dit artikel identificeert de exacte wijzigingen aangebracht in een ASP.NET-project, bij het toevoegen van de [Sleutelkluis verbonden service met behulp van Visual Studio](vs-key-vault-add-connected-service.md).

Zie voor meer informatie over het werken met de gekoppelde service [aan de slag](vs-key-vault-aspnet-get-started.md).

## <a name="added-references"></a>Toegevoegde verwijzingen

Is van invloed op het project *.NET bestandsverwijzingen en `packages.config` (NuGet-verwijzingen).

| Type | Referentie |
| --- | --- |
| .NET; NuGet | Microsoft.Azure.KeyVault |
| .NET; NuGet | Microsoft.Azure.KeyVault.WebKey |
| .NET; NuGet | Microsoft.Rest.ClientRuntime |
| .NET; NuGet | Microsoft.Rest.ClientRuntime.Azure |

## <a name="added-files"></a>Toegevoegde bestanden

- ConnectedService.json toegevoegd, die enige informatie over de provider Connected-Service, versie en een koppeling naar de documentatie registreert.

## <a name="project-file-changes"></a>Wijzigingen in bestanden

- Het bestand verbonden Services ItemGroup en ConnectedServices.json toegevoegd.
- Verwijzingen naar de .NET-assembly's beschreven in de [referenties toegevoegd](#added-references) sectie.

## <a name="webconfig-or-appconfig-changes"></a>wijzigingen in web.config of app.config

- De volgende configuratie-items toegevoegd:

    ```xml
    <appSettings>
       <add key="vaultName" value="<your Key Vault name>" />
       <add key="vaultUri" value="<the URI to your Key Vault in Azure>" />
    </appSettings>
    ```

## <a name="changes-on-azure"></a>Wijzigingen in Azure

- Een resourcegroep gemaakt (of een bestaande gebruikt).
- Een Sleutelkluis in de opgegeven resourcegroep gemaakt.

