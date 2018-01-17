---
title: Procedures voor omgevingsvariabelen voor services in Azure Service Fabric opgeven | Microsoft Docs
description: Ziet u het gebruik van omgevingsvariabelen voor toepassingen in Service Fabric
documentationcenter: .net
author: mikkelhegn
manager: markfuss
editor: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: d487eeadde9f9a45549763863f8fe5b06b2945a4
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-specify-environment-variables-for-services-in-service-fabric"></a>Het opgeven van omgevingsvariabelen voor services in Service Fabric

In dit artikel leest u hoe het opgeven van omgevingsvariabelen voor een service of de container in Service Fabric.

## <a name="procedure-for-specifying-environment-variables-for-services"></a>Procedure voor het opgeven van omgevingsvariabelen voor services

In dit voorbeeld stelt u een omgevingsvariabele voor een container. Het artikel wordt ervan uitgegaan dat u hebt al een toepassing en service manifest.

1. Open het bestand ServiceManifest.xml.
1. In de `CodePackage` element, Voeg een nieuwe `EnvironmentVariables` element en een `EnvironmentVariable` element voor elk omgevingsvariabele.

    ```xml
      <CodePackage Name="MyCode" Version="CodeVersion1">
      ...
        <EnvironmentVariables>
          <EnvironmentVariable Name="MyEnvVariable" Value="DeafultValue"/>
          <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentVariables>
      </CodePackage>
    ```

    Omgevingsvariabelen kunnen worden overschreven in het toepassingsmanifest.

1. U kunt de omgevingsvariabelen in het toepassingsmanifest overschrijven, met de `EnvironmentOverrides` element.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
        <EnvironmentOverrides CodePackageRef="MyCode">
          <EnvironmentVariable Name="MyEnvVariable" Value="OverrideValue"/>
        </EnvironmentOverrides>
      </ServiceManifestImport>
    ```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over een aantal van de belangrijkste concepten die worden beschreven in dit artikel, de [beheren van toepassingen voor artikelen in meerdere omgevingen](service-fabric-manage-multiple-environment-app-configuration.md).

Zie voor meer informatie over de mogelijkheden van andere app-beheer die beschikbaar in Visual Studio zijn [beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).