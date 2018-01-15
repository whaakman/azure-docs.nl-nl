---
title: Overzicht van Service Fabric en containers | Microsoft Docs
description: Een overzicht van Service Fabric en het gebruik van containers microservice toepassingen implementeren. In dit artikel biedt een overzicht van hoe de containers kunnen worden gebruikt en de beschikbare functies in Service Fabric.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: c98b3fcb-c992-4dd9-b67d-2598a9bf8aab
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/20/2017
ms.author: msfussell
ms.openlocfilehash: 412107db2dc446eb5a6a433bfb7fc3bc5e760c27
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/13/2018
---
# <a name="service-fabric-and-containers"></a>Service Fabric en containers
> [!NOTE]
> Containers implementeren naar een Service Fabric-cluster in Windows 10 of met Docker CE wordt niet ondersteund. 
>   

> [!NOTE]
> Service Fabric versie 6.1 heeft preview-ondersteuning voor Windows Server versie 1709. Open netwerk- en DNS-Service voor Service Fabric werken niet met Windows Server versie 1709. 
> 

## <a name="introduction"></a>Inleiding
Azure Service Fabric is een [orchestrator](service-fabric-cluster-resource-manager-introduction.md) van services in een cluster van machines met jaar gebruiks- en optimalisatie in grootschalige services bij Microsoft. Services kunnen worden ontwikkeld op tal van manieren van het gebruik van de [Service Fabric modellen programming](service-fabric-choose-framework.md) tot het implementeren van [Gast uitvoerbare bestanden](service-fabric-deploy-existing-app.md). Standaard wordt Service Fabric implementeert en deze services als processen wordt geactiveerd. Processen bieden de snelste activation en hoogste dichtheid gebruik van de resources in een cluster. Service Fabric kunnen ook services implementeren in container-installatiekopieën. Belangrijker nog, kunt u services in processen en -services in containers in dezelfde toepassing elkaar.   

## <a name="what-are-containers"></a>Wat zijn containers?
Containers zijn ingekapselde, afzonderlijk implementeerbare onderdelen die worden uitgevoerd als geïsoleerde gevallen op de dezelfde kernel om te profiteren van virtualisatie die een besturingssysteem biedt. Dus elke toepassing en de runtime, afhankelijkheden en system-bibliotheken worden uitgevoerd binnen een container met volledige, persoonlijke toegang naar de weergave van de container eigen geïsoleerd van besturingssysteem constructies. Samen met draagbaarheid is deze mate van beveiliging en resource isolatie het belangrijkste voordeel voor het gebruik van containers met Service Fabric, die anders services in processen worden uitgevoerd.

Containers zijn een virtualisatietechnologie die het onderliggende besturingssysteem van toepassingen virtualiseert. Containers bieden een niet-wijzigbaar omgeving voor toepassingen worden uitgevoerd met verschillende mate van isolatie. Containers uitvoeren rechtstreeks boven op de kernel en hebben een geïsoleerd beeld van het bestandssysteem en andere bronnen. Vergeleken met de virtuele machines, hebben containers de volgende voordelen:

* **Kleine**: Containers gebruiken een enkele opslagruimte en layer-versies en updates voor de efficiëntie verhogen.
* **Snel**: Containers zijn om een volledige besturingssysteem worden opgestart zodat deze veel sneller, doorgaans in seconden opstarten kunnen.
* **Draagbaarheid**: de installatiekopie van een beperkte toepassing kan worden overgebracht om te worden uitgevoerd in de cloud, on-premises, binnen de virtuele machines of rechtstreeks op fysieke computers.
* **Resource governance**: een container kan de fysieke resources die deze kan worden gebruikt op de host beperken.

## <a name="container-types-and-supported-environments"></a>Containertypen en ondersteunde omgevingen
Service Fabric ondersteunt containers op Linux- en Windows, en ook isolatiemodus van Hyper-V op de laatste. 

> [!NOTE]
> Containers implementeren naar een Service Fabric-cluster op Windows 10 wordt momenteel niet ondersteund. 
> 

### <a name="docker-containers-on-linux"></a>Docker-containers op Linux
Docker biedt op hoog niveau API's maken en beheren van containers boven op Linux kernel containers. Docker-Hub is een centrale opslagplaats voor het opslaan en ophalen van installatiekopieën van de container.
Zie voor een zelfstudie [Docker-container implementeren op Service Fabric](service-fabric-get-started-containers-linux.md).

### <a name="windows-server-containers"></a>Windows Server-containers
Windows Server 2016 biedt twee verschillende soorten containers die van het niveau van de opgegeven isolatie afwijken. Windows Server-containers en Docker-containers zijn vergelijkbaar, omdat beide naamruimte en de bestandsnaam system isolatie hebben, maar de kernel delen met de host die ze worden uitgevoerd. Op Linux, deze isolatie oudsher is geleverd door `cgroups` en `namespaces`, en Windows Server-containers hetzelfde gedrag vertonen.

