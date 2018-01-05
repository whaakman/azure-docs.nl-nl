---
title: Azure-Container exemplaren Containergroepen
description: Begrijpen hoe Containergroepen werken in Azure Containerexemplaren
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 12/19/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: a42c01917926a4297c97cf9c5dfd1333dbef6793
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="container-groups-in-azure-container-instances"></a>Containergroepen in Azure Containerexemplaren

De resource op het hoogste niveau in Azure Container gevallen is de *containergroep*. Dit artikel wordt beschreven wat containergroepen zijn en de typen van scenario's die ze inschakelen.

## <a name="how-a-container-group-works"></a>De werking van een containergroep

Een containergroep is een verzameling van containers die op dezelfde hostcomputer ophalen gepland. De containers in een containergroep delen levenscyclus, lokale netwerk en opslagvolumes. Het is vergelijkbaar met het concept van een *schil* in [Kubernetes] [ kubernetes-pod] en [DC/OS][dcos-pod].

Het volgende diagram toont een voorbeeld van een containergroep met meerdere containers.

![Diagram van container-groepen][container-groups-example]

In dit voorbeeld containergroep:

* Is gepland op een machine één host.
* Beschrijft de één openbaar IP-adres, met één blootgestelde poort.
* Bestaat uit twee containers. Een container luistert op poort 80, terwijl de andere luistert op poort 5000.
* Bevat twee Azure bestandsshares als volume koppelingen en elke container koppelt een van de shares lokaal.

> [!NOTE]
> Meerdere container groepen zijn momenteel beperkt tot Linux containers. Terwijl we proberen te zorgen dat alle functies op Windows-containers, vindt u de huidige platform verschillen in [quota en beschikbaarheid in regio's voor exemplaren van Azure-Container](container-instances-quotas.md).

### <a name="networking"></a>Netwerken

Containergroepen delen een IP-adres en een poort-naamruimte op dat IP-adres. Voor het inschakelen van externe clients een container in de groep bereikt, moet u de poort van het IP-adres en de container beschikbaar. Aangezien containers in de groep een naamruimte poort delen, worden poorttoewijzing wordt niet ondersteund. Containers in een groep kunnen bereiken elkaar via localhost op de poorten die ze beschikbaar zijn gesteld, zelfs als deze poorten niet extern worden weergegeven op de groep IP-adres.

### <a name="storage"></a>Storage

U kunt externe volumes te koppelen in een container wilt opgeven. U kunt deze volumes toewijzen aan specifieke paden binnen de afzonderlijke containers in een groep.

## <a name="common-scenarios"></a>Algemene scenario's

Meerdere container groepen zijn handig in gevallen waarin u wilt delen van een enkele functionele taak in een klein aantal installatiekopieën van de container, die kunnen worden geleverd door verschillende teams en afzonderlijke resourcevereisten hebben.

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
