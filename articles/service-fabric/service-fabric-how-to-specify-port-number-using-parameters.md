---
title: Het opgeven van het poortnummer van een service met behulp van parameters in Azure Service Fabric | Microsoft Docs
description: Ziet u hoe u kunt parameters gebruiken om op te geven van de poort voor een toepassing in Service Fabric
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
ms.openlocfilehash: d69e02126564388bf045693b9960e6e574307641
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391331"
---
# <a name="how-to-specify-the-port-number-of-a-service-using-parameters-in-service-fabric"></a>Het opgeven van het poortnummer van een service met behulp van parameters in Service Fabric

Dit artikel ziet u hoe u het poortnummer van een service met behulp van parameters in Service Fabric met Visual Studio.

## <a name="procedure-for-specifying-the-port-number-of-a-service-using-parameters"></a>Procedure voor het opgeven van het poortnummer van een service met behulp van parameters

In dit voorbeeld stelt u het poortnummer voor uw asp.net core web-API met behulp van een parameter.

1. Open Visual Studio en maak een nieuwe Service Fabric-toepassing.
1. Kies de Stateless ASP.NET Core-sjabloon.
1. Kies Web-API.
1. Open het bestand ServiceManifest.xml.
1. Noteer de naam van het eindpunt dat is opgegeven voor uw service. De standaardwaarde is `ServiceEndpoint`.
1. Open het bestand ApplicationManifest.xml
1. In de `ServiceManifestImport` -element, toevoegen van een nieuwe `RessourceOverrides` element met een verwijzing naar het eindpunt in het bestand ServiceManifest.xml.

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

1. In de `Endpoint` -element, kunt u nu een kenmerk met een parameter overschrijven. In dit voorbeeld geeft u `Port` en stel deze in op de naam van een parameter met behulp van de vierkante haken - bijvoorbeeld `[MyWebAPI_PortNumber]`

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

1. Nog steeds in het bestand ApplicationManifest.xml geeft u vervolgens de parameter in de `Parameters` element

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
1. Als u wilt een andere poort moet worden gebruikt bij het publiceren naar een extern cluster opgeven, moet u de parameter toevoegen met het poortnummer dat aan dit bestand.

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" Value="80" />
      </Parameters>
    ```

Bij het publiceren van uw toepassing vanuit Visual Studio met behulp van de Cloud.xml publicatieprofiel, wordt uw service is geconfigureerd om poort 80 te gebruiken. Als u de toepassing implementeren zonder de parameter MyWebAPI_PortNumber op te geven, gebruikt de service poort 8080.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over een aantal van de belangrijkste concepten die in dit artikel worden besproken, de [beheren van toepassingen voor meerdere omgevingen artikelen](service-fabric-manage-multiple-environment-app-configuration.md).

Zie voor meer informatie over de mogelijkheden van andere app-beheer die beschikbaar in Visual Studio zijn [beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).
