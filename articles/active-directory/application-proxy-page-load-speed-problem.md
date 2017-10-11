---
title: Een toepassing toepassingsproxy duurt te lang laden | Microsoft Docs
description: Pagina load prestatieproblemen met de Azure AD-toepassingsproxy oplossen
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: ce462c90746e6af0dc201686557121665b82b93d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Een toepassing toepassingsproxy duurt te lang om te laden

In dit artikel helpt u bij het begrijpen waarom een Azure AD-toepassingsproxy-toepassing kan lang duren om te laden en wat u kunt doen om dit probleem te verhelpen.

## <a name="overview"></a>Overzicht
Als uw toepassingen werken, maar u een lange latentie ziet, mogelijk zijn er enkele kleine trucs in uw netwerktopologie die u overwegen kunt de snelheid verhogen. Zie voor een evaluatie van verschillende topologieën, de [netwerk overwegingen document](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).

Als deze overwegingen niet helpt, er zijn momenteel hebben helaas momenteel geen verdere aanbevelingen voor prestaties afstemmen. Als de service voor toepassingsproxy wordt uitgebreid naar meer datacenters die mogelijk dichter bij u, kunt u gaan Zie verbeterde latentie rechtstreeks. De volledige lijst van Azure-datacenters wilt bekijken, ziet u de [latentie testpagina](http://www.azurespeed.com/Azure/Latency). 

De datacenters met de service voor toepassingsproxy kunnen gevonden met de [Connectorhulpprogramma poorten Test](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Feedback over toepassingsproxy data center-locaties 
Mogelijk zijn er Azure-datacenters die toepassingsproxy nog niet zijn maar zou leiden tot een verbetering geweldige latentie voor u. De locatie van het datacentrum < aadapfeedback@microsoft.com > zodat we uw feedback als we vouw plannen kunt gebruiken.

We werken aan enkele aanvullende mogelijkheden die de latentie verbeteren voor tenants die momenteel Zie lang latenties en zorg ervoor dat voor het delen van documentatie zodra deze beschikbaar is.

## <a name="next-steps"></a>Volgende stappen
[Werken met bestaande lokale proxyservers](application-proxy-working-with-proxy-servers.md)
