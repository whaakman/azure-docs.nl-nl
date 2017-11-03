---
title: OpenShift op Azure overzicht | Microsoft Docs
description: OpenShift op overzicht van Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: f9641b52db91a4356f6d5789a8cd78a6bb3da02b
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="openshift-overview"></a>Overzicht van OpenShift

OpenShift is een open en uitbreidbaar container toepassingsplatform die docker en Kubernetes naar de onderneming brengt.  

OpenShift bevat Kubernetes voor container orchestration en -beheer. Hiermee worden toegevoegd voor ontwikkelaars en bewerkingen gericht hulpprogramma's waarmee:

- Snelle ontwikkeling van toepassingen
- Eenvoudige implementatie en schalen
- Op lange termijn levenscyclus onderhoud voor teams en toepassingen

Er zijn meerdere aanbiedingen van OpenShift van die twee zijn beschikbaar in Azure uit te voeren.

- OpenShift Origin
- OpenShift Container Platform
- OpenShift Online
- OpenShift toegewezen

Van de vier aanbiedingen gedekt, twee zijn beschikbaar voor klanten om te implementeren in Azure op hun eigen - OpenShift oorsprong en OpenShift Container Platform.

## <a name="openshift-origin"></a>OpenShift Origin

[Open-source](https://www.openshift.org/) upstream-project van OpenShift die community ondersteund. Oorsprong kan worden geïnstalleerd op CentOS of RHEL.

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Enterprise-ready ([commerciële aanbieding](https://www.openshift.com)) versie van Red Hat die wordt ondersteund door Red Hat. De klant de benodigde rechten voor OpenShift Container Platform koopt en is verantwoordelijk voor de installatie en beheer van de gehele infrastructuur.

Omdat de klant 'eigenaar' het platform, kan worden geïnstalleerd in hun on-premises datacentrum, openbare cloud (Azure, AWS, Google, enzovoort), enzovoort.

## <a name="openshift-online"></a>OpenShift Online

Red Hat beheerd **multitenant** OpenShift (met behulp van de Container Platform). Red Hat beheert alle onderliggende infrastructuur (VM's, OpenShift cluster, netwerken, opslag, enz.). 

Klant containers worden geïmplementeerd, maar heeft geen controle over welke hosts de containers worden uitgevoerd. Aangezien het multitenant, kunnen containers worden geplaatst op hetzelfde VM-hosts als containers van andere klanten. Er is een kosten per container.

## <a name="openshift-dedicated"></a>OpenShift toegewezen

Red Hat beheerd **één tenant** OpenShift (met behulp van de Container Platform). Red Hat beheert alle onderliggende infrastructuur (VM's, OpenShift cluster, netwerken, opslag, enz.). Het cluster is specifiek voor één klant en wordt uitgevoerd in een openbare cloud (Azure AWS, Google, - binnenkort in vroege 2018). Begin cluster bevat vier knooppunten van de toepassingen voor 48K-jaar (vooraf betaling voor een heel jaar).

## <a name="next-steps"></a>Volgende stappen

- [Algemene vereisten voor OpenShift configureren in Azure](./openshift-prerequisites.md)
- [OpenShift oorsprong implementeren](./openshift-origin.md)
- [OpenShift Container Platform implementeren](./openshift-container-platform.md)
- [Taken voor na implementatie](./openshift-post-deployment.md)
- [Problemen met OpenShift implementatie oplossen](./openshift-troubleshooting.md)
