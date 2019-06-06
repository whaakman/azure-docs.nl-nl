---
title: Fouten opsporen in Windows-containers met Service Fabric en Visual Studio | Microsoft Docs
description: Leer hoe u fouten opsporen in Windows-containers in Azure Service Fabric met behulp van Visual Studio 2019.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/14/2019
ms.author: aljo, mikhegn
ms.openlocfilehash: 15f288d5400b49ec05c9ffb936fd2097cc61bae8
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428145"
---
# <a name="how-to-debug-windows-containers-in-azure-service-fabric-using-visual-studio-2019"></a>Procedure: Fouten opsporen in Windows-containers in Azure Service Fabric met behulp van Visual Studio 2019

Met Visual Studio 2019, kunt u .NET-toepassingen in containers foutopsporing als Service Fabric-services. Dit artikel ziet u hoe u uw omgeving te configureren en vervolgens fouten opsporen in een .NET-toepassing in een container die wordt uitgevoerd in een lokaal Service Fabric-cluster.

## <a name="prerequisites"></a>Vereisten

* Op Windows 10, volgt u deze snelstartgids voor het [Windows 10 configureren voor het uitvoeren van Windows-containers](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10)
* Op Windows Server 2016, volgt u deze snelstartgids voor het [Windows 2016 configureren om uit te voeren van Windows-containers](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-server)
* Instellen van uw lokale Service Fabric-omgeving door [uw ontwikkelomgeving voorbereiden in Windows](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)

## <a name="configure-your-developer-environment-to-debug-containers"></a>Uw ontwikkelomgeving instellen om op te sporen containers configureren

1. Zorg ervoor dat de Docker voor Windows-service wordt uitgevoerd voordat u doorgaat met de volgende stap.

1. Ter ondersteuning van DNS-omzetting tussen containers, hebt u voor het instellen van uw lokale ontwikkelcluster, met behulp van de naam van de machine. Deze stappen zijn ook nodig als u adresservices via de omgekeerde proxy wilt.
   1. Open PowerShell als beheerder
   2. Navigeer naar de installatiemap van de SDK-Cluster, doorgaans `C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup`.
   3. Het script uitvoeren `DevClusterSetup.ps1`

      ``` PowerShell
        C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1
      ```

      > [!NOTE]
      > U kunt de `-CreateOneNodeCluster` voor het instellen van een cluster met één knooppunt. Standaard wordt een lokaal cluster met vijf knooppunten maken.
      >

      Zie voor meer informatie over de DNS-Service in Service Fabric, [DNS-Service in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice). Zie voor meer informatie over het gebruik van Service Fabric omgekeerde proxy van de services die worden uitgevoerd in een container [omgekeerde proxy speciale handelingen voor services die worden uitgevoerd in containers](service-fabric-reverseproxy.md#special-handling-for-services-running-in-containers).

### <a name="known-limitations-when-debugging-containers-in-service-fabric"></a>Bekende beperkingen bij het opsporen van fouten in containers in Service Fabric

Hieronder volgt een lijst met bekende beperkingen met foutopsporing containers in Service Fabric en mogelijke oplossingen:

* Met behulp van ' localhost ' voor ClusterFQDNorIP wordt niet ondersteuning voor DNS-omzetting in containers.
    * Oplossing: Instellen van het lokale cluster met behulp van de naam van de computer (Zie hierboven)
* Windows10 uitgevoerd in een virtuele Machine wordt niet DNS-antwoord terug naar de container ophalen.
    * Oplossing: UDP-controlesom-offload voor IPv4 op de NIC van de virtuele Machines uitschakelen
    * Uitvoeren van Windows10 afnemen netwerken prestaties op de machine.
    * https://github.com/Azure/service-fabric-issues/issues/1061
* Het omzetten van services in dezelfde toepassing met behulp van DNS werkt servicenaam niet op Windows10, als de toepassing is geïmplementeerd met behulp van Docker Compose
    * Oplossing: Servicename.applicationname gebruiken voor het omzetten van service-eindpunten
    * https://github.com/Azure/service-fabric-issues/issues/1062
* Als u IP-adres voor ClusterFQDNorIP, wordt wijzigen van de primaire IP-adres op de host DNS-functionaliteit verbroken.
    * Oplossing: Maak het cluster met behulp van het nieuwe primaire IP-adres op de host opnieuw of gebruik van de naam van de computer. Deze breuk is standaard.
* Als het cluster is gemaakt met FQDN-naam kan niet worden opgelost op het netwerk, mislukt de DNS.
    * Oplossing: Maak het lokale cluster met behulp van de primaire IP-adres van de host opnieuw. Deze fout is standaard.
* Bij het opsporen van fouten in een container, docker-logboeken worden pas beschikbaar in het uitvoervenster van Visual Studio, niet via de Service Fabric-API's, met inbegrip van Service Fabric Explorer

## <a name="debug-a-net-application-running-in-docker-containers-on-service-fabric"></a>Fouten opsporen in een .NET-toepassing uitvoeren in docker-containers in Service Fabric

1. Visual Studio als beheerder uitvoeren.

1. Open een bestaande .NET-toepassing of een nieuwe maken.

1. Met de rechtermuisknop op het project en selecteer **Add -> Orchestrator-ondersteuning voor containers Service Fabric ->**

1. Druk op **F5** foutopsporing van de toepassing te starten.

    Visual Studio biedt ondersteuning voor console- en ASP.NET-projecttypen voor .NET en .NET Core.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de mogelijkheden van Service Fabric en containers, Service Fabric-containers overview](service-fabric-containers-overview.md).
