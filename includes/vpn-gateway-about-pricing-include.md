---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 205c67377e4bff66c02e3ee508c0f6f4e2823608
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
U betaalt voor twee dingen: het uurtarief voor het berekenen van de gateway van het virtuele netwerk en de overdracht van egress-gegevens van de gateway van het virtuele netwerk. Prijsinformatie vindt u op de pagina [Prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway).

**Berekeningskosten voor de virtuele netwerkgateway**<br>Voor de berekening van elke virtuele netwerkgateway wordt een uurtarief in rekening gebracht. De prijs is gebaseerd op de gateway-SKU die u opgeeft wanneer u een virtuele netwerkgateway maakt. De kosten zijn voor de gateway zelf en komen boven op de kosten voor de gegevensoverdracht die via de gateway verloopt.

**Kosten voor de gegevensoverdracht**<br>De kosten voor de gegevensoverdracht worden berekend op basis van uitgaand verkeer van de bron voor de gateway van het virtuele netwerk.

* Als u verkeer naar uw on-premises VPN-apparaat verzendt, wordt dit in rekening gebracht met de overdrachtssnelheid van egress-gegevens via het internet.
* Als u verkeer tussen virtuele netwerken in verschillende gebieden verzendt, zijn de prijzen gebaseerd op de regio.
* Als u alleen verkeer verzendt tussen virtuele netwerken die zich in dezelfde regio bevinden, worden er geen gegevenskosten in rekening gebracht. Verkeer tussen VNets in dezelfde regio is gratis.