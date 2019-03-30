---
title: Overzicht van Service Fabric en containers | Microsoft Docs
description: Een overzicht van Service Fabric en het gebruik van containers microservicetoepassingen te implementeren. Dit artikel bevat een overzicht van hoe containers kunnen worden gebruikt en de beschikbare mogelijkheden in Service Fabric.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: c98b3fcb-c992-4dd9-b67d-2598a9bf8aab
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/8/2018
ms.author: aljo
ms.openlocfilehash: 5a45f14e5ac1da5152f320bd92b1ebb42be1d214
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662738"
---
# <a name="service-fabric-and-containers"></a>Service Fabric en containers

## <a name="introduction"></a>Inleiding

Azure Service Fabric is een platform voor gedistribueerde systemen waarmee u gemakkelijk schaalbare en betrouwbare microservices en containers verpakt, implementeert en beheert.

Service Fabric is van Microsoft [containerorchestrator](service-fabric-cluster-resource-manager-introduction.md) voor het implementeren van microservices in een computercluster. Service Fabric-voordelen van de ervaringen opgedaan gedurende het jaar met services van Microsoft op grote schaal.

Microservices kunnen op tal van manieren worden ontwikkeld met behulp van de [Service Fabric-programmeermodellen](service-fabric-choose-framework.md), [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md), voor het implementeren van [code naar keuze](service-fabric-guest-executables-introduction.md). Of, als u alleen wilt [implementeren en beheren van containers](service-fabric-containers-overview.md), Service Fabric is ook een uitstekende keuze.

Standaard wordt Service Fabric implementeert en Hiermee activeert u deze services als processen. Processen bieden de snelste activering en de hoogste dichtheid gebruik van de resources in een cluster. Service Fabric kunt u ook services in containerinstallatiekopieën implementeren. U kunt ook de services in processen en services in containers, in dezelfde toepassing combineren.

Als u wilt meedoen en uitproberen van containers in Service Fabric, kunt u proberen een snelstart, zelfstudie of voorbeeld:  

