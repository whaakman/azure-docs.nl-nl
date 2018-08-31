---
title: OpenShift in overzicht van Azure | Microsoft Docs
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
ms.openlocfilehash: e3ab060c1cea28f83c18dc89aeea7716ec86572a
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190340"
---
# <a name="openshift-in-azure"></a>OpenShift in Azure

OpenShift is een container open en uitbreidbaar platform voor toepassingen die zorgt voor Docker en Kubernetes in de onderneming.  

OpenShift bevat Kubernetes voor containerindeling en beheer. Developer - en operations-gebaseerde hulpprogramma's waarmee wordt toegevoegd:

- Snel ontwikkelen van toepassingen.
- Eenvoudige implementatie en schalen.
- Op de lange termijn onderhoud van de levenscyclus voor teams en toepassingen.

Er zijn meerdere versies van OpenShift beschikbaar:

- OKD (voorheen OpenShift Origin)
- OpenShift Container Platform
- OpenShift Online
- OpenShift Dedicated

Van de vier versies in dit artikel besproken, alleen twee beschikbaar voor klanten om te implementeren in Azure: OpenShift Origin en OpenShift Container Platform.

## <a name="okd-formerly-openshift-origin"></a>OKD (voorheen OpenShift Origin)

OKD is een [open-source](https://www.okd.io/) upstream-project van OpenShift die community ondersteund. OKD kan worden geïnstalleerd op een CentOS of Red Hat Enterprise Linux (RHEL).

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Containerplatform is een bedrijfsklare [commerciële versie](https://www.openshift.com) uit en wordt ondersteund door Red Hat. Met deze versie kunnen klanten de benodigde rechten voor OpenShift Container Platform aanschaffen en verantwoordelijk zijn voor installatie en beheer van de gehele infrastructuur.

Omdat klanten 'eigenaar' van het platform, kunnen ze deze installeren in hun on-premises datacenter of in een openbare cloud (zoals Azure, AWS en Google).

## <a name="openshift-online"></a>OpenShift Online

Online is een Red Hat-beheerde *multitenant* OpenShift die gebruikmaakt van Container-Platform. Red Hat beheert alle van de onderliggende infrastructuur (zoals virtuele machines, OpenShift-cluster, netwerken en opslag). 

Met deze versie, de klant wordt geïmplementeerd containers, maar heeft geen controle over welke hosts de containers worden uitgevoerd. Omdat Online met meerdere tenants is, kunnen de containers zich bevinden op dezelfde VM-hosts als containers van andere klanten. De kosten zijn per container.

## <a name="openshift-dedicated"></a>OpenShift Dedicated

Toegewezen is een Red Hat-beheerde *één tenant* OpenShift die gebruikmaakt van Container-Platform. Red Hat beheert alle van de onderliggende infrastructuur (VM's, OpenShift-cluster, netwerken, opslag, enzovoort.). Het cluster is specifiek voor één klant en wordt uitgevoerd in een openbare cloud (zoals AWS of Google, met Azure binnenkort begin 2018). Een eerste cluster bevat vier knooppunten van de toepassingen voor $48,000 per jaar (vooraf betaald).

## <a name="next-steps"></a>Volgende stappen

- [Algemene vereisten voor OpenShift configureren in Azure](./openshift-prerequisites.md)
- [OpenShift Origin in Azure implementeren](./openshift-origin.md)
- [OpenShift Containerplatform in Azure implementeren](./openshift-container-platform.md)
- [Taken na de implementatie](./openshift-post-deployment.md)
- [OpenShift-implementatie oplossen](./openshift-troubleshooting.md)
