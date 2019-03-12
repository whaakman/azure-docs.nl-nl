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
ms.openlocfilehash: fc945a7e9389c8aec48a6a1dba969fbf92002d3a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553373"
---
| Resource | Limiet |
| --- | --- |
| Maximum aantal schaaleenheden | 10 per regio<sup>1</sup> |
| Cachegrootte | 5 GB per eenheid<sup>2</sup> |
| Gelijktijdige verbindingen voor back-end<sup>3</sup> per HTTP-instantie | 2048 per eenheid<sup>4</sup> |
| Maximumgrootte van antwoorden in de cache | 2 MB |
| Beleid voor maximale documentgrootte | 256 KB<sup>5</sup> | 
| Maximum aantal aangepaste gateway domeinen per service-exemplaar<sup>6</sup> | 20 |
| Maximum aantal van CA-certificaten per service-exemplaar | 10 | 
| Maximum aantal exemplaren van de service per abonnement<sup>7</sup> | 20 | 
| Maximum aantal abonnementen per service-exemplaar<sup>7</sup> | 500 |
| Maximum aantal clientcertificaten per service-exemplaar<sup>7</sup> | 50 | 
| Maximum aantal API's per service-exemplaar<sup>7</sup> | 50 | 
| Maximum aantal API-bewerkingen per service-exemplaar<sup>7</sup> | 1000 | 
| Voor maximale totale aanvraagduur<sup>7</sup> | 30 seconden | 
| Maximale pakketgrootte gebufferde<sup>7</sup> | 2 MB | 


<sup>1</sup>limieten voor schalen, is afhankelijk van de prijscategorie. Zie voor de Prijscategorieën en hun limieten voor schalen [API Management-prijzen](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>2</sup>per eenheid cache grootte is afhankelijk van de prijscategorie. Zie voor de Prijscategorieën en hun limieten voor schalen [API Management-prijzen](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>3</sup>verbindingen zijn gegroepeerd en opnieuw gebruikt, tenzij expliciet gesloten door de back-end.<br/>
<sup>4</sup>deze limiet is per eenheid van de lagen Basic, Standard en Premium. De Developer-laag is beperkt tot 1024. Deze limiet niet van toepassing op de laag verbruik.<br/> 
<sup>5</sup>deze limiet geldt voor de lagen Basic, Standard en Premium. In de laag verbruik is documentgrootte beleid beperkt tot 4 KB.<br/>
<sup>6</sup>deze resource is beschikbaar in de laag Premium.<br/>
<sup>7</sup>deze resource is van toepassing op het verbruik alleen in de laag.<br/>



