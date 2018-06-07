---
title: Fouten opsporen in Windows-containers met Service Fabric en VS | Microsoft Docs
description: Informatie over het Windows-containers in Azure Service Fabric met behulp van Visual Studio 2017 voor foutopsporing.
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
ms.openlocfilehash: bca33fe187668d38d4451b2de5b9e54d86e40ba9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655250"
---
# <a name="how-to-debug-windows-containers-in-azure-service-fabric-using-visual-studio-2017"></a>Procedure: fouten opsporen in Windows-containers in Azure Service Fabric met behulp van Visual Studio 2017

Met Visual Studio 2017 Update 7 (15,7), kunt u fouten opsporen in .NET-toepassingen in containers als Service Fabric-services. In dit artikel laat zien hoe uw omgeving configureren en vervolgens fouten opsporen in een .NET-toepassing in een container in een lokale Service Fabric-cluster wordt uitgevoerd.

## <a name="prerequisites"></a>Vereisten

* Op Windows 10, volgt u deze snelstartgids voor [configureren Windows 10 om uit te voeren van Windows-containers](https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/quick-start-windows-10)
* Volg deze snelstartgids voor op Windows Server 2016 [Windows 2016 configureren om uit te voeren van Windows-containers](https://docs.microsoft.com/en-us/virtualization/windowscontainers/quick-start/quick-start-windows-server)
* Instellen van uw lokale Service Fabric-omgeving door [uw ontwikkelingsomgeving op Windows voorbereiden](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started)

## <a name="configure-your-developer-environment-to-debug-containers"></a>Uw ontwikkelomgeving voor foutopsporing containers configureren

1. Zorg ervoor dat de Docker voor Windows-service wordt uitgevoerd voordat u doorgaat met de volgende stap.

1. Om DNS-omzetting tussen containers te ondersteunen, wordt er voor het instellen van uw lokaal ontwikkelcluster met de naam van de machine.
    1. Open PowerShell als beheerder
    1. Navigeer naar de installatiemap SDK Cluster meestal `C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup`
    1. Voer het script `DevClusterSetup.ps1` met de parameter `-UseMachineName`

    ``` PowerShell
      C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1 -UseMachineName
    ```

    > [!NOTE]
    > U kunt de `-CreateOneNodeCluster` voor het instellen van een cluster met één knooppunt. De standaardwaarde maakt een lokaal cluster met vijf knooppunten.
    >

    Zie voor meer informatie over de DNS-Service in Service Fabric, [DNS-Service in Azure Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-dnsservice).

### <a name="known-limitations-when-debugging-containers-in-service-fabric"></a>Bekende beperkingen als u fouten opspoort containers in Service Fabric

Hieronder vindt u een lijst met bekende beperkingen met foutopsporing containers in Service Fabric en mogelijke oplossingen:

* Gebruik van localhost voor ClusterFQDNorIP ondersteunen geen DNS-omzetting in containers.
    * Oplossing: Het lokale cluster met behulp van de computernaam (Zie hierboven) instellen
* Windows10 uitgevoerd in een virtuele Machine, krijgt geen DNS-antwoord terug naar de container.
    * Oplossing: UDP-offload van de controlesom voor IPv4 op de virtuele Machines NIC uitschakelen
    * Houd er rekening mee afnemen netwerkprestaties op de machine.
    * https://github.com/Azure/service-fabric-issues/issues/1061
* Het omzetten van services in dezelfde toepassing met behulp van DNS werkt servicenaam niet op Windows10, als de toepassing werd geïmplementeerd met behulp van Docker Compose
    * Oplossing: Servicename.applicationname gebruiken voor het omzetten van de service-eindpunten
    * https://github.com/Azure/service-fabric-issues/issues/1062
* Als u IP-adres voor ClusterFQDNorIP, wordt het wijzigen van de primaire IP-adres op de host DNS-functionaliteit verbroken.
    * Oplossing: Maak het cluster met behulp van het nieuwe primaire IP-adres op de host opnieuw of gebruik de machinenaam van de. Dit is standaard.
* Als het cluster is gemaakt met FQDN-naam kan niet worden omgezet in het netwerk, mislukt de DNS.
    * Oplossing: Maak het lokale cluster met behulp van het primaire IP-adres van de host opnieuw. Dit is standaard.
* Als u fouten opspoort een container, docker-Logboeken alleen worden gebruikt in het venster Visual Studio niet via Service Fabric-API's, met inbegrip van Service Fabric Explorer

## <a name="debug-a-net-application-running-in-docker-containers-on-service-fabric"></a>Fouten opsporen in een .NET-toepassing uitgevoerd in docker-containers op Service Fabric

1. Visual Studio als beheerder uitvoeren.

1. Open een bestaande .NET-toepassing of een nieuwe maken.

1. Met de rechtermuisknop op het project en selecteer **toevoegen -> Container Orchestrator ondersteuning -> Service Fabric**

1. Druk op **F5** foutopsporing van de toepassing te starten.

    Visual Studio biedt ondersteuning voor console- en ASP.NET-projecttypen voor .NET en .NET Core.

## <a name="next-steps"></a>Volgende stappen
Volg deze link voor meer informatie over de functionaliteit van Service Fabric en containers: [Service Fabric-containers overzicht](service-fabric-containers-overview.md).