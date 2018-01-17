---
title: Hoe om te voorzien van de configuratiebestanden in Azure Service Fabric | Microsoft Docs
description: Ziet u hoe om te voorzien van de configuratiebestanden in Service Fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: 1e7d59ecb231440711b8ed3dc0b27a2b105890c4
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Hoe om te voorzien van de configuratiebestanden in Service Fabric

In dit artikel leest u hoe om te voorzien van een configuratiebestand in Service Fabric.

## <a name="procedure-for-parameterizing-configuration-files"></a>Procedure voor de configuratiebestanden van parameters voorzien

In dit voorbeeld moet u een configuratiewaarde met behulp van de parameters in de implementatie van uw toepassing overschrijven.

1. Open het bestand Config\Settings.xml.
1. Stel een configuratieparameter door het volgende XML-bestand toe te voegen:

    ```xml
      <Section Name="MyConfigSection">
        <Parameter Name="CacheSize" Value="25" />
      </Section>
    ```

1. Sla en sluit het bestand.
1. Open het `ApplicationManifest.xml`-bestand.
1. Voeg een `ConfigOverride` -element verwijst naar het configuratiepakket, de sectie en de parameter.

      ```xml
        <ConfigOverrides>
          <ConfigOverride Name="Config">
              <Settings>
                <Section Name="MyConfigSection">
                    <Parameter Name="CacheSize" Value="[Stateless1_CacheSize]" />
                </Section>
              </Settings>
          </ConfigOverride>
        </ConfigOverrides>
      ```

1. Nog steeds in het bestand ApplicationManifest.xml, geeft u vervolgens de parameter in de `Parameters` element

    ```xml
      <Parameters>
        <Parameter Name="Stateless1_CacheSize" />
      </Parameters>
    ```

1. En definieer een`DefaultValue`

    ```xml
      <Parameters>
        <Parameter Name="Stateless1_CacheSize" DefaultValue="80" />
      </Parameters>
    ```

> [!NOTE]
> In het geval waar u een ConfigOverride toevoegen, kiest Service Fabric altijd de parameters voor de toepassing of de standaardwaarde opgegeven in het toepassingsmanifest.
>
>

Bij het publiceren van uw toepassing vanuit Visual Studio met behulp van de Cloud.xml publicatieprofiel, wordt de service is geconfigureerd om poort 80 te gebruiken. Als u de toepassing zonder op te geven van de parameter MyWebAPI_PortNumber implementeert, wordt de service poort 8080 gebruikt.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over een aantal van de belangrijkste concepten die worden beschreven in dit artikel, de [beheren van toepassingen voor artikelen in meerdere omgevingen](service-fabric-manage-multiple-environment-app-configuration.md).

Zie voor meer informatie over de mogelijkheden van andere app-beheer die beschikbaar in Visual Studio zijn [beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).