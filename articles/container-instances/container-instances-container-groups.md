---
title: Azure-Container exemplaren Containergroepen
description: Begrijpen hoe Containergroepen werken in Azure Containerexemplaren
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 25eab41c3f0c986bcce33123f86f4c9638b77191
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="container-groups-in-azure-container-instances"></a>Containergroepen in Azure Containerexemplaren

De resource op het hoogste niveau in Azure Containerexemplaren is een containergroep. Dit artikel wordt beschreven wat containergroepen zijn en wat voor soort scenario's die ze inschakelen.

## <a name="how-a-container-group-works"></a>De werking van een containergroep

Een containergroep is een verzameling van containers die ophalen gepland op dezelfde hostcomputer en dezelfde levenscyclus, het lokale netwerk en opslagvolumes. Het is vergelijkbaar met het concept van een *schil* in [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) en [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).

Het volgende diagram toont een voorbeeld van een containergroep met meerdere containers.

![Voorbeeld van de container-groepen][container-groups-example]

Opmerking:

- De groep is gepland op een machine één host.
- De groep wordt één openbaar IP-adres, met één blootgestelde poort.
- De groep bestaat uit twee containers. Een container luistert op poort 80, terwijl de andere luistert op poort 5000.
- De groep bevat twee Azure-bestandsshares als volume koppelingen en elke container koppelt een van de shares lokaal.

### <a name="networking"></a>Netwerken

Containergroepen delen een IP-adres en een poort-naamruimte op dat IP-adres. Voor het inschakelen van externe clients een container in de groep bereikt, moet u de poort van het IP-adres en de container beschikbaar. Aangezien containers in de groep een naamruimte poort delen, worden poorttoewijzing wordt niet ondersteund. Containers in een groep kunnen bereiken elkaar via localhost op de poorten die ze beschikbaar zijn gesteld, zelfs als deze poorten niet extern worden weergegeven op de groep IP-adres.

### <a name="storage"></a>Storage

U kunt externe volumes te koppelen in een container wilt opgeven. U kunt deze volumes toewijzen aan specifieke paden binnen de afzonderlijke containers in een groep.

## <a name="common-scenarios"></a>Algemene scenario's

Meerdere container groepen zijn handig in gevallen waarin u wilt delen van een enkele functionele taak in een klein aantal installatiekopieën van de container, die kunnen worden geleverd door verschillende teams en afzonderlijke resourcevereisten hebben. Voorbeeld van syntaxis kan omvatten:

- Een toepassingscontainer en een container voor logboekregistratie. De container logboekregistratie verzamelt uitvoer van de logboeken en metrische gegevens van de hoofdtoepassing en schrijft deze naar langetermijnopslag.
- Een toepassing en een controle container. De bewaking container doet een aanvraag periodiek naar de toepassing om ervoor te zorgen dat het actief is en correct reageert en wordt een waarschuwing gegeven als dat niet.
- Een container voor een webtoepassing en een container binnenhalen van de meest recente inhoud vanuit resourcebeheer.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [implementeren van een groep met meerdere container](container-instances-multi-container-group.md) met een Azure Resource Manager-sjabloon.

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png