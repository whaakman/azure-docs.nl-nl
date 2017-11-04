---
title: Hoe containerize van uw Azure Service Fabric-microservices (preview)
description: Azure Service Fabric is nieuwe functionaliteit om uw Service Fabric-microservices containerize toegevoegd. Deze functie is momenteel beschikbaar als preview-product.
services: service-fabric
documentationcenter: .net
author: anmolah
manager: anmolah
editor: anmolah
ms.assetid: 0b41efb3-4063-4600-89f5-b077ea81fa3a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/04/2017
ms.author: anmola
ms.openlocfilehash: 6f8ad0bad8d1ae861e6b72f7e1a32ab0675813c2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-containerize-your-service-fabric-reliable-services-and-reliable-actors-preview"></a>Hoe containerize uw Service Fabric Reliable Services en Reliable Actors (Preview)

Service Fabric ondersteunt containerizing Service Fabric-microservices (Reliable Services en betrouwbare op basis van actorservices). Zie voor meer informatie [service fabric-containers](service-fabric-containers-overview.md).


 Deze functie is een Preview-versie en dit artikel bevat de verschillende stappen om uw service uitgevoerd binnen een container.  

> [!NOTE]
> Deze functie is Preview-versie en wordt niet ondersteund in productie. Deze functie werkt momenteel alleen voor Windows.

## <a name="steps-to-containerize-your-service-fabric-application"></a>Stappen voor uw Service Fabric-toepassing containerize

1. Open uw Service Fabric-toepassing in Visual Studio.

2. Klasse toevoegen [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) aan uw project. De code in deze klasse is een helper correct laden van de Service Fabric-runtime binaire bestanden in uw toepassing bij uitvoering binnen een container.

3. Voor elk codepakket dat u wilt containerize, initialiseren het laadprogramma op de programmavermelding verwijzen. De statische constructor weergegeven in het volgende codefragment aan het programma vermelding punt bestand toevoegen.

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

4. Opbouwen en [pakket](service-fabric-package-apps.md#Package-App) uw project. Voor het bouwen en maak een pakket, met de rechtermuisknop op de application-project in Solution Explorer en kiest u de **pakket** opdracht.

5. Voor elke codepakket moet u containerize, voer de PowerShell-script [CreateDockerPackage.ps1](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1). Het gebruik is als volgt:
  ```powershell
    $codePackagePath = 'Path to the code package to containerize.'
    $dockerPackageOutputDirectoryPath = 'Output path for the generated docker folder.'
    $applicationExeName = 'Name of the ode package executable.'
    CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
 ```
  Het script maakt een map met Docker-artefacten op $dockerPackageOutputDirectoryPath. Wijzig de gegenereerde Dockerfile naar eventuele poorten beschikbaar, voert u setup-scripts op basis van de behoeften van uw enzovoort.

6. Vervolgens moet u [bouwen](service-fabric-get-started-containers.md#Build-Containers) en [push](service-fabric-get-started-containers.md#Push-Containers) uw Docker-container-pakket naar uw opslagplaats.

7. Wijzig de ApplicationManifest.xml en ServiceManifest.xml als uw installatiekopie van de container, opslagplaats informatie, register-verificatie en toewijzing van de poort-naar-host wilt toevoegen. Zie voor het wijzigen van de manifesten [maken van een toepassing Azure Service Fabric-container](service-fabric-get-started-containers.md). De pakketdefinitie code in het servicemanifest moet worden vervangen door de bijbehorende container installatiekopie. Zorg ervoor dat het ingangspunt wijzigen in een ContainerHost-type.

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

8. De toewijzing van de poort-naar-host voor de replicator en service-eindpunt toevoegen. Omdat beide deze poorten worden toegewezen tijdens runtime door Service Fabric, worden de ContainerPort is ingesteld op nul wordt op de toegewezen poort gebruiken voor de toewijzing.

 ```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
  </ContainerHostPolicies>
</Policies>
 ```

9. Testen van deze toepassing, moet u deze implementeren naar een cluster dat versie 5.7 of hoger wordt uitgevoerd. Bovendien hoeft te bewerken en bijwerken van de clusterinstellingen voor deze preview-functie. Volg de stappen in dit [artikel](service-fabric-cluster-fabric-settings.md) om toe te voegen van de instelling van de volgende weergegeven.
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
10. Volgende [implementeren](service-fabric-deploy-remove-applications.md) het bewerkte toepassingspakket aan dit cluster.

U hebt nu een beperkte Service Fabric-toepassing die het cluster wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het uitvoeren van [containers in Service Fabric](service-fabric-get-started-containers.md).
* Meer informatie over de [levenscyclus](service-fabric-application-lifecycle.md) van de Service Fabric-toepassing.
