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
ms.openlocfilehash: 04e5499011e72dfcc20dff370d5d837227ed29b6
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/21/2017
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Gebruikspatronen Azure CDN analyseren

Nadat u CDN voor uw toepassing inschakelt, kunt u gebruik van CDN controleren, Controleer de status van uw levering en mogelijke problemen. Azure CDN biedt deze mogelijkheden in de volgende twee manieren: 

## <a name="verizon-core-reports"></a>Rapporten van Verizon Core

U kunt als Azure CDN-gebruiker met een standaard Verizon of een profiel voor het premium van Verizon Verizon Core rapporten weergeven in de aanvullende portal Verizon. Rapporten van Verizon Core is toegankelijk via de **beheren** optie uit de Azure-portal en biedt een verscheidenheid aan grafieken en weergaven. Zie voor meer informatie [Core rapporten van Verizon](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Verizon aangepaste rapporten

U kunt als Azure CDN-gebruiker met een standaard Verizon of een profiel voor het premium van Verizon Verizon aangepaste rapporten weergeven in de aanvullende portal Verizon. Aangepaste rapporten van Verizon is toegankelijk via de **beheren** optie uit de Azure-portal. De aangepaste rapporten van Verizon pagina ziet u het aantal treffers of gegevens voor elk overgedragen van de rand CName die horen bij een Azure CDN-profiel. De gegevens kunnen worden gegroepeerd op http-code of cache antwoordstatus gedurende een bepaalde periode. Zie voor meer informatie [aangepaste rapporten van Verizon](cdn-verizon-custom-reports.md).

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Basisanalyse via Azure diagnostische logboeken

Basisanalyse is beschikbaar voor alle CDN-eindpunten die horen bij Verizon (standaard en Premium) en (standaard) CDN van Akamai profielen. Azure diagnostics-logboeken toestaan basisanalyse worden geëxporteerd naar Azure storage, event hubs of logboekanalyse Operations Management Suite (OMS). OMS Log Analytics biedt een oplossing met grafieken die gebruiker configureerbare en aanpasbare zijn. Zie voor meer informatie [Azure diagnostische logboeken](cdn-azure-diagnostic-logs.md).

