---
title: Uw Azure Service Fabric-services op Windows in een container plaatsen
description: Leer hoe u uw Service Fabric Reliable Services en Reliable Actors-services op Windows in een container plaatsen.
services: service-fabric
documentationcenter: .net
author: TylerMSFT
manager: anmolah
editor: roroutra
ms.assetid: 0b41efb3-4063-4600-89f5-b077ea81fa3a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/23/2018
ms.author: twhitney, anmola
ms.openlocfilehash: 24ec0de77c796ad2abf8587b7542e53f745c532d
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51298653"
---
# <a name="containerize-your-service-fabric-reliable-services-and-reliable-actors-on-windows"></a>Uw Service Fabric Reliable Services en Reliable Actors op Windows in een container plaatsen

Service Fabric biedt ondersteuning voor Service Fabric microservices beperken (Reliable Services en Reliable Actor op basis van services). Zie voor meer informatie, [service fabric-containers](service-fabric-containers-overview.md).

Dit document bevat richtlijnen voor het ophalen van de service die wordt uitgevoerd binnen een Windows-container.

> [!NOTE]
> Deze functie werkt momenteel alleen voor Windows. Als u wilt uitvoeren van containers, moet het cluster worden uitgevoerd op Windows Server 2016 met Containers.

## <a name="steps-to-containerize-your-service-fabric-application"></a>Stappen voor uw Service Fabric-toepassing in een container plaatsen

1. Open uw Service Fabric-toepassing in Visual Studio.

2. Klasse toevoegen [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) aan uw project. De code in deze klasse is een helper correct laden van de Service Fabric-runtime binaire bestanden in uw toepassing bij het uitvoeren van binnen een container.

3. Voor ieder codepakket die u wilt in een container plaatsen, initialiseren het laadprogramma op de vermelding wordt verwijzen. De statische constructor wordt weergegeven in het volgende codefragment toe aan het bestand programma vermelding punt toevoegen.

  ```csharp
  namespace MyApplication
  {
      internal static class Program
      {
          static Program()
          {
              SFBinaryLoader.Initialize();
          }

          /// <summary>
          /// This is the entry point of the service host process.
          /// </summary>
          private static void Main()
          {
  ```

4. Bouw en [pakket](service-fabric-package-apps.md#Package-App) uw project. Als u wilt bouwen en maken van een pakket, met de rechtermuisknop op het toepassingsproject in Solution Explorer en kies de **pakket** opdracht.

5. Voor elke codepakket moet u in een container plaatsen, voer de PowerShell-script [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1). Het gebruik is als volgt:

    Volledige .NET
      ```powershell
        $codePackagePath = 'Path to the code package to containerize.'
        $dockerPackageOutputDirectoryPath = 'Output path for the generated docker folder.'
        $applicationExeName = 'Name of the Code package executable.'
        CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
      ```
    .NET Core
      ```powershell
        $codePackagePath = 'Path to the code package to containerize.'
        $dockerPackageOutputDirectoryPath = 'Output path for the generated docker folder.'
        $dotnetCoreDllName = 'Name of the Code package dotnet Core Dll.'
        CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -DotnetCoreDllName $dotnetCoreDllName
      ```
      Het script maakt een map met Docker-artefacten op $dockerPackageOutputDirectoryPath. Wijzigen van het gegenereerde bestand Dockerfile naar `expose` alle poorten, installatiescripts enzovoort worden uitgevoerd. op basis van uw behoeften.

6. Vervolgens moet u [bouwen](service-fabric-get-started-containers.md#Build-Containers) en [push](service-fabric-get-started-containers.md#Push-Containers) uw Docker-container-pakket aan uw opslagplaats.

7. De ApplicationManifest.xml en ServiceManifest.xml om toe te voegen uw container-installatiekopie, gegevens-opslagplaats, registerverificatie en poort-naar-host-toewijzing wijzigen. Zie voor het wijzigen van de manifesten [een Azure Service Fabric-containertoepassing maken](service-fabric-get-started-containers.md). De definitie van de code-pakket in het servicemanifest moet worden vervangen door de bijbehorende containerinstallatiekopie. Zorg ervoor dat u de EntryPoint wijzigen in een ContainerHost-type.

  ```xml
<!-- Code package is your service executable. -->
<CodePackage Name="Code" Version="1.0.0">
  <EntryPoint>
    <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
    <ContainerHost>
      <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
    </ContainerHost>
  </EntryPoint>
  <!-- Pass environment variables to your container: -->
</CodePackage>
  ```

8. De poort-naar-host-toewijzing voor de replicatie en service-eindpunt toevoegen Nadat de beide deze poorten worden toegewezen tijdens runtime door Service Fabric, worden de ContainerPort is ingesteld op nul voor het gebruik van de toegewezen poort voor de toewijzing.

 ```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
  </ContainerHostPolicies>
</Policies>
 ```

9. Zie voor het configureren van container-isolatiemodus [isolatiemodus configureren]( https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-containers#configure-isolation-mode). Windows ondersteunt twee isolatiemodi voor containers: proces en Hyper-V. De volgende codefragmenten laten zien hoe de isolatiemodus wordt opgegeven in het manifestbestand van de toepassing.

 ```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code" Isolation="process">
  ...
  </ContainerHostPolicies>
</Policies>
 ```
  ```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
  ...
  </ContainerHostPolicies>
</Policies>
 ```

10. Als u wilt testen van deze toepassing, moet u deze implementeren in een cluster met versie 5.7 of hoger. Voor runtime-versie 6.1 of lager, die u wilt bewerken en bijwerken van de clusterinstellingen voor deze preview-functie. Volg de stappen in dit [artikel](service-fabric-cluster-fabric-settings.md) om toe te voegen van de instelling van de volgende weergegeven.
```
      {
        "name": "Hosting",
        "parameters": [
          {
            "name": "FabricContainerAppsEnabled",
            "value": "true"
          }
        ]
      }
```

11. Volgende [implementeren](service-fabric-deploy-remove-applications.md) het bewerkte toepassingspakket aan dit cluster.

U hebt nu een beperkte Service Fabric-toepassing met uw cluster.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het uitvoeren van [containers in Service Fabric](service-fabric-get-started-containers.md).
* Meer informatie over de [levenscyclus](service-fabric-application-lifecycle.md) van de Service Fabric-toepassing.
