---
title: Azure Advisor prestatieaanbevelingen | Microsoft Docs
description: Advisor gebruiken om de prestaties van uw Azure-implementaties te optimaliseren.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 3331c795cbb1c45820d4c86d287ef57b54f0ae6b
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247639"
---
# <a name="advisor-performance-recommendations"></a>Advisor-aanbevelingen voor prestaties

Azure Advisor-aanbevelingen voor prestaties te verbeteren en de reactiesnelheid van uw bedrijfskritische toepassingen. U kunt ook aanbevelingen voor prestaties van de Advisor krijgen bij de **prestaties** van de Advisor-dashboard.

## <a name="reduce-dns-time-to-live-on-your-traffic-manager-profile-to-fail-over-to-healthy-endpoints-faster"></a>Time to live van uw Traffic Manager-profiel sneller failover naar de eindpunten in orde van DNS-beperken

[Tijd naar Live (TTL) instellingen](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-performance-considerations) op uw Traffic Manager-profiel kunt u opgeven hoe snel om over te schakelen eindpunten als een bepaald eindpunt niet meer reageert op query's. Vermindering van de TTL-waarden, betekent dat clients worden doorgestuurd naar een functionerende eindpunten sneller.

Azure Advisor identificeert Traffic Manager-profielen met een langer TTL geconfigureerd en aanbevolen configuratie van de TTL-waarde 20 seconden of 60 seconden, afhankelijk van of u het profiel is geconfigureerd voor [Fast Failover](https://azure.microsoft.com/roadmap/fast-failover-and-tcp-probing-in-azure-traffic-manager/).

## <a name="improve-database-performance-with-sql-db-advisor"></a>De prestaties van de database verbeteren met SQL DB Advisor

Advisor biedt u een consistente, geconsolideerde weergave van de aanbevelingen voor al uw Azure-resources. Het is geïntegreerd met SQL Database Advisor voor aanbevelingen voor het verbeteren van de prestaties van uw SQL Azure-database. SQL Database Advisor beoordeelt de prestaties van uw SQL Azure-databases door het analyseren van uw gebruiksgeschiedenis. Vervolgens biedt aanbevelingen die bij uitstek geschikt zijn voor het uitvoeren van de typische werkbelasting van de database. 

> [!NOTE]
> Om aanbevelingen te krijgen, moet een database over een week van het gebruik van, en in de afgelopen week moet er activiteit zijn consistent. SQL Database Advisor kunt eenvoudiger voor consistente querypatronen dan voor willekeurige pieken van activiteit optimaliseren.

Zie voor meer informatie over SQL Database Advisor [SQL Database Advisor](https://azure.microsoft.com/documentation/articles/sql-database-advisor/).

## <a name="improve-redis-cache-performance-and-reliability"></a>Redis-Cache-prestaties en betrouwbaarheid verbeteren

Advisor identificeert waarbij prestaties kan nadelig worden beïnvloed door hoog geheugengebruik, serverbelasting, bandbreedte van het netwerk of een groot aantal clientverbindingen Redis Cache-exemplaren. Advisor biedt ook aanbevolen procedures voor aanbevelingen om te voorkomen van potentiële problemen. Zie voor meer informatie over aanbevelingen voor de Redis-Cache [Redis Cache Advisor](https://azure.microsoft.com/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>App Service-prestaties en betrouwbaarheid verbeteren

Azure Advisor kan worden geïntegreerd met aanbevelingen voor het verbeteren van de ervaring van uw App Services en relevante platformmogelijkheden detecteren. Voorbeelden van aanbevelingen voor de App-Services zijn:
* Detectie van exemplaren waar geheugen of CPU-resources worden uitgeput door app-runtimes met opties voor risicobeperking.
* Detectie van exemplaren waar collocating resources, zoals web-apps en -databases kan verbeteren, prestaties en lagere kosten. 

Zie voor meer informatie over aanbevelingen voor de App Services [Best Practices voor Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-best-practices/).

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Toegang tot de aanbevelingen voor prestaties in Advisor

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2.  Klik op de Advisor-dashboard op de **prestaties** tabblad.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over aanbevelingen van Advisor:

* [Inleiding tot Advisor](advisor-overview.md)
* [Aan de slag met Advisor](advisor-get-started.md)
* [Aanbevelingen van Advisor-kosten](advisor-performance-recommendations.md)
* [Advisor-aanbevelingen voor hoge beschikbaarheid](advisor-high-availability-recommendations.md)
* [Advisor-aanbevelingen voor beveiliging](advisor-security-recommendations.md)

