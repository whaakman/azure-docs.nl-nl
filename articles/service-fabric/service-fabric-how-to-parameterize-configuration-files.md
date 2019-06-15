---
title: Parameteriseren configuratiebestanden in Azure Service Fabric | Microsoft Docs
description: Leer hoe u om te voorzien van de configuratiebestanden in Service Fabric.
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/09/2018
ms.author: mikhegn
ms.openlocfilehash: 3d03ca5cec2cef67862c2678b3b0a8f17b413787
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60482434"
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Hoe om te voorzien van de configuratiebestanden in Service Fabric

In dit artikel laat u zien hoe om te voorzien van een configuratiebestand in Service Fabric.  Als u nog niet bekend bent met de belangrijkste concepten van het beheer van toepassingen voor meerdere omgevingen lezen [toepassingen voor meerdere omgevingen beheren](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="procedure-for-parameterizing-configuration-files"></a>Procedure voor het parametriseren-configuratiebestanden

In dit voorbeeld moet u de configuratiewaarde van een met behulp van parameters in de implementatie van uw toepassing overschrijven.

1. Open de  *\<MijnService > \PackageRoot\Config\Settings.xml* bestand in uw serviceproject.
1. Stel een naam van de configuratie en de waarde, bijvoorbeeld cachegrootte gelijk is aan 25, door het volgende XML-bestand toe te voegen:

   ```xml
    <Section Name="MyConfigSection">
      <Parameter Name="CacheSize" Value="25" />
    </Section>
   ```

1. Sla het bestand op en sluit het.
1. Open de  *\<Mijntoepassing > \ApplicationPackageRoot\ApplicationManifest.xml* bestand.
1. In het bestand ApplicationManifest.xml declareert u een waarde voor parameter en standaard in de `Parameters` element.  Het wordt aanbevolen dat de parameternaam de naam van de service (bijvoorbeeld ' MyService') bevat.

   ```xml
    <Parameters>
      <Parameter Name="MyService_CacheSize" DefaultValue="80" />
    </Parameters>
   ```
1. In de `ServiceManifestImport` sectie van het bestand ApplicationManifest.xml toevoegen een `ConfigOverride` -element verwijst naar het configuratiepakket voor de sectie en de parameter.

   ```xml
    <ConfigOverrides>
      <ConfigOverride Name="Config">
          <Settings>
            <Section Name="MyConfigSection">
                <Parameter Name="CacheSize" Value="[MyService_CacheSize]" />
            </Section>
          </Settings>
      </ConfigOverride>
    </ConfigOverrides>
   ```

> [!NOTE]
> In het geval waarin u een ConfigOverride toevoegen, kiest Service Fabric altijd de parameters voor de toepassing of de standaardwaarde die is opgegeven in het toepassingsmanifest.
>
>

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de mogelijkheden van andere app-beheer die beschikbaar in Visual Studio zijn [beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).