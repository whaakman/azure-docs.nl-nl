---
title: Een toepassing Application Proxy duurt te lang om te laden | Microsoft Docs
description: Pagina laden prestatieproblemen met de Azure AD-toepassingsproxy oplossen
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: a4110ceddb55de1990d85597f8c1a9618743f946
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56170428"
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Een toepassing Application Proxy duurt te lang om te laden

In dit artikel helpt u te begrijpen waarom een toepassing met Azure AD Application Proxy kan erg lang duren om te laden. Ook wordt uitgelegd wat u kunt doen om dit probleem te verhelpen.

## <a name="overview"></a>Overzicht
Hoewel uw toepassingen werkt, kunnen ze een lange vertraging optreden. Mogelijk zijn er netwerk-topologie correcties die u maken kunt om te versnellen. Zie voor een evaluatie van verschillende topologieën, de [netwerk overwegingen document](application-proxy-network-topology.md).

Naast de netwerktopologie zijn er momenteel geen verdere aanbevelingen voor het afstemmen van prestaties. Als de service uitgebreid toepassingsproxy kan deze naar een datacenter die zich fysiek dichter zijn. De dichter bij kan helpen bij het latentie. Zie voor een lijst van Azure-datacenters, de [latentie testpagina](http://www.azurespeed.com/Azure/Latency). 

De datacenters met de Application Proxy-service kunnen worden gevonden in de [verbindingslijn poorten Test](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Feedback over de toepassingsproxy data center locaties 
Het is mogelijk dat er Azure-datacenters die niet nog Application Proxy bevatten, maar zou leiden tot een verbetering geweldige latentie voor u. De locatie voor het datacenter verzenden aadapfeedback@microsoft.com. Microsoft gebruikt uw feedback voor het plannen van de uitbreiding.

Microsoft werkt aan aanvullende mogelijkheden voor het verbeteren van latentie. Zodra deze verbeteringen beschikbaar zijn, kunt u de documentatie wordt bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
[Werken met bestaande on-premises proxy-servers](application-proxy-configure-connectors-with-proxy-servers.md)
