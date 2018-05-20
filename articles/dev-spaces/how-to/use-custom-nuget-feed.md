---
title: Het gebruik van een aangepaste NuGet-kanaal in Azure Dev spaties | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: johnsta
ms.author: johnsta
ms.date: 05/11/2018
ms.topic: article
description: Gebruik een aangepaste NuGet feed te openen en gebruiken van NuGet-pakketten in een Azure-Dev-ruimte.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, containers
manager: ghogen
ms.openlocfilehash: 3badd15bcfd09c97b43744a20c5df05f4ff57e84
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
#  <a name="use-a-custom-nuget-feed-in-an-azure-dev-space"></a>Gebruik een aangepaste NuGet-kanaal in een Azure-Dev-ruimte

Een NuGet-feed biedt een handige manier om op te nemen pakket bronnen in een project. Azure Dev spaties moeten toegang kunnen krijgen tot deze feed in de volgorde voor afhankelijkheden goed worden geïnstalleerd in de Docker-container.

## <a name="set-up-a-nuget-feed"></a>Stel een NuGet-kanaal

Voor het instellen van een NuGet-kanaal:
1. Voeg een [verwijzing van het pakket](https://docs.microsoft.com/en-us/nuget/consume-packages/package-references-in-project-files) in de `*.csproj` bestand onder de `PackageReference` knooppunt.

   ```xml
   <ItemGroup>
       <!-- ... -->
       <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
       <!-- ... -->
   </ItemGroup>
   ```

2. Maak een [NuGet.Config](https://docs.microsoft.com/en-us/nuget/reference/nuget-config-file) bestand in de projectmap.
     * Gebruik de `packageSources` sectie om te verwijzen naar uw locatie feed NuGet. Belangrijk: De NuGet-feed moet openbaar toegankelijk.
     * Gebruik de `packageSourceCredentials` gedeelte referenties voor gebruikersnaam en het wachtwoord configureren. 

   ```xml
   <packageSources>
       <add key="Contoso" value="https://contoso.com/packages/" />
   </packageSources>

   <packageSourceCredentials>
       <Contoso>
           <add key="Username" value="user@contoso.com" />
           <add key="ClearTextPassword" value="33f!!lloppa" />
       </Contoso>
   </packageSourceCredentials>
   ```

3. Als u beheer van broncode:
    - Verwijzing `NuGet.Config` in uw `.gitignore` bestand zodat u niet per ongeluk referenties naar uw opslagplaats bron doorvoert.
    - Open de `azds.yaml` bestand in uw project en zoek de `build` sectie en voeg het volgende fragment om ervoor te zorgen dat de `NuGet.Config` bestand worden gesynchroniseerd naar Azure zodat deze tijdens het maken container installatiekopie gebruikt. (Standaard Azure Dev spaties niet synchroniseren bestanden die overeenkomen met `.gitignore` en `.dockerignore` regels.)

        ```yaml
        build:
        useGitIgnore: true
        ignore:
        - “!NuGet.Config”
        ```


## <a name="next-steps"></a>Volgende stappen

Nadat u de bovenstaande stappen hebt voltooid, de volgende keer dat u uitvoert `azds up` (of druk op `F5` in VSCode of Visual Studio), Azure Dev spaties synchroniseert de `NuGet.Config` bestand naar Azure, die vervolgens wordt gebruikt door `dotnet restore` pakket te installeren afhankelijkheden in de container.

