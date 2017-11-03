---
title: Overzicht van de load balancer Internetgericht | Microsoft Docs
description: Overzicht voor Internet gerichte load balancer en de bijbehorende functies. Hoe werkt een load balancer voor virtuele machines en cloudservices met Azure.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: tysonn
ms.assetid: 529b37aa-a45c-41d1-8877-fee8cc1fa375
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 5b9ffeadf6b1ffc4eaf4f49b85ba752c27da0e46
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="internet-facing-load-balancer-overview"></a>Internet gerichte load balancer-overzicht

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure load balancer wijst het openbare IP-adres en poort aantal binnenkomend verkeer naar het particuliere IP-adres en poort nummer van de virtuele machine en omgekeerd voor het verkeer van het antwoord van de virtuele machine. Regels voor taakverdeling kunnen u bepaalde soorten verkeer tussen meerdere virtuele machines of services distribueren. Bijvoorbeeld, kunt u het laden van de aanvraag webverkeer spreiden meerdere webservers of web-rollen.

Voor een cloudservice die exemplaren van webrollen of werkrollen bevat, kunt u een openbaar eindpunt definiëren in het servicedefinitiebestand (.csdef).

De *servicedefinition.csdef* bestand bevat de eindpuntconfiguratie en wanneer er meerdere rolexemplaren voor een implementatie van web- of worker-rol, de load balancer worden ingesteld voor het. Het aantal exemplaren op het serviceconfiguratiebestand (.csfg) is het wijzigen van de manier waarop exemplaren toevoegen aan uw implementatie van de cloud.

## <a name="example-of-an-internet-facing-load-balancer"></a>Voorbeeld van een Internetgericht Load Balancer

De volgende afbeelding ziet een eindpunt met gelijke taakverdeling voor internetverkeer die wordt gedeeld door drie virtuele machines voor de openbare en persoonlijke TCP-poort 80. Deze drie virtuele machines zijn in een set met gelijke taakverdeling.

![openbare load balancer-voorbeeld](./media/load-balancer-internet-overview/IC727496.png)

Afbeelding 1 - eindpunt voor internetverkeer Netwerktaakverdeling

Wanneer Internet-clients webpagina-aanvragen naar het openbare IP-adres van de cloudservice op TCP-poort 80 verzenden, distribueert de Azure Load Balancer de aanvragen tussen de drie virtuele machines in de set met gelijke taakverdeling. Zie voor meer informatie over de algoritmen voor load balancer, het [overzichtspagina van load balancer](load-balancer-overview.md#load-balancer-features).

Azure Load Balancer distribueert standaard netwerkverkeer wordt evenredig verdeeld over meerdere exemplaren van de virtuele machine. U kunt ook de affiniteit van de sessie configureren voor meer informatie Zie [load balancer-distributie modus](load-balancer-distribution-mode.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [interne load balancer](load-balancer-internal-overview.md) om beter te begrijpen welke load balancer is beter geschikt zijn voor uw implementatie van de cloud.

U kunt ook [maken van de load balancer van een Internetgericht](load-balancer-get-started-internet-arm-ps.md) en configureren van welk type [distributie modus](load-balancer-distribution-mode.md) voor een specifieke load balancer-netwerk het gedrag van verkeer.

Als uw toepassing verbindingen actief moet houden voor servers achter een load balancer, krijgt u meer inzicht in [niet-actieve TCP-time-outinstellingen voor een load balancer](load-balancer-tcp-idle-timeout.md). Op deze manier krijgt u meer informatie over het gedrag van niet-actieve verbindingen wanneer u Azure Load Balancer gebruikt.
