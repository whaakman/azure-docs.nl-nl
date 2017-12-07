---
title: Gebruikspatronen Azure CDN analyseren | Microsoft Docs
description: De klant kunt logboekanalyse inschakelen voor Azure CDN.
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: v-semcev
ms.openlocfilehash: ce9ec021250a3548e23ad87273a9225cdf700c70
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Gebruikspatronen Azure CDN analyseren

Nadat u CDN voor uw toepassing inschakelt, kunt u gebruik van CDN controleren, Controleer de status van uw levering en mogelijke problemen. Azure CDN biedt deze mogelijkheden in de volgende manieren: 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Basisanalyse via Azure diagnostische logboeken

Basisanalyse is beschikbaar voor alle CDN-eindpunten die horen bij Verizon (standaard en Premium) en (standaard) CDN van Akamai profielen. Azure diagnostics-logboeken toestaan basisanalyse worden geëxporteerd naar Azure storage, event hubs of logboekanalyse Operations Management Suite (OMS). OMS Log Analytics biedt een oplossing met grafieken die gebruiker configureerbare en aanpasbare zijn. Zie voor meer informatie [Azure diagnostische logboeken](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>Rapporten van Verizon core

U kunt als Azure CDN-gebruiker met een standaard Verizon of een profiel voor het premium van Verizon Verizon core rapporten weergeven in de aanvullende portal Verizon. Rapporten van Verizon core is toegankelijk via de **beheren** optie uit de Azure-portal en biedt een verscheidenheid aan grafieken en weergaven. Zie voor meer informatie [Core rapporten van Verizon](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Aangepaste rapporten van Verizon

Als een gebruiker met Azure CDN met een standaard Verizon of een profiel voor het premium van Verizon kunt u aangepaste rapporten van Verizon weergeven in de aanvullende portal Verizon. Aangepaste rapporten van Verizon is toegankelijk via de **beheren** optie uit de Azure-portal. Het Verizon aangepaste rapporten pagina bevat het aantal treffers of gegevens voor elk overgedragen van de rand CName die horen bij een Azure CDN-profiel. De gegevens kunnen worden gegroepeerd op http-code of cache antwoordstatus gedurende een bepaalde periode. Zie voor meer informatie [aangepaste rapporten van Verizon](cdn-verizon-custom-reports.md).

## <a name="verizon-premium-reports"></a>Rapporten van Verizon premium

Met **Azure CDN Premium van Verizon**, u kunt ook toegang tot de volgende rapporten:
   * [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)
   * [Realtime statistieken](cdn-real-time-stats.md)
   * [Prestaties van Edge-knooppunt](cdn-edge-performance.md)

