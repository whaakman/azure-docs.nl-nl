---
title: Gebruikspatronen Azure CDN analyseren | Microsoft Docs
description: In dit artikel beschrijft de verschillende soorten analyserapporten die beschikbaar zijn voor Azure CDN-producten.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: rli; v-deasim
ms.openlocfilehash: 61fbe6e29df787048a9694138d3c9095f5cba76b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33764890"
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Gebruikspatronen van Azure CDN analyseren

Nadat u CDN voor uw toepassing inschakelt, kunt u gebruik van CDN controleren, Controleer de status van uw levering en mogelijke problemen. Azure CDN biedt deze mogelijkheden in de volgende manieren: 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Basisanalyse via Azure diagnostische logboeken

Basisanalyse is beschikbaar voor CDN-eindpunten voor alle Prijscategorieën. Azure diagnostics-logboeken toestaan basisanalyse worden geëxporteerd naar Azure storage, event hubs of Azure-logboekanalyse. Azure Log Analytics biedt een oplossing met grafieken die gebruiker configureerbare en aanpasbare zijn. Zie voor meer informatie over Azure diagnostische logboeken [Azure diagnostische logboeken](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>Rapporten van Verizon core

Als een Azure CDN-gebruiker met een **Azure CDN Standard van Verizon** of **Azure CDN Premium van Verizon** profiel, u kunt Verizon core rapporten weergeven in de aanvullende portal Verizon. Rapporten van Verizon core is toegankelijk via de **beheren** optie uit de Azure-portal en biedt een verscheidenheid aan grafieken en weergaven. Zie voor meer informatie [Core rapporten van Verizon](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Aangepaste rapporten van Verizon

Als een Azure CDN-gebruiker met een **Azure CDN Standard van Verizon** of **Azure CDN Premium van Verizon** profiel, kunt u aangepaste rapporten van Verizon weergeven in de aanvullende portal Verizon. Aangepaste rapporten van Verizon is toegankelijk via de **beheren** optie uit de Azure-portal. Het Verizon aangepaste rapporten pagina bevat het aantal treffers of gegevens voor elk overgedragen van de rand CName die horen bij een Azure CDN-profiel. De gegevens kunnen worden gegroepeerd op http-code of cache antwoordstatus gedurende een bepaalde periode. Zie voor meer informatie [aangepaste rapporten van Verizon](cdn-verizon-custom-reports.md).

## <a name="azure-cdn-premium-from-verizon-reports"></a>Azure CDN Premium van Verizon rapporten

Met **Azure CDN Premium van Verizon**, u kunt ook toegang tot de volgende rapporten:
   * [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)
   * [Realtime statistieken](cdn-real-time-stats.md)
   * [Prestaties van edge-knooppunt](cdn-edge-performance.md)

