---
title: Para meters-configuratie bestanden in azure Service Fabric | Microsoft Docs
description: Meer informatie over het para meters van configuratie bestanden in Service Fabric.
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
ms.openlocfilehash: dad497978de7187177998524db3b2f2ee448c717
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68464786"
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Configuratie bestanden in Service Fabric para meters

In dit artikel leest u hoe u een configuratie bestand in Service Fabric kunt para meters.  Als u nog niet bekend bent met de basis concepten van het beheren van toepassingen voor meerdere omgevingen, lees dan [toepassingen beheren voor meerdere omgevingen](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="procedure-for-parameterizing-configuration-files"></a>Procedure voor parameterizing-configuratie bestanden

In dit voor beeld overschrijft u een configuratie waarde met behulp van para meters in de implementatie van uw toepassing.

1. Open het  *\<bestand MyService > \PackageRoot\Config\Settings.XML* in uw service project.
1. Stel een naam en waarde voor de configuratie parameter in, bijvoorbeeld cache grootte gelijk aan 25, door het volgende XML-bestand toe te voegen:

   ```xml
    <Section Name="MyConfigSection">
      <Parameter Name="CacheSize" Value="25" />
    </Section>
   ```

1. Sla het bestand op en sluit het.
1. Open het  *\<bestand mijn toepassing > \ApplicationPackageRoot\ApplicationManifest.XML* .
1. In het bestand ApplicationManifest. XML declareert u een para meter en standaard waarde `Parameters` in het-element.  Het is aan te bevelen de naam van de service (bijvoorbeeld "MyService") in de parameter naam te vinden.

   ```xml
    <Parameters>
      <Parameter Name="MyService_CacheSize" DefaultValue="80" />
    </Parameters>
   ```
1. Voeg in `ServiceManifestImport` de sectie van het ApplicationManifest. XML-bestand `ConfigOverride` een `ConfigOverrides` -element toe, dat verwijst naar het configuratie pakket, de sectie en de para meter.

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
> Als u een ConfigOverride toevoegt, kiest Service Fabric altijd de para meters van de toepassing of de standaard waarde die is opgegeven in het toepassings manifest.
>
>

## <a name="next-steps"></a>Volgende stappen
Zie [uw service Fabric-toepassingen beheren in Visual Studio](service-fabric-manage-application-in-visual-studio.md)voor informatie over andere mogelijkheden voor het beheren van apps die beschikbaar zijn in Visual Studio.