Windows-containers met Hyper-ondersteuning bieden meer isolatie en beveiligingsvereisten omdat elke container de kernel besturingssysteem niet met andere containers of met de host delen. Met deze hoger niveau van beveiliging, isolatie, worden ingeschakeld voor de Hyper-V-containers gericht op onveilige, multitenant-scenario's.
Zie voor een zelfstudie [implementeren van een Windows-container voor Service Fabric](service-fabric-get-started-containers.md).

De volgende afbeelding ziet de verschillende typen van netwerkvirtualisatie en -isolatie zijn beschikbaar in het besturingssysteem.
![Service Fabric-platform][Image1]

## <a name="scenarios-for-using-containers"></a>Scenario's voor het gebruik van containers
Hier volgen typische voorbeelden waar een container een goede keuze is:

* **IIS lift- en verschuiven**: als u bestaande hebt [ASP.NET MVC](https://www.asp.net/mvc) apps die u wilt blijven gebruiken, plaatst u deze in een container in plaats van het migreren van ze naar ASP.NET Core. Deze apps ASP.NET MVC afhankelijk zijn van op Internet Information Services (IIS). U kunt deze toepassingen in de container installatiekopieën vanaf de precreated IIS-installatiekopie van het pakket en implementeren met Service Fabric. Zie [Container afbeeldingen op Windows Server](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-server) voor meer informatie over Windows containers.
* **Meng containers en Service Fabric microservices**: een installatiekopie van een bestaande container gebruiken voor een deel van uw toepassing. U kunt bijvoorbeeld de [NGINX-container](https://hub.docker.com/_/nginx/) voor de webfront-end van uw toepassing en stateful services voor de intensievere back-end-berekeningen.
* **De gevolgen van de 'ruis neighbors' services**: U kunt de mogelijkheid van de resource governance van containers gebruiken voor het beperken van de resources die gebruikmaakt van een service op een host. Als services mogelijk veel resources in beslag nemen en van invloed zijn op de prestaties van andere gebruikers (zoals een langlopende, query-achtige-bewerking), kunt u deze services in de containers die resource governance stellen.

## <a name="service-fabric-support-for-containers"></a>Service Fabric-ondersteuning voor containers
Service Fabric ondersteunt de implementatie van Docker-containers voor Linux en Windows Server-containers op Windows Server 2016, evenals ondersteuning voor Hyper-V-isolatiemodus. 

In de Service Fabric [toepassingsmodel](service-fabric-application-model.md), een container vertegenwoordigt een toepassingshost welke service meerdere replica's worden geplaatst. Service Fabric geen containers kunt uitvoeren en het scenario is vergelijkbaar met de [Gast uitvoerbare scenario](service-fabric-deploy-existing-app.md), waar u een bestaande toepassing binnen een container van het pakket. Dit scenario is het algemene gebruiksvoorbeeld voor containers en uitvoeren van een toepassing die is geschreven met behulp van elke taal of frameworks, maar niet met behulp van de ingebouwde Service Fabric-programmeermodellen enkele voorbeelden.

Bovendien kunt u uitvoeren [Service Fabric-services in containers](service-fabric-services-inside-containers.md) ook. Ondersteuning voor actieve Service Fabric-services in containers is momenteel beperkt, en zal in toekomstige releases worden verbeterd.

Service Fabric heeft diverse container mogelijkheden waarmee u toepassingen maken die bestaan uit een van microservices die beperkte zijn. Service Fabric biedt de volgende mogelijkheden voor beperkte services:

* Implementatie van de container en activering.
* Resource governance inclusief het instellen van waarden van resource standaard op Azure-clusters.
* Verificatie van de opslagplaats.
* De poort van de container voor poorttoewijzing host.
* Detectie van de container-naar-container en communicatie.
* De mogelijkheid om te configureren en omgevingsvariabelen worden ingesteld.
* Mogelijkheid om beveiligingsreferenties instellen op de container.
* Een keuze uit verschillende VPN modi voor containers.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd over containers dat Service Fabric is een container orchestrator en dat het Service Fabric bevat functies die ondersteuning bieden voor containers. Een volgende stap gaan we over voorbeelden van elk van de functies die u kunt zien hoe u deze gebruikt.

[Uw eerste Service Fabric-container-toepassing maken in Windows](service-fabric-get-started-containers.md)

[Uw eerste Service Fabric-container-toepassing maken op Linux](service-fabric-get-started-containers-linux.md)

[Meer informatie over Windows-Containers](https://docs.microsoft.com/virtualization/windowscontainers/about/)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png
