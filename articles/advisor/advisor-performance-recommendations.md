---
title: Azure Advisor prestaties aanbevelingen | Microsoft Docs
description: Advisor gebruiken om de prestaties van uw Azure-implementaties te optimaliseren.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: e32723cd3ef13829890a630f4bff308164e17674
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2017
---
# <a name="advisor-performance-recommendations"></a>Aanbevelingen van Advisor-prestaties

Azure aanbevelingen van Advisor prestaties te verbeteren en de reactiesnelheid van uw bedrijfskritieke toepassingen. U kunt prestaties aanbevelingen van Advisor ophalen op de **prestaties** van de Advisor-dashboard.

## <a name="improve-database-performance-with-sql-db-advisor"></a>Verbeteren de databaseprestaties met SQL database Advisor

Advisor biedt een consistente, geconsolideerde weergave van aanbevelingen voor alle Azure-resources. Het is geïntegreerd met SQL Database Advisor zodat u de aanbevelingen voor het verbeteren van de prestaties van uw Azure SQL database. SQL Database Advisor beoordeelt de prestaties van uw Azure SQL-database door een analyse van uw gebruiksgeschiedenis. Deze biedt aanbevelingen die geschikt zijn voor het uitvoeren van de normale werkbelasting van de database vervolgens. 

> [!NOTE]
> Als u de aanbevelingen, moet een database over een week van het gebruik van, en binnen een week moet er een consistente activiteit. SQL Database Advisor kunt eenvoudiger voor consistente querypatronen dan voor willekeurige lichtflitsen activiteit optimaliseren.

Zie voor meer informatie over SQL Database Advisor [SQL Database Advisor](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).

## <a name="improve-redis-cache-performance-and-reliability"></a>De Redis-Cache-prestaties en betrouwbaarheid verbeteren

Advisor identificeert exemplaren van Redis-Cache waarbij prestaties mogelijk negatief beïnvloed door hoog geheugengebruik, serverbelasting, bandbreedte van het netwerk of een groot aantal clientverbindingen. Advisor biedt ook aanbevolen procedures aanbevelingen om te voorkomen, potentiële problemen. Zie voor meer informatie over Redis-Cache aanbevelingen [Redis-Cache Advisor](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>App Service-prestaties en betrouwbaarheid te verbeteren

Azure Advisor kan worden geïntegreerd met aanbevelingen voor uw App Services-ervaring te verbeteren en relevante platformmogelijkheden detecteren. Voorbeelden van App Services aanbevelingen zijn:
* Detectie van exemplaren waar het geheugen of CPU-bronnen zijn uitgeput door app runtimes met risicobeperking opties.
* Detectie van exemplaren waar collocating resources, zoals web-apps en -databases kan de prestaties en lagere kosten verbeteren. 

Zie voor meer informatie over App-Services aanbevelingen [aanbevolen procedures voor het Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Toegang tot de aanbevelingen van de prestaties in Advisor

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2.  Klik op het dashboard Advisor de **prestaties** tabblad.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Advisor aanbevelingen:

* [Inleiding tot Advisor](advisor-overview.md)
* [Aan de slag met Advisor](advisor-get-started.md)
* [Advisor kosten aanbevelingen](advisor-performance-recommendations.md)
* [Hoge beschikbaarheid aanbevelingen Advisor te ontvangen](advisor-high-availability-recommendations.md)
* [Aanbevelingen voor beveiliging van Advisor](advisor-security-recommendations.md)

