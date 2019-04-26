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
ms.date: 04/19/2019
ms.author: haroldw
ms.openlocfilehash: 53bed2131e81ee5ed0f46bde389262ee8349339a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60542421"
---
# <a name="openshift-in-azure"></a>OpenShift in Azure

OpenShift is een container open en uitbreidbaar platform voor toepassingen die zorgt voor Docker en Kubernetes in de onderneming.  

OpenShift bevat Kubernetes voor containerindeling en beheer. Wordt toegevoegd speciaal voor ontwikkelaars en operations-gebaseerde hulpprogramma's die inschakelen:

- Snel ontwikkelen van toepassingen.
- Eenvoudige implementatie en schalen.
- Op de lange termijn onderhoud van de levenscyclus voor teams en toepassingen.

Er zijn meerdere versies van OpenShift beschikbaar.  Van deze versies zijn slechts twee die momenteel beschikbaar voor klanten om te implementeren in Azure: OpenShift Container Platform en OKD (voorheen OpenShift Origin).

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Containerplatform is een bedrijfsklare [commerciële versie](https://www.openshift.com) uit en wordt ondersteund door Red Hat. Met deze versie kunnen klanten de benodigde rechten voor OpenShift Container Platform aanschaffen en verantwoordelijk zijn voor installatie en beheer van de gehele infrastructuur.

Omdat klanten 'eigenaar' van het platform, kunnen ze deze installeren in hun on-premises datacenter of in een openbare cloud (zoals Azure).

## <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Azure Red Hat OpenShift is een volledig beheerde van OpenShift uitvoeren in Azure. Deze service is gezamenlijk beheerd en ondersteund door Microsoft en Red Hat. Het cluster implementeert in Azure-abonnement van de klant. De service is gepland voor algemene beschikbaarheid rond mei 2019. Afzonderlijke documentatie voor de beheerde service zijn beschikbaar nadat de service is de algemene beschikbaarheid.

## <a name="okd"></a>OKD

OKD is een [open-source](https://www.okd.io/) upstream-project van OpenShift die community ondersteund. OKD kan worden geïnstalleerd op een CentOS of Red Hat Enterprise Linux (RHEL).

## <a name="next-steps"></a>Volgende stappen

- [Algemene vereisten voor OpenShift configureren in Azure](./openshift-prerequisites.md)
- [OpenShift Containerplatform in Azure implementeren](./openshift-container-platform.md)
- [Implementeren van OpenShift Container Platform zelfbeheerde Marketplace-aanbieding](./openshift-marketplace-self-managed.md)
- [OpenShift in Azure Stack implementeren](./openshift-azure-stack.md)
- [Taken na de implementatie](./openshift-post-deployment.md)
- [OpenShift-implementatie oplossen](./openshift-troubleshooting.md)
