---
title: Fouten opsporen in Windows-containers met Service Fabric en Visual Studio | Microsoft Docs
description: Leer hoe u fouten opsporen in Windows-containers in Azure Service Fabric met behulp van Visual Studio 2017.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/14/2018
ms.author: mikhegn
ms.openlocfilehash: e7ff7ca6d4fb18121dfa282c2c47e140a4c51a84
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39505964"
---
# <a name="how-to-debug-windows-containers-in-azure-service-fabric-using-visual-studio-2017"></a>Hoe: fouten opsporen in Windows-containers in Azure Service Fabric met behulp van Visual Studio 2017

Met Visual Studio 2017 Update 7 (15,7), kunt u .NET-toepassingen in containers foutopsporing als Service Fabric-services. Dit artikel ziet u hoe u uw omgeving te configureren en vervolgens fouten opsporen in een .NET-toepassing in een container die wordt uitgevoerd in een lokaal Service Fabric-cluster.

## <a name="prerequisites"></a>Vereiste onderdelen

* Op Windows 10, volgt u deze snelstartgids voor het [Windows 10 configureren voor het uitvoeren van Windows-containers](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10)
* Op Windows Server 2016, volgt u deze snelstartgids voor het [Windows 2016 configureren om uit te voeren van Windows-containers](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-server)
* Instellen van uw lokale Service Fabric-omgeving door [uw ontwikkelomgeving voorbereiden in Windows](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)

## <a name="configure-your-developer-environment-to-debug-containers"></a>Uw ontwikkelomgeving instellen om op te sporen containers configureren

1. Zorg ervoor dat de Docker voor Windows-service wordt uitgevoerd voordat u doorgaat met de volgende stap.

1. Ter ondersteuning van DNS-omzetting tussen containers, wordt u voor het instellen van uw lokale ontwikkelcluster, hebben de computernaam. Deze stappen zijn ook nodig als u adresservices via de omgekeerde proxy wilt.
    a. Open PowerShell als beheerder b. Navigeer naar de installatiemap van de SDK-Cluster, doorgaans `C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup` c. Voer het script `DevClusterSetup.ps1` met de parameter `-UseMachineName`

    ``` PowerShell
      C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1 -UseMachineName
    ```

    > [!NOTE]
    > U kunt de `-CreateOneNodeCluster` voor het instellen van een cluster met één knooppunt. Standaard wordt een lokaal cluster met vijf knooppunten maken.
    >

    Zie voor meer informatie over de DNS-Service in Service Fabric, [DNS-Service in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice). Zie voor meer informatie over het gebruik van Service Fabric omgekeerde proxy van de services die worden uitgevoerd in een container [omgekeerde proxy speciale handelingen voor services die worden uitgevoerd in containers](service-fabric-reverseproxy.md#special-handling-for-services-running-in-containers).

### <a name="known-limitations-when-debugging-containers-in-service-fabric"></a>Bekende beperkingen bij het opsporen van fouten in containers in Service Fabric

Hieronder volgt een lijst met bekende beperkingen met foutopsporing containers in Service Fabric en mogelijke oplossingen:

* Met behulp van ' localhost ' voor ClusterFQDNorIP wordt geen ondersteuning voor DNS-omzetting in containers.
    * Oplossing: Het lokale cluster met behulp van de naam van de computer (Zie hierboven) instellen
* Windows10 uitgevoerd in een virtuele Machine, krijgt geen DNS-antwoord terug naar de container.
    * Oplossing: UDP-controlesom-offload voor IPv4 op de NIC van de virtuele Machines uitschakelen
    * Houd er rekening mee dat netwerken prestaties op de machine afnemen.
    * https://github.com/Azure/service-fabric-issues/issues/1061
* Het omzetten van services in dezelfde toepassing met behulp van DNS werkt servicenaam niet op Windows10, als de toepassing is geïmplementeerd met behulp van Docker Compose
    * Oplossing: Servicename.applicationname gebruiken voor het omzetten van de service-eindpunten
    * https://github.com/Azure/service-fabric-issues/issues/1062
* Als u IP-adres voor ClusterFQDNorIP, wordt wijzigen van de primaire IP-adres op de host DNS-functionaliteit verbroken.
    * Oplossing: Maak het cluster met behulp van het nieuwe primaire IP-adres op de host opnieuw of gebruik van de naam van de computer. Dit is standaard.
* Als het cluster is gemaakt met FQDN-naam niet omgezet in het netwerk is, mislukt DNS.
    * Oplossing: Het lokale cluster met behulp van de primaire IP-adres van de host opnieuw. Dit is standaard.
* Bij het opsporen van fouten in een container, docker-logboeken worden pas beschikbaar in het uitvoervenster van Visual Studio, niet via de Service Fabric-API's, met inbegrip van Service Fabric Explorer

## <a name="debug-a-net-application-running-in-docker-containers-on-service-fabric"></a>Fouten opsporen in een .NET-toepassing uitvoeren in docker-containers in Service Fabric

1. Visual Studio als beheerder uitvoeren.

1. Open een bestaande .NET-toepassing of een nieuwe maken.

1. Met de rechtermuisknop op het project en selecteer **Add -> Orchestrator-ondersteuning voor containers Service Fabric ->**

1. Druk op **F5** foutopsporing van de toepassing te starten.

    Visual Studio biedt ondersteuning voor console- en ASP.NET-projecttypen voor .NET en .NET Core.

## <a name="next-steps"></a>Volgende stappen
Volg deze koppeling voor meer informatie over de mogelijkheden van Service Fabric en containers: [overzicht van Service Fabric-containers](service-fabric-containers-overview.md).
