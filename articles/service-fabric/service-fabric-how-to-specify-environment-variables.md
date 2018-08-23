---
title: Hoe kan ik opgeven omgevingsvariabelen voor services in Azure Service Fabric | Microsoft Docs
description: Ziet u hoe u omgevingsvariabelen voor toepassingen in Service Fabric
documentationcenter: .net
author: mikkelhegn
manager: markfuss
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: f75de635f08ae06db349387a436c636c149ec9f2
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42055689"
---
# <a name="how-to-specify-environment-variables-for-services-in-service-fabric"></a>Het opgeven van omgevingsvariabelen voor services in Service Fabric

Dit artikel ziet u hoe u omgevingsvariabelen voor een service of de container opgeven in Service Fabric.

## <a name="procedure-for-specifying-environment-variables-for-services"></a>Procedure voor het opgeven van omgevingsvariabelen voor services

In dit voorbeeld kunt u een omgevingsvariabele instellen voor een container. Het artikel wordt ervan uitgegaan dat u hebt al een toepassing en service manifest.

1. Open het bestand ServiceManifest.xml.
1. In de `CodePackage` -element, toevoegen van een nieuwe `EnvironmentVariables` element en een `EnvironmentVariable` -element voor elk omgevingsvariabele.

    ```xml
      <CodePackage Name="MyCode" Version="CodeVersion1">
      ...
        <EnvironmentVariables>
          <EnvironmentVariable Name="MyEnvVariable" Value="DefaultValue"/>
          <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentVariables>
      </CodePackage>
    ```

    Omgevingsvariabelen kunnen worden overschreven in het toepassingsmanifest.

1. Als u wilt overschrijven de omgevingsvariabelen in het toepassingsmanifest, gebruikt u de `EnvironmentOverrides` element.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
        <EnvironmentOverrides CodePackageRef="MyCode">
          <EnvironmentVariable Name="MyEnvVariable" Value="OverrideValue"/>
        </EnvironmentOverrides>
      </ServiceManifestImport>
    ```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over een aantal van de belangrijkste concepten die in dit artikel worden besproken, de [beheren van toepassingen voor meerdere omgevingen artikelen](service-fabric-manage-multiple-environment-app-configuration.md).

Zie voor meer informatie over de mogelijkheden van andere app-beheer die beschikbaar in Visual Studio zijn [beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).