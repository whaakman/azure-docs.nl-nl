---
title: OpenShift in overzicht van Azure | Microsoft Docs
description: Een overzicht van OpenShift in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/7/2019
ms.author: haroldw
ms.openlocfilehash: c8ebd10544331d3b7d887124542fe4ee2753c714
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233136"
---
# <a name="openshift-in-azure"></a>OpenShift in Azure

OpenShift is een container open en uitbreidbaar platform voor toepassingen die zorgt voor Docker en Kubernetes in de onderneming.  

OpenShift bevat Kubernetes voor containerindeling en beheer. Wordt toegevoegd speciaal voor ontwikkelaars en operations-gebaseerde hulpprogramma's die inschakelen:

- Snel ontwikkelen van toepassingen.
- Eenvoudige implementatie en schalen.
- Op de lange termijn onderhoud van de levenscyclus voor teams en toepassingen.

Er zijn meerdere versies van OpenShift beschikbaar.  Van deze versies zijn slechts twee die momenteel beschikbaar voor klanten om te implementeren in Azure: OpenShift Container Platform en OKD (voorheen OpenShift Origin).

## <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Microsoft Azure Red Hat OpenShift is een volledig beheerde van OpenShift uitvoeren in Azure. Deze service is gezamenlijk beheerd en ondersteund door Microsoft en Red Hat. Volledige documentatie [hier](https://docs.microsoft.com/azure/openshift/). 

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Containerplatform is een bedrijfsklare [commerciële versie](https://www.openshift.com) uit en wordt ondersteund door Red Hat. Met deze versie kunnen klanten de benodigde rechten voor OpenShift Container Platform aanschaffen en verantwoordelijk zijn voor installatie en beheer van de gehele infrastructuur.

Omdat klanten 'eigenaar' van het platform, kunnen ze deze installeren in hun on-premises datacenter of in een openbare cloud (zoals Azure).

## <a name="okd"></a>OKD

OKD is een [open-source](https://www.okd.io/) upstream-project van OpenShift die community ondersteund. OKD kan worden geïnstalleerd op een CentOS of Red Hat Enterprise Linux (RHEL).

## <a name="next-steps"></a>Volgende stappen

- [Algemene vereisten voor OpenShift configureren in Azure](./openshift-prerequisites.md)
- [OpenShift Containerplatform in Azure implementeren](./openshift-container-platform.md)
- [Implementeren van OpenShift Container Platform zelfbeheerde Marketplace-aanbieding](./openshift-marketplace-self-managed.md)
- [OpenShift in Azure Stack implementeren](./openshift-azure-stack.md)
- [Taken na de implementatie](./openshift-post-deployment.md)
- [OpenShift-implementatie oplossen](./openshift-troubleshooting.md)
