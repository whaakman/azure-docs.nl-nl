---
title: VMware-oplossing door CloudSimple - Azure openbaar IP-adres
description: Meer informatie over openbare IP-adressen en hun voordelen op VMware-oplossing door CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f57b7397f4a2d288cd2b8b55cf23b2d635aa5f8c
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65209553"
---
# <a name="cloudsimple-public-ip-address-overview"></a>Overzicht van CloudSimple openbare IP-adres

Een openbaar IP-adres kunt resources op internet om te communiceren inkomend, in de privécloud bronnen op een privé IP-adres. Het privé IP-adres is een virtuele machine of een software load balancer. Het privé IP-adres is op de vCenter-privécloud. Het openbare IP-adres kunt u services die worden uitgevoerd op uw privécloud naar internet blootstellen.

Het openbare IP-adres is toegewezen aan het privé IP-adres, totdat u deze toewijzing ongedaan maken. Een openbaar IP-adres kan alleen worden toegewezen aan één privé IP-adres.

Het openbare IP-adres een resource die is gekoppeld aan een openbaar IP-adres altijd gebruikt voor toegang tot internet. Alleen uitgaande internettoegang is standaard toegestaan op een openbaar IP-adres.  Verkeer dat binnenkomt op het openbare IP-adres wordt geweigerd.  Als u wilt dat inkomend verkeer, een firewallregel voor het openbare IP-adres op de specifieke poort te maken.

## <a name="benefits"></a>Voordelen

Een openbaar IP-adres gebruiken om te communiceren inkomende biedt:

* Distributed Denial of service (DDoS) aanvallen om te voorkomen. Deze beveiliging is automatisch ingeschakeld voor het openbare IP-adres.
* Verkeer altijd risicobeperking bewakings- en realtime van algemene op netwerkniveau aanvallen. Deze beveiliging zijn de dezelfde beveiliging die wordt gebruikt door Microsoft online services.
* De volledige omvang van het wereldwijde netwerk van Azure. Het netwerk kan worden gebruikt om te distribueren en te verhelpen aanval verkeer tussen regio's.  

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [openbaar IP-adres toewijzen](https://docs.azure.cloudsimple.com/public-ips/)
