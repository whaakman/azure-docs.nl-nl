---
title: Groepen van Azure Container Instances-container
description: Inzicht in hoe meerdere containergroepen werken in Azure Container Instances
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/20/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 8724bd7e13b0d8607ad5a6814b27c8c06681f331
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58202007"
---
# <a name="container-groups-in-azure-container-instances"></a>Containergroepen in Azure Container Instances

De resource op het hoogste niveau in Azure Container Instances is de *containergroep*. Dit artikel wordt beschreven wat containergroepen zijn en de typen van scenario's die ze inschakelen.

## <a name="how-a-container-group-works"></a>De werking van een containergroep

Een containergroep is een verzameling van containers die u gepland op dezelfde hostcomputer. De containers in een containergroep delen een levenscyclus, resources, lokale netwerk en opslagvolumes. Qua concept dat is een *pod* in [Kubernetes][kubernetes-pod].

Het volgende diagram toont een voorbeeld van een containergroep met meerdere containers:

![Diagram van container-groepen][container-groups-example]

In dit voorbeeld van de container-groep:

* Is gepland op één enkele hostcomputer.
* Een DNS-naamlabel toegewezen.
* Wordt aangegeven dat een enkel openbaar IP-adres, met één beschikbaar gestelde poort.
* Bestaat uit twee containers. Een container luistert op poort 80, terwijl de andere luistert op poort 5000.
* Bevat twee Azure-bestandsshares als volume koppelingen en elke container koppelt een van de shares lokaal.

