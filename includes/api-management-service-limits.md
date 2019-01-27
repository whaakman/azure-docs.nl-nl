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
ms.openlocfilehash: 646ebc79e3615b289b60e47c0a38595e7892d8cc
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55084888"
---
| Resource | Limiet |
| --- | --- |
| Maximum aantal schaaleenheden | 10 per regio<sup>1</sup> |
| Cachegrootte | 5 GB per eenheid<sup>2</sup> |
| Gelijktijdige back-end verbindingen<sup>3</sup> per HTTP-instantie | 2048 per eenheid<sup>4</sup> |
| Maximumgrootte van antwoorden in de cache | 2MB |
| Beleid voor maximale documentgrootte | 256KB<sup>5</sup> | 
| Maximum aantal aangepaste gateway domeinen per service-exemplaar<sup>6</sup> | 20 | 
| Maximum aantal exemplaren van de service per abonnement<sup>7</sup> | 20 | 
| Maximum aantal abonnementen per service-exemplaar<sup>7</sup> | 500 |
| Maximum aantal clientcertificaten per service-exemplaar<sup>7</sup> | 50 | 
| Maximum aantal API's per service-exemplaar<sup>7</sup> | 50 | 
| Maximum aantal API-bewerkingen per service-exemplaar<sup>7</sup> | 1000 | 
| Voor maximale totale aanvraagduur<sup>7</sup> | 30 seconden | 
| Maximale pakketgrootte gebufferde<sup>7</sup> | 2MB | 


<sup>1</sup> limieten voor schalen, is afhankelijk van de prijscategorie. Voor het Prijsoverzicht-lagen en hun limieten voor schalen gaat u naar [API Management-prijzen](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>2</sup> per eenheid cache grootte is afhankelijk van de prijscategorie. Voor het Prijsoverzicht-lagen en hun limieten voor schalen gaat u naar [API Management-prijzen](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>3</sup> verbindingen zijn gegroepeerd en opnieuw gebruikt, tenzij expliciet gesloten door de back-end.<br/>
<sup>4</sup> per eenheid van de lagen Basic, Standard en Premium. De Developer-laag is beperkt tot 1024. Niet van toepassing op de laag verbruik.<br/> 
<sup>5</sup> in de Basic, Standard en Premium-lagen. In het verbruik is laag beleid documentgrootte beperkt tot 4KB.<br/>
<sup>6</sup> beschikbaar in de laag Premium.<br/>
<sup>7</sup> van toepassing op het verbruik alleen in de laag.<br/>



