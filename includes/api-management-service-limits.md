---
title: bestand opnemen
description: bestand opnemen
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 03/22/2018
ms.author: vlvinogr
ms.custom: include file
ms.openlocfilehash: 676bd3b3a369e3f834016f626a8e69f6b41d9b23
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52170697"
---
| Resource | Limiet |
| --- | --- |
| Eenheden van de schaal | 10 per regio<sup>1</sup> |
| Cache | 5 GB per eenheid<sup>1</sup> |
| Gelijktijdige back-end verbindingen<sup>2</sup> per HTTP-instantie | 2048 per eenheid<sup>3</sup> |
| Maximumgrootte van antwoorden in de cache | 2MB |
| Beleid voor maximale documentgrootte | 256KB |
| Gateway voor maximum aantal aangepaste domeinen | 20 per service-exemplaar<sup>4</sup> |


<sup>1</sup>API Management-limieten zijn verschillend voor elke prijscategorie. Voor het Prijsoverzicht-lagen en hun limieten voor schalen gaat u naar [API Management-prijzen](https://azure.microsoft.com/pricing/details/api-management/).
<sup>2</sup> verbindingen zijn gegroepeerd en opnieuw gebruikt, tenzij expliciet gesloten door de back-end.
<sup>3</sup> per eenheid van de lagen Basic, Standard en Premium. Developer-laag is beperkt tot 1024.
<sup>4</sup> beschikbaar in Premium-laag.