[Snelstart: Een Linux-containertoepassing in Service Fabric implementeren](service-fabric-quickstart-containers-linux.md)  
[Snelstart: Een Windows-containertoepassing in Service Fabric implementeren](service-fabric-quickstart-containers.md)  
[Een bestaande .NET-app in een container plaatsen](service-fabric-host-app-in-a-container.md)  
[Voorbeelden van Service Fabric-containers](https://azure.microsoft.com/resources/samples/service-fabric-containers/)  

## <a name="what-are-containers"></a>Wat zijn containers

Containers oplossen het probleem van actieve toepassingen betrouwbaar in verschillende rekenomgevingen door te geven van een onveranderbare omgeving voor de toepassing om in te voeren. Containers verpakken een toepassing en alle bijbehorende afhankelijkheden, zoals bibliotheken en configuratiebestanden, in een eigen geïsoleerde 'vak' die alles bevat wat nodig is om uit te voeren van de software in de container. Waar de container wordt uitgevoerd, heeft de toepassing erin altijd alles wat die zij nodig heeft om uit te voeren, zoals de juiste versies van de afhankelijke bibliotheken, alle configuratiebestanden en iets anders nodig om uit te voeren.

Containers uitvoeren rechtstreeks boven op de kernel en beschikt over een geïsoleerd beeld van het bestandssysteem en andere bronnen. Een toepassing in een container is geen kennis van andere toepassingen of processen buiten de container. Elke toepassing en de bibliotheken runtime, afhankelijkheden en het systeem worden uitgevoerd binnen een container met volledige, persoonlijke toegang naar de weergave van de container eigen geïsoleerd van het besturingssysteem. Maakt het gemakkelijk voor alle afhankelijkheden van van uw toepassing moet worden uitgevoerd in verschillende rekenomgevingen, beveiliging en isolatie van resources zijn belangrijke voordelen van het gebruik van containers met Service Fabric - services-waaronder anders wordt uitgevoerd in een het proces.

Vergeleken met de virtuele machines, hebben containers de volgende voordelen:

* **Kleine**: Containers gebruiken een één opslagruimte en layer-versies en updates op de efficiëntie wordt verhoogd.
* **Snel**: Containers hebt geen volledig besturingssysteem, het opstarten, zodat ze kunnen veel sneller--doorgaans in een paar seconden.
* **Draagbaarheid**: De installatiekopie van een containertoepassing kunt om uit te voeren in de cloud, on-premises, binnen virtuele machines of rechtstreeks op fysieke computers worden overgezet.
* **Resourcebeheer**: Een container kunt beperken van de fysieke resources die deze op de host gebruiken kan.

### <a name="container-types-and-supported-environments"></a>Containertypen en ondersteunde omgevingen

Service Fabric biedt ondersteuning voor containers op zowel Windows als Linux, en biedt ondersteuning voor Hyper-V-isolatiemodus op Windows.

#### <a name="docker-containers-on-linux"></a>Docker-containers in Linux

Docker biedt API's voor containers op Linux-kernel-containers maken en beheren. Docker-Hub biedt een centrale opslagplaats voor het opslaan en ophalen van containerinstallatiekopieën.
Zie voor een zelfstudie op basis van Linux [uw eerste Service Fabric-containertoepassing maken in Linux](service-fabric-get-started-containers-linux.md).

#### <a name="windows-server-containers"></a>Windows Server-containers

Windows Server 2016 biedt twee verschillende soorten containers die op het niveau van isolatie afwijken. Windows Server-containers en Docker-containers zijn vergelijkbaar, omdat zowel de naamruimte en de bestandsnaam system isolatie, terwijl de kernel deelt met de host waarop ze worden uitgevoerd. Deze isolatie is traditioneel opgegeven door cgroups en naamruimten op Linux en Windows Server-containers op dezelfde manier werken.

Windows-containers met ondersteuning voor Hyper-V bieden meer isolatie en beveiliging, omdat er geen container de kernel van het besturingssysteem met een andere container of de host deelt. Ingeschakeld Hyper-V-containers zijn met dit hogere niveau van, beveiligingsisolatie gericht op scenario's mogelijk onveilig, meerdere tenants.
Zie voor een zelfstudie op basis van Windows [uw eerste Service Fabric-containertoepassing maken in Windows](service-fabric-get-started-containers.md).

De volgende afbeelding ziet u de verschillende typen virtualisatie en isolatie niveaus beschikbaar.
![Service Fabric-platform][Image1]

## <a name="scenarios-for-using-containers"></a>Scenario's voor het gebruik van containers

Hier volgen typische voorbeelden waarin een container een goede keuze is:

* **IIS lift- and -shift**: U kunt ook een bestaande plaatsen [ASP.NET MVC](https://www.asp.net/mvc) -app in een container in plaats van het migreren van het ASP.NET Core. Deze ASP.NET MVC-apps zijn afhankelijk van op Internet Information Services (IIS). U kunt deze toepassingen in containerinstallatiekopieën van de precreated IIS-installatiekopie Inpakken en te implementeren met Service Fabric. Zie [Containerinstallatiekopieën op Windows Server](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-server) voor informatie over Windows-containers.

* **Containers en microservices voor Service Fabric**: Gebruik een bestaande containerinstallatiekopie voor een deel van uw toepassing. Bijvoorbeeld, kunt u de [NGINX-container](https://hub.docker.com/_/nginx/) voor de webfront-end van uw toepassing en de stateful services voor de berekening van de meer intensieve back-end.

* **De gevolgen van 'luidruchtige buren' services**: U kunt de mogelijkheid van de resource governance van containers gebruiken om te beperken van de resources die gebruikmaakt van een service op een host. Als services mogelijk veel resources verbruiken en invloed op de prestaties van anderen (zoals een langdurige, query-achtige-bewerking), kunt u deze services in containers die resourcebeheer brengen.

## <a name="service-fabric-support-for-containers"></a>Service Fabric-ondersteuning voor containers

Service Fabric biedt ondersteuning voor de implementatie van Docker-containers in Linux en Windows Server-containers op Windows Server 2016, samen met de ondersteuning voor Hyper-V-isolatiemodus. 

Service Fabric biedt een [toepassingsmodel](service-fabric-application-model.md) in die een container vertegenwoordigt een toepassingshost in welke meerdere service replica's worden geplaatst. Service Fabric ondersteunt ook een [Gast uitvoerbare scenario](service-fabric-guest-executables-introduction.md) in die u niet de ingebouwde Service Fabric-programmeermodellen gebruiken maar in plaats daarvan een bestaande toepassing, die zijn geschreven met elke taal of framework, in het pakket een de container. Dit scenario is de algemene use case voor containers.

U kunt ook uitvoeren [Service Fabric-services binnen een container](service-fabric-services-inside-containers.md). Ondersteuning voor actieve Service Fabric-services in containers is momenteel beperkt.

Service Fabric biedt verschillende containermogelijkheden voor waarmee u toepassingen bouwen die bestaan uit microservices voorzien van containers, zoals:

* Implementatie van de container-installatiekopie en activeren.
* Resourcebeheer inclusief het instellen van de bronwaarden standaard op Azure-clusters.
* Verificatie van de opslagplaats.
* De poort van de container naar host poorttoewijzing.
* Container-naar-container-detectie en communicatie.
* Mogelijkheid om te configureren en instellen van omgevingsvariabelen.
* De mogelijkheid om in te stellen van beveiligingsreferenties op de container.
* Een keuze uit verschillende netwerkmodi voor containers.

Ondersteuning voor een uitgebreid overzicht van de container in Azure, zoals het maken van een Kubernetes-cluster met Azure Kubernetes Service, hoe voor het maken van een privé dockerregister in Azure Container Registry, en meer, Zie [Azure voor Containers](https://docs.microsoft.com/azure/containers/).

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de ondersteuning voor die service Fabric biedt voor het uitvoeren van containers. Vervolgens gaan we over voorbeelden van elk van de functies leert u hoe u deze kunt gebruiken.

[Uw eerste Service Fabric-containertoepassing maken in Linux](service-fabric-get-started-containers-linux.md)  
[Uw eerste Service Fabric-containertoepassing maken in Windows](service-fabric-get-started-containers.md)  
[Meer informatie over Windows-Containers](https://docs.microsoft.com/virtualization/windowscontainers/about/)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png