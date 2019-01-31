---
title: Het gebruik van een aangepaste NuGet-kanaal in Azure Dev Spaces | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: johnsta
ms.author: johnsta
ms.date: 05/11/2018
ms.topic: article
description: Gebruik een aangepaste NuGet feed te openen en gebruiken van NuGet-pakketten in een Azure Dev Space.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, containers
manager: ghogen
ms.openlocfilehash: 21a70100fe186e176dfe8eb7c247d83a5d4705bd
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55466401"
---
#  <a name="use-a-custom-nuget-feed-in-an-azure-dev-space"></a>Gebruik een aangepaste NuGet-feed in een Azure Dev Space

Een NuGet-feed biedt een gemakkelijke manier om pakketbronnen opnemen in een project. Azure Dev Spaces moet toegang krijgen tot deze feed opdat afhankelijkheden correct worden geïnstalleerd in de Docker-container.

## <a name="set-up-a-nuget-feed"></a>Instellen van een NuGet-feed

Een NuGet-feed instellen:
1. Voeg een [pakket verwijzing](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) in de `*.csproj` bestand onder het `PackageReference` knooppunt.

   ```xml
   <ItemGroup>
       <!-- ... -->
       <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
       <!-- ... -->
   </ItemGroup>
   ```

2. Maak een [NuGet.Config](https://docs.microsoft.com/nuget/reference/nuget-config-file) bestand in de projectmap.
     * Gebruik de `packageSources` sectie om te verwijzen naar uw NuGet-feedlocatie. Belangrijk: De feed NuGet moet openbaar toegankelijk zijn.
     * Gebruik de `packageSourceCredentials` sectie om de gebruikersnaam en het wachtwoord configureren. 

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

3. Als u broncodebeheer gebruikt:
    - Neem `NuGet.Config` op in uw `.gitignore` bestand, zodat u niet per ongeluk toegangsreferenties in uw versiebeheersysteem vastlegt.
    - Open het bestand `azds.yaml` in uw project, en ga naar de sectie `build`. Voeg hier het volgende fragment toe om ervoor te zorgen dat het bestand `NuGet.Config`wordt gesynchroniseerd naar Azure zodat deze tijdens het bouwproces van container-installatiekopie wordt gebruikt. Standaard worden in Azure Dev Spaces geen bestanden gesynchroniseerd die overeenkomen met `.gitignore` en `.dockerignore` regels.)

        ```yaml
        build:
        useGitIgnore: true
        ignore:
        - “!NuGet.Config”
        ```


## <a name="next-steps"></a>Volgende stappen

Nadat u de bovenstaande stappen hebt voltooid, zal de volgende keer dat u `azds up` uitvoert (of op `F5` drukt in VSCode of Visual Studio) Azure Dev Spaces het bestand `NuGet.Config` synchroniseren naar Azure, waar het vervolgens wordt gebruikt door `dotnet restore` om pakketafhankelijkheden in de container te installeren.

