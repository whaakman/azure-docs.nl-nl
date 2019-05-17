---
title: Azure Container Instances - Veelgestelde vragen
description: Antwoorden op veelgestelde vragen met betrekking tot de service Azure Container Instances
services: container-instances
author: dkkapur
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 4/25/2019
ms.author: dekapur
ms.openlocfilehash: 99882bd23d7b94afc550247172e5b70deb23bec9
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65791400"
---
# <a name="frequently-asked-questions-about-azure-container-instances"></a>Veelgestelde vragen over Azure Container Instances

In dit artikel komen de antwoorden op veelgestelde vragen over Azure Container Instances.

## <a name="deployment"></a>Implementatie

### <a name="how-large-can-my-container-image-be"></a>Hoe groot mag de containerinstallatiekopie zijn?

De maximale grootte voor de containerinstallatiekopie van een kan worden geïmplementeerd op Azure Container Instances is 15 GB. Kunt u mogelijk meer installatiekopieën implementeren, afhankelijk van de exacte beschikbaarheid op het moment dat u hebt geïmplementeerd, maar dit kan niet worden gegarandeerd.

De grootte van uw containerinstallatiekopie heeft invloed op hoe lang het duurt om te implementeren, dus algemeen dat u wilt uw containerinstallatiekopieën zo klein mogelijk te houden.

### <a name="how-can-i-speed-up-the-deployment-of-my-container"></a>Hoe kan ik de implementatie van mijn container versnellen?

Omdat een van de belangrijkste factoren van implementatietijden grootte van de installatiekopie, zoek naar manieren om de grootte te beperken. Verwijder de lagen die u niet nodig hebben, en Reduceer de grootte van lagen in de installatiekopie (met een lichtere OS-basisinstallatiekopie ophalen). Bijvoorbeeld, als u Linux-containers uitvoert, kunt u overwegen Alpine als uw basisinstallatiekopie in plaats van een volledige Ubuntu-Server. Op dezelfde manier voor Windows-containers, gebruikt u indien mogelijk een basisinstallatiekopie Nano Server in. 

Controleer ook de lijst met vooraf in cache opgeslagen afbeeldingen in Azure Container afbeeldingen, beschikbaar via de [lijst in de cache opgeslagen afbeeldingen](/rest/api/container-instances/listcachedimages) API. Kunt u mogelijk om over te schakelen van een installatiekopie-laag voor een van de vooraf in cache opgeslagen afbeeldingen. 

