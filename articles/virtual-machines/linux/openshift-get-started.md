---
title: OpenShift in Azure overzicht | Microsoft Docs
description: Een overzicht van OpenShift in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
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
ms.openlocfilehash: c8e740a66271c88b3abb036867d1760cc9e77607
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33944498"
---
# <a name="openshift-in-azure"></a>OpenShift in Azure

OpenShift is een open en uitbreidbaar container toepassingsplatform die Docker en Kubernetes naar de onderneming brengt.  

OpenShift bevat Kubernetes voor container orchestration en -beheer. Ontwikkelaars en operations-gericht hulpprogramma's waarmee wordt toegevoegd:

- Snelle ontwikkeling van toepassingen.
- Eenvoudige implementatie en schalen.
- Op lange termijn onderhoud van de levenscyclus voor teams en toepassingen.

Er zijn meerdere versies van OpenShift beschikbaar:

- OpenShift Origin
- OpenShift Container Platform
- OpenShift Online
- OpenShift toegewezen

Van de vier versies die in dit artikel slechts twee beschikbaar voor klanten om te implementeren in Azure: OpenShift oorsprong en OpenShift Container Platform.

## <a name="openshift-origin"></a>OpenShift Origin

Oorsprong is een [open source](https://www.openshift.org/) upstream-project van OpenShift die community ondersteund. Oorsprong kan worden geïnstalleerd op CentOS of Red Hat Enterprise Linux (RHEL).

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Container Platform is een enterprise-ready [commerciële versie](https://www.openshift.com) uit en ondersteund door Red Hat. Met deze versie klanten de benodigde rechten voor OpenShift Container Platform kopen en verantwoordelijk zijn voor installatie en beheer van de gehele infrastructuur.

Omdat klanten 'eigenaar' van het platform, kunnen ze deze installeren in hun on-premises datacentrum of in een openbare cloud (zoals Azure, AWS of Google).

## <a name="openshift-online"></a>OpenShift Online

Online is een Red Hat beheerde *multitenant* OpenShift die gebruikmaakt van de Container-Platform. Red Hat beheert alle van de onderliggende infrastructuur (zoals virtuele machines, OpenShift cluster, netwerken en opslag). 

Met deze versie, de klant containers worden geïmplementeerd, maar heeft geen controle over welke hosts de containers worden uitgevoerd. Omdat Online multitenant is, kunnen de containers zich bevinden op dezelfde VM-hosts als containers van andere klanten. Er is een kosten per container.

## <a name="openshift-dedicated"></a>OpenShift toegewezen

Speciale is een Red Hat beheerde *één tenant* OpenShift die gebruikmaakt van de Container-Platform. Red Hat beheert alle van de onderliggende infrastructuur (VM's, OpenShift cluster, netwerken, opslag, enz.). Het cluster is specifiek voor één klant en wordt uitgevoerd in een openbare cloud (zoals AWS of Google met Azure binnenkort in vroege 2018). Een begin-cluster bevat vier knooppunten van de toepassing voor $48,000 per jaar (vooraf betaald).

## <a name="next-steps"></a>Volgende stappen

- [Algemene vereisten voor OpenShift configureren in Azure](./openshift-prerequisites.md)
- [OpenShift oorsprong in Azure implementeren](./openshift-origin.md)
- [OpenShift Container Platform in Azure implementeren](./openshift-container-platform.md)
- [Taken na de implementatie](./openshift-post-deployment.md)
- [Problemen met OpenShift implementatie oplossen](./openshift-troubleshooting.md)
