---
title: Wijzigingen in een ASP.NET Core project wanneer verbindingen naar Azure Sleutelkluis | Microsoft Docs
description: Hierin wordt beschreven wat er gebeurt met uw project ASP.NET Core wanneer u toKey kluis verbinding maakt met behulp van Visual Studio verbonden services.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: 8b6c590344db2997c1a987da14cabba76cdca83b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787372"
---
# <a name="what-happened-to-my-aspnet-core-project-visual-studio-key-vault-connected-service"></a>Wat is er gebeurd met mijn ASP.NET Core-project (Visual Studio-Sleutelkluis verbonden service)?

> [!div class="op_single_selector"]
> - [Aan de slag](vs-key-vault-aspnet-core-get-started.md)
> - [Wat is er gebeurd](vs-key-vault-aspnet-core-what-happened.md)

In dit artikel identificeert de exacte wijzigingen aangebracht in een ASP.NET-project, bij het toevoegen van de [Sleutelkluis verbonden service met behulp van Visual Studio](vs-key-vault-add-connected-service.md).

Zie voor meer informatie over het werken met de gekoppelde service [aan de slag](vs-key-vault-aspnet-core-get-started.md).

## <a name="added-references"></a>Toegevoegde verwijzingen

Beïnvloedt de projectverwijzingen bestand *.NET en verwijzingen voor NuGet-pakket.

| Type | Referentie |
| --- | --- |
| NuGet | Microsoft.AspNetCore.AzureKeyVault.HostingStartup |

## <a name="added-files"></a>Toegevoegde bestanden

- ConnectedService.json toegevoegd, die enige informatie over de provider Connected-Service, versie en een koppeling van de documentatie registreert.

## <a name="project-file-changes"></a>Wijzigingen in bestanden

- Het bestand verbonden Services ItemGroup en ConnectedServices.json toegevoegd.

## <a name="launchsettingsjson-changes"></a>launchsettings.JSON wijzigingen

- De volgende omgeving variabele items toegevoegd aan het profiel IIS Express en het profiel dat overeenkomt met de naam van uw web-project:

    ```json
      "environmentVariables": {
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED": "true",
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT": "<your keyvault URL>"
      }
    ```

## <a name="changes-on-azure"></a>Wijzigingen in Azure

- Een resourcegroep gemaakt (of een bestaande gebruikt).
- Een Sleutelkluis in de opgegeven resourcegroep gemaakt.

