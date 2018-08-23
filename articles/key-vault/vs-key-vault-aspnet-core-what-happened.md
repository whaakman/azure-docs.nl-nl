---
title: Wijzigingen in een ASP.NET Core-project als u verbinding naar Azure Key Vault | Microsoft Docs
description: Hierin wordt beschreven wat gebeurt er met uw ASP.NET Core-project als u toKey kluis verbinding maakt met behulp van Visual Studio verbonden services.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: b7cbe55fa3a524965e0ebc16c5ff350a60d6e440
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056685"
---
# <a name="what-happened-to-my-aspnet-core-project-visual-studio-key-vault-connected-service"></a>Wat is er gebeurd met mijn ASP.NET Core-project (Visual Studio Key Vault verbonden service)?

> [!div class="op_single_selector"]
> - [Aan de slag](vs-key-vault-aspnet-core-get-started.md)
> - [Wat is er gebeurd](vs-key-vault-aspnet-core-what-happened.md)

In dit artikel geeft de exacte wijzigingen aangebracht in een ASP.NET-project bij het toevoegen van de [Key Vault verbonden service met behulp van Visual Studio](vs-key-vault-add-connected-service.md).

Zie voor meer informatie over het werken met de gekoppelde service [aan de slag](vs-key-vault-aspnet-core-get-started.md).

## <a name="added-references"></a>Toegevoegde verwijzingen

Is van invloed op de projectverwijzingen bestand *.NET en verwijzingen naar NuGet-pakket.

| Type | Referentie |
| --- | --- |
| NuGet | Microsoft.AspNetCore.AzureKeyVault.HostingStartup |

## <a name="added-files"></a>Toegevoegde bestanden

- ConnectedService.json toegevoegd, welke records enkele gegevens over de Connected Service-provider, versie en een koppeling van de documentatie.

## <a name="project-file-changes"></a>Wijzigingen in bestanden

- Het bestand verbonden Services ItemGroup en ConnectedServices.json toegevoegd.

## <a name="launchsettingsjson-changes"></a>launchsettings.JSON wijzigingen

- De volgende omgeving variabele vermeldingen toegevoegd aan zowel de IIS Express-profiel als het profiel dat overeenkomt met de naam van uw web-project:

    ```json
      "environmentVariables": {
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED": "true",
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT": "<your keyvault URL>"
      }
    ```

## <a name="changes-on-azure"></a>Wijzigingen in Azure

- Een resourcegroep gemaakt (of een bestaande resourcegroep gebruikt).
- Een Key Vault in de opgegeven resourcegroep gemaakt.

