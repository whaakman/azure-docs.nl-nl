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
ms.openlocfilehash: bee289da3f18edd0cb425f3d9acde084567a3b13
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
| Resource | Limiet |
| --- | --- |
| Eenheden van de schaal | 10 per regio<sup>1</sup> |
| Cache | 5 GB per eenheid<sup>1</sup> |
| Back-end voor gelijktijdige verbindingen<sup>2</sup> per HTTP-instantie | 2048 per eenheid<sup>3</sup> |
| Maximumgrootte van in cache opgeslagen antwoorden | 10MB |
| Maximum aantal aangepaste gateway domeinen | 20 per service-exemplaar<sup>4</sup> |


<sup>1</sup>limieten van API Management zijn verschillend voor elke prijscategorie. Voor de prijzen lagen en de schaal ervan gaat u naar [API Management-prijzen](https://azure.microsoft.com/pricing/details/api-management/).
<sup>2</sup> verbindingen zijn gegroepeerd en opnieuw te gebruiken, tenzij expliciet afgesloten door de back-end.
<sup>3</sup> per eenheid van categorieën Basic, Standard en Premium. Categorie Developer is beperkt tot 1024.
<sup>4</sup> beschikbaar in Premium-laag.


