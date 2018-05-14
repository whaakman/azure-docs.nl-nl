---
title: Een toepassing toepassingsproxy duurt te lang laden | Microsoft Docs
description: Pagina load prestatieproblemen met de Azure AD-toepassingsproxy oplossen
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 517f78f811d57e06b4c6e6f0fad24d3b0f96b6e3
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Een toepassing toepassingsproxy duurt te lang om te laden

In dit artikel helpt u om te begrijpen waarom een Azure AD-toepassingsproxy-toepassing kan lang duren om te laden. Ook wordt uitgelegd wat u kunt doen om dit probleem te verhelpen.

## <a name="overview"></a>Overzicht
Hoewel u uw toepassingen werkt, kunnen ze hebben een lange latentie. Er is mogelijk netwerk topologie trucs die u kunt snelheid verhogen. Zie voor een evaluatie van verschillende topologieën, de [netwerk overwegingen document](application-proxy-network-topology-considerations.md).

Naast de netwerktopologie zijn er momenteel geen aanbevelingen meer voor prestaties afstemmen. Als de toepassingsproxy service breidt deze mogelijk geleverd aan een datacenter die zich fysiek dichter. De dichter bij de kan u helpen met een latentie. Zie voor een overzicht van Azure-datacenters, de [latentie testpagina](http://www.azurespeed.com/Azure/Latency). 

De datacenters met de service voor toepassingsproxy kunnen gevonden met de [Connectorhulpprogramma poorten Test](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Feedback over toepassingsproxy data center-locaties 
Mogelijk zijn er Azure-datacenters die niet nog toepassingsproxy omvatten, maar zou leiden tot een verbetering geweldige latentie voor u. Locatie voor de data center verzenden aadapfeedback@microsoft.com. Microsoft gebruikt uw feedback voor uitbreidingsplannen.

Microsoft werkt op de aanvullende mogelijkheden voor het verbeteren van de latentie. Zodra deze verbeteringen beschikbaar zijn, kunt u de documentatie worden bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
[Werken met bestaande lokale proxyservers](application-proxy-working-with-proxy-servers.md)
