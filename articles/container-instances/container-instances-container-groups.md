---
title: Azure-Container exemplaren Containergroepen
description: Begrijpen hoe Containergroepen werken in Azure Containerexemplaren
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 03/19/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 6f7f0d9aea86594140c302e6d12e6528e802b9e7
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="container-groups-in-azure-container-instances"></a>Containergroepen in Azure Containerexemplaren

De resource op het hoogste niveau in Azure Container gevallen is de *containergroep*. Dit artikel wordt beschreven wat containergroepen zijn en de typen van scenario's die ze inschakelen.

## <a name="how-a-container-group-works"></a>De werking van een containergroep

Een containergroep is een verzameling van containers die op dezelfde hostcomputer ophalen gepland. De containers in een containergroep delen levenscyclus, lokale netwerk en opslagvolumes. Het is vergelijkbaar met het concept van een *schil* in [Kubernetes] [ kubernetes-pod] en [DC/OS][dcos-pod].

Het volgende diagram toont een voorbeeld van een containergroep met meerdere containers:

![Diagram van container-groepen][container-groups-example]

In dit voorbeeld containergroep:

* Is gepland op een machine één host.
* Een label van DNS-naam toegewezen.
* Beschrijft de één openbaar IP-adres, met één blootgestelde poort.
* Bestaat uit twee containers. Een container luistert op poort 80, terwijl de andere luistert op poort 5000.
* Bevat twee Azure bestandsshares als volume koppelingen en elke container koppelt een van de shares lokaal.

> [!NOTE]
> Meerdere container groepen zijn momenteel beperkt tot Linux containers. Hoewel we ons best doen om alle functies beschikbaar te maken voor Windows-containers, kunnen de [quota en beschikbaarheid in regio´s voor Azure Container Instances](container-instances-quotas.md) variëren op de verschillende platforms.

### <a name="deployment"></a>Implementatie

**Containergroepen** een minimale brontoewijzing 1 vCPU en 1 GB geheugen hebben. Afzonderlijke **containers** kunnen worden ingericht met behulp van minder dan 1 vCPU en 1 GB geheugen. Binnen een containergroep kan de distributie van bronnen aan meerdere containers binnen de grenzen op het niveau van de groep container worden aangepast. Bijvoorbeeld twee containers met 0,5 vCPU die zich binnen een containergroep 1 vCPU toegewezen.

### <a name="networking"></a>Netwerken

Containergroepen delen een IP-adres en een poort-naamruimte op dat IP-adres. Voor het inschakelen van externe clients een container in de groep bereikt, moet u de poort van het IP-adres en de container beschikbaar. Aangezien containers in de groep een naamruimte poort delen, worden poorttoewijzing wordt niet ondersteund. Containers in een groep kunnen bereiken elkaar via localhost op de poorten die ze beschikbaar zijn gesteld, zelfs als deze poorten niet extern worden weergegeven op de groep IP-adres.

### <a name="storage"></a>Storage

U kunt externe volumes te koppelen in een container wilt opgeven. U kunt deze volumes toewijzen aan specifieke paden binnen de afzonderlijke containers in een groep.

## <a name="common-scenarios"></a>Algemene scenario's

Meerdere container groepen zijn handig in gevallen waar u een enkele taak voor het functionele onderverdelen in een klein aantal installatiekopieën van de container. Deze installatiekopieën vervolgens kunnen worden geleverd door verschillende teams en afzonderlijke resourcevereisten hebben.

Voorbeeld van syntaxis kan omvatten:

* Een toepassingscontainer en een container voor logboekregistratie. De container logboekregistratie verzamelt uitvoer van de logboeken en metrische gegevens van de hoofdtoepassing en schrijft deze naar langetermijnopslag.
* Een toepassing en een controle container. De bewaking container doet een aanvraag periodiek naar de toepassing om ervoor te zorgen dat het actief is en correct reageert en wordt een waarschuwing gegeven als dat niet.
* Een container voor een webtoepassing en een container binnenhalen van de meest recente inhoud vanuit resourcebeheer.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [implementeren van een groep met meerdere container](container-instances-multi-container-group.md) met een Azure Resource Manager-sjabloon.

<!-- IMAGES -->
[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png

<!-- LINKS - External -->
[dcos-pod]: https://dcos.io/docs/1.10/deploying-services/pods/
[kubernetes-pod]: https://kubernetes.io/docs/concepts/workloads/pods/pod/
