---
title: OpenShift in overzicht van Azure | Microsoft Docs
description: Een overzicht van OpenShift in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: d68215359d50ac153d6df3bbcce5a9b6171698bb
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085436"
---
# <a name="openshift-in-azure"></a>OpenShift in Azure

OpenShift is een container open en uitbreidbaar platform voor toepassingen die zorgt voor Docker en Kubernetes in de onderneming.  

OpenShift bevat Kubernetes voor containerindeling en beheer. Wordt toegevoegd speciaal voor ontwikkelaars en operations-gebaseerde hulpprogramma's die inschakelen:

- Snel ontwikkelen van toepassingen.
- Eenvoudige implementatie en schalen.
- Op de lange termijn onderhoud van de levenscyclus voor teams en toepassingen.

Er zijn meerdere versies van OpenShift beschikbaar:

- OpenShift Container Platform
- OpenShift op Azure (volledig beheerde OpenShift in vroege CY2019 binnenkort)
- OKD (voorheen OpenShift Origin)
- OpenShift Dedicated
- OpenShift Online

Van de vijf versies in dit artikel besproken, alleen twee nu beschikbaar zijn voor klanten om te implementeren in Azure: OpenShift Container Platform en OKD.

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Containerplatform is een bedrijfsklare [commerciële versie](https://www.openshift.com) uit en wordt ondersteund door Red Hat. Met deze versie kunnen klanten de benodigde rechten voor OpenShift Container Platform aanschaffen en verantwoordelijk zijn voor installatie en beheer van de gehele infrastructuur.

Omdat klanten 'eigenaar' van het platform, kunnen ze deze installeren in hun on-premises datacenter of in een openbare cloud (zoals Azure, AWS en Google).

## <a name="openshift-on-azure"></a>OpenShift op Azure

OpenShift op Azure is een volledig beheerde van OpenShift uitvoeren in Azure. Deze service is gezamenlijk beheerd en ondersteund door Microsoft en Red Hat. Het cluster implementeert in Azure-abonnement van de klant. De service is momenteel in Private Preview en is gepland voor algemene beschikbaarheid in vroege CY 2019. Meer informatie zal worden opgegeven omdat de aanbieding wordt verkregen dichter bij algemene beschikbaarheid.

## <a name="okd-formerly-openshift-origin"></a>OKD (voorheen OpenShift Origin)

OKD is een [open-source](https://www.okd.io/) upstream-project van OpenShift die community ondersteund. OKD kan worden geïnstalleerd op een CentOS of Red Hat Enterprise Linux (RHEL).

## <a name="openshift-dedicated"></a>OpenShift Dedicated

Toegewezen is een Red Hat-beheerde *één tenant* OpenShift die gebruikmaakt van OpenShift Container Platform. Red Hat beheert alle van de onderliggende infrastructuur (VM's, OpenShift-cluster, netwerken, opslag, enzovoort.). Het cluster is specifiek voor één klant en wordt uitgevoerd in een openbare cloud (zoals AWS of Google). Een eerste cluster bevat vier knooppunten van de toepassingen en alle kosten worden jaarlijks en betaalde kosten vooraf.

## <a name="openshift-online"></a>OpenShift Online

Online is een Red Hat-beheerde *multitenant* OpenShift die gebruikmaakt van Container-Platform. Red Hat beheert alle van de onderliggende infrastructuur (zoals virtuele machines, OpenShift-cluster, netwerken en opslag). 

Met deze versie, de klant wordt geïmplementeerd containers, maar heeft geen controle over welke hosts de containers worden uitgevoerd. Omdat Online met meerdere tenants is, kunnen de containers zich bevinden op dezelfde VM-hosts als containers van andere klanten. De kosten zijn per container.

## <a name="next-steps"></a>Volgende stappen

- [Algemene vereisten voor OpenShift configureren in Azure](./openshift-prerequisites.md)
- [OpenShift Containerplatform in Azure implementeren](./openshift-container-platform.md)
- [OKD in Azure implementeren](./openshift-okd.md)
- [OpenShift in Azure Stack implementeren](./openshift-azure-stack.md)
- [Taken na de implementatie](./openshift-post-deployment.md)
- [OpenShift-implementatie oplossen](./openshift-troubleshooting.md)
