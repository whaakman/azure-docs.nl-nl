---
title: Het poortnummer van een service met behulp van de parameters in Azure Service Fabric opgeven | Microsoft Docs
description: Ziet u hoe u parameters gebruiken om op te geven van de poort voor een toepassing in Service Fabric
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
ms.openlocfilehash: 06cfb375c6c18082a0d0316cfcb742a7779fc8a8
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="how-to-specify-the-port-number-of-a-service-using-parameters-in-service-fabric"></a>Het poortnummer van een service met behulp van de parameters in Service Fabric opgeven

Dit artikel ziet u hoe u het poortnummer van een service met behulp van de parameters in Service Fabric opgeven met behulp van Visual Studio.

## <a name="procedure-for-specifying-the-port-number-of-a-service-using-parameters"></a>Procedure voor het opgeven van het poortnummer van een service met parameters

In dit voorbeeld stelt u het poortnummer voor uw web-API met behulp van een parameter van asp.net core.

1. Open Visual Studio en maak een nieuwe Service Fabric-toepassing.
1. Kies een sjabloon voor de staatloze ASP.NET Core.
1. Kies de Web-API.
1. Open het bestand ServiceManifest.xml.
1. Noteer de naam van het eindpunt dat is opgegeven voor uw service. De standaardwaarde is `ServiceEndpoint`.
1. Open het bestand ApplicationManifest.xml
1. In de `ServiceManifestImport` element, Voeg een nieuwe `RessourceOverrides` element met een verwijzing naar het eindpunt in uw bestand ServiceManifest.xml.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
        <ResourceOverrides>
          <Endpoints>
            <Endpoint Name="ServiceEndpoint"/>
          </Endpoints>
        </ResourceOverrides>
        <ConfigOverrides />
      </ServiceManifestImport>
    ```

1. In de `Endpoint` element, kunt u nu een kenmerk met een parameter overschrijven. In dit voorbeeld u `Port` en wordt ingesteld op een parameternaam met vierkante haken - bijvoorbeeld `[MyWebAPI_PortNumber]`

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
        <ResourceOverrides>
          <Endpoints>
            <Endpoint Name="ServiceEndpoint" Port="[MyWebAPI_PortNumber]"/>
          </Endpoints>
        </ResourceOverrides>
        <ConfigOverrides />
      </ServiceManifestImport>
    ```

1. Nog steeds in het bestand ApplicationManifest.xml, geeft u vervolgens de parameter in de `Parameters` element

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" />
      </Parameters>
    ```

1. En definieer een `DefaultValue`

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" DefaultValue="8080" />
      </Parameters>
    ```

1. Open de map ApplicationParameters en de `Cloud.xml` bestand
1. Geef een andere poort moet worden gebruikt bij het publiceren naar een externe cluster door moet u de parameter toevoegen met het poortnummer aan dit bestand.

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" DefaultValue="80" />
      </Parameters>
    ```

Bij het publiceren van uw toepassing vanuit Visual Studio met behulp van de Cloud.xml publicatieprofiel, wordt de service is geconfigureerd om poort 80 te gebruiken. Als u de toepassing zonder op te geven van de parameter MyWebAPI_PortNumber implementeert, wordt de service poort 8080 gebruikt.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over een aantal van de belangrijkste concepten die worden beschreven in dit artikel, de [beheren van toepassingen voor artikelen in meerdere omgevingen](service-fabric-manage-multiple-environment-app-configuration.md).

Zie voor meer informatie over de mogelijkheden van andere app-beheer die beschikbaar in Visual Studio zijn [beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).