Zie voor meer informatie [gedetailleerde richtlijnen](container-instances-troubleshooting.md#container-takes-a-long-time-to-start) op het verkorten van de opstarttijd van de container.

### <a name="what-windows-base-os-images-are-supported"></a>Welke Windows-basisinstallatiekopieën besturingssysteem worden ondersteund?

#### <a name="windows-server-2016-base-images"></a>Basisinstallatiekopieën voor Windows Server 2016

* [Nano Server](https://hub.docker.com/_/microsoft-windows-nanoserver): `10.0.14393.x`, `sac2016`
* [Windows Server Core](https://hub.docker.com/_/microsoft-windows-servercore): `ltsc2016`,  `10.0.14393.x`

> [!NOTE]
> Windows-installatiekopieën op basis van het semi-Annual-kanaal versie 1709 of 1803 worden niet ondersteund.

#### <a name="windows-server-2019-and-client-base-images-preview"></a>Client- en Windows Server 2019 basisinstallatiekopieën (preview)

* [Nano Server](https://hub.docker.com/_/microsoft-windows-nanoserver): `1809`, `10.0.17763.x`
* [Windows Server Core](https://hub.docker.com/_/microsoft-windows-servercore): `ltsc2019`, `1809`, `10.0.17763.x`
* [Windows](https://hub.docker.com/_/microsoft-windows): `1809`, `10.0.17763.x` 

### <a name="what-net-or-net-core-image-layer-should-i-use-in-my-container"></a>Welke laag .NET of .NET Core-installatiekopie moet ik gebruiken in mijn container? 

De kleinste installatiekopie gebruikt die aan uw vereisten voldoet. Voor Linux, kunt u een *runtime-alpine* .NET Core-installatiekopie, die werd ondersteund sinds de release van .NET Core 2.1. Voor Windows, als u het volledige .NET Framework, moet u het gebruik van een Windows Server Core-installatiekopie (alleen-runtime-installatiekopie, zoals *4.7.2-windowsservercore-ltsc2016*). Alleen-runtime-installatiekopieën zijn kleiner, maar bieden geen ondersteuning voor workloads waarvoor de .NET SDK.

## <a name="availability-and-quotas"></a>Beschikbaarheid en quota

### <a name="how-many-cores-and-memory-should-i-allocate-for-my-containers-or-the-container-group"></a>Het aantal kernen en het geheugen moet ik voor mijn containers of de containergroep toewijzen?

Dit is echt afhankelijk van uw workload. Begin klein en test de prestaties om te zien hoe uw containers doen. [CPU en geheugen Resourcegebruik te bewaken](container-instances-monitor.md), en voeg deze kernen of geheugen op basis van het type van de processen die u implementeert in de container. 

Controleer ook of de [resourcebeschikbaarheid](container-instances-region-availability.md#availability---general) voor de regio die u voor de bovengrens voor CPU-kernen en geheugen die beschikbaar is per containergroep van de implementeert. 

### <a name="what-underlying-infrastructure-does-aci-run-on"></a>Welke onderliggende infrastructuur ACI uitgevoerd?

Azure Container Instances is erop gericht om een serverloze containers-on-demand-service, dus we willen dat u om te worden gericht op de ontwikkeling van uw containers en u geen over de infrastructuur zorgen. Die u meer wilt weten of wanting vergelijkingen op de prestaties, doen ACI wordt uitgevoerd op Azure virtuele machines van verschillende SKU's, sets voornamelijk uit de F en de D-serie. We verwachten dat dit wijzigen in de toekomst als we blijven ontwikkelen en optimaliseren van de service. 

### <a name="i-want-to-deploy-thousand-of-cores-on-aci---can-i-get-my-quota-increased"></a>Ik wil implementeren duizenden kernen op ACI - vind ik mijn quotamaximum te verhogen?
 
Ja (soms). Zie de [quota en limieten](container-instances-quotas.md) artikel voor de huidige quota's en welke limieten kunnen worden verhoogd door de aanvraag.

### <a name="can-i-deploy-with-more-than-4-cores-and-16-gb-of-ram"></a>Kan ik implementeren met meer dan 4 kernen en 16 GB aan RAM-geheugen?

Nog niet. Dit zijn op dit moment de maximumwaarden voor een containergroep. Neem contact op met ondersteuning van Azure met specifieke vereisten of aanvragen. 

### <a name="when-will-aci-be-in-a-specific-region"></a>Wanneer worden ACI in een bepaalde regio?

Huidige regionale beschikbaarheid wordt gepubliceerd [hier](container-instances-region-availability.md#availability---general). Als u een vereiste voor een specifieke regio hebt, neem dan contact op met ondersteuning voor Azure.

## <a name="features-and-scenarios"></a>Functies en scenario 's

### <a name="how-do-i-scale-a-container-group"></a>Hoe schaal ik een containergroep?

Schalen is momenteel niet beschikbaar voor containers of groepen met containers. Als u meer instanties worden uitgevoerd wilt, moet u de API gebruiken om automatiseren en meer aanvragen voor container groep maken met de service te maken. 

### <a name="what-features-are-available-to-instances-running-in-a-custom-vnet"></a>Welke functies zijn beschikbaar voor exemplaren die worden uitgevoerd in een aangepaste VNet?

U kunt containergroepen in een Azure-netwerk van uw keuze implementeren en privé-IP's aan de containergroepen voor het routeren van verkeer binnen het VNet via uw Azure-resources overdragen. Implementatie van de containergroep van een in een virtueel netwerk is momenteel in preview en sommige aspecten van deze functie kunnen wijzigen voordat de algemene beschikbaarheid (GA). Zie [beperkingen bekijken](container-instances-vnet.md#preview-limitations) voor actuele informatie.

## <a name="pricing"></a>Prijs

### <a name="when-does-the-meter-start-running"></a>Wanneer begint de meter uitgevoerd?

Duur van de containergroep wordt berekend vanaf het moment waarop we starten voor het ophalen van uw eerste container-installatiekopie (voor een nieuwe implementatie) of uw containergroep opnieuw wordt opgestart (indien al geïmplementeerd), totdat de containergroep is gestopt. Zie de details op [Container Instances prijzen](https://azure.microsoft.com/pricing/details/container-instances/).

### <a name="do-i-stop-being-charged-when-my-containers-are-stopped"></a>Stop ik in rekening gebracht als mijn containers worden gestopt?

Meters werkt niet meer nadat de volledige container-groep is gestopt. Als een container in de containergroep wordt uitgevoerd, die we de resources in het geval u wilt de containers opnieuw opgestart. 

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie](container-instances-overview.md) over Azure Container Instances.
* [Algemene problemen oplossen](container-instances-troubleshooting.md) in Azure Container Instances.