> [!NOTE]
> Groepen met meerdere containers ondersteunen momenteel alleen Linux-containers. Voor Windows-containers ondersteunt Azure Container Instances alleen de implementatie van één exemplaar. Terwijl we werken om alle functies op Windows-containers, vindt u de huidige platform verschillen in de service [overzicht](container-instances-overview.md#linux-and-windows-containers).

## <a name="deployment"></a>Implementatie

Hier volgen twee algemene manieren om een groep met meerdere containers implementeren: gebruik een [Resource Manager-sjabloon] [ resource-manager template] of een [YAML-bestand][yaml-file]. Resource Manager-sjabloon gebruiken wanneer u nodig hebt om extra Azure-service-resources te implementeren (bijvoorbeeld een [Azure Files delen][azure-files]) op het moment u de containerinstanties implementeren. Vanwege de YAML-indeling meer beknopte aard, worden een YAML-bestand wordt aanbevolen wanneer uw implementatie alleen containerinstanties bevat.

## <a name="resource-allocation"></a>Toewijzing van resources

Azure Container Instances toegewezen resources, zoals CPU, geheugen, en eventueel [GPU's] [ gpus] (preview) en een containergroep door toe te voegen de [resourceaanvragen] [ resource-requests] van de exemplaren in de groep. CPU-resources nemen een voorbeeld: als u een containergroep met twee instanties, elke aanvragende 1 maken CPU en vervolgens de containergroep wordt toegewezen 2 CPU's.

De maximale beschikbare resources voor een containergroep afhankelijk zijn van de [Azure-regio] [ region-availability] gebruikt voor de implementatie.

### <a name="container-resource-requests-and-limits"></a>Container resourceaanvragen en -limieten

* Standaard, containerexemplaren in een groep delen de aangevraagde resources van de groep. In een groep met twee exemplaren met elke aanvragende 1 CPU, de groep als geheel heeft toegang tot 2 CPU's. Elk exemplaar u kunt de 2 CPU's kunt gebruiken en de exemplaren kunnen concurreren om CPU-bronnen, terwijl ze worden uitgevoerd.

* Als u wilt beperken Resourcegebruik door een instantie in een groep, eventueel instellen een [resourcelimiet] [ resource-limits] voor het exemplaar. Aanvragen in een groep met twee exemplaren van 1 CPU, een van uw containers mogelijk meer CPU's vereist zijn om uit te voeren dan de andere.

  In dit scenario kan stelt u een resourcelimiet van 0,5 CPU voor één exemplaar en een limiet van 2 CPU's voor de tweede. Deze configuratie beperkt gebruik van de eerste container op 0,5 CPU, zodat de tweede container die u kunt de volledige 2 CPU's gebruiken indien beschikbaar.

Zie voor meer informatie de [ResourceRequirements] [ resource-requirements] eigenschap in de container groepen REST-API.

### <a name="minimum-and-maximum-allocation"></a>Minimale en maximale toewijzing

* Toewijzen van een **minimale** van 1 CPU- en 1 GB geheugen aan een containergroep. Afzonderlijke containerinstanties binnen een groep kunnen worden ingericht met minder dan 1 CPU- en 1 GB geheugen. 

* Voor de **maximale** resources in een containergroep, Zie de [resourcebeschikbaarheid] [aci--beschikbaarheid in regio] voor Azure Container Instances in de implementatieregio.

## <a name="networking"></a>Netwerken

Containergroepen delen een IP-adres en een poort-naamruimte op dat IP-adres. Als u externe clients tot een container in de groep, moet u de poort van het IP-adres en de container beschikbaar maken. Aangezien containers in de groep een poort-naamruimte delen, wordt niet poorttoewijzing ondersteund. Containers in een groep kunnen bereiken elkaar via ' localhost ' op de poorten die ze beschikbaar zijn gesteld, zelfs als deze poorten extern worden niet op het IP-adres van de groep weergegeven.

(Optioneel) implementeren containergroepen in een [virtueel Azure-netwerk] [ virtual-network] (preview) om toe te staan van containers om veilig te communiceren met andere resources in het virtuele netwerk.

## <a name="storage"></a>Opslag

U kunt externe volumes te koppelen binnen een containergroep opgeven. U kunt deze volumes in specifieke paden binnen de afzonderlijke containers in een groep toewijzen.

## <a name="common-scenarios"></a>Algemene scenario's

Groepen met meerdere containers zijn handig in gevallen waar u wilt een enkele functionele taak onderverdelen in een klein aantal containerinstallatiekopieën. Deze installatiekopieën vervolgens kunnen worden geleverd door verschillende teams en afzonderlijke resourcevereisten hebben.

Voorbeeld kan zijn:

* Een container voor een webtoepassing en een container voor het binnenhalen van de meest recente inhoud vanuit broncodebeheer.
* Een App-container en een container voor logboekregistratie. De container logboekregistratie de hoofdtoepassing de uitvoer van de logboeken en metrische gegevens verzamelt en schrijft deze naar langetermijnopslag.
* Een App-container en een controle-container. De container monitoring zendt een verzoek periodiek naar de toepassing om ervoor te zorgen dat deze actief is en goed reageert, en een waarschuwing genereert als deze niet.
* Een front-end-container en een back-end-container. De front-end kan een webtoepassing, dienen met de back-end van een service om gegevens te halen. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het implementeren van een multi-containertoepassingen-containergroep met een Azure Resource Manager-sjabloon:

> [!div class="nextstepaction"]
> [De containergroep van een implementeren][resource-manager template]

<!-- IMAGES -->
[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png

<!-- LINKS - External -->
[dcos-pod]: https://dcos.io/docs/1.10/deploying-services/pods/
[kubernetes-pod]: https://kubernetes.io/docs/concepts/workloads/pods/pod/

<!-- LINKS - Internal -->
[resource-manager template]: container-instances-multi-container-group.md
[yaml-file]: container-instances-multi-container-yaml.md
[region-availability]: container-instances-region-availability.md
[resource-requests]: /rest/api/container-instances/containergroups/createorupdate#resourcerequests
[resource-limits]: /rest/api/container-instances/containergroups/createorupdate#resourcelimits
[resource-requirements]: /rest/api/container-instances/containergroups/createorupdate#resourcerequirements
[azure-files]: container-instances-volume-azure-files.md
[virtual-network]: container-instances-vnet.md
[gpus]: container-instances-gpu.md
