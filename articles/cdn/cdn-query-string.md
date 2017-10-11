---
title: Beheren van Azure CDN cachegedrag met queryreeksen | Microsoft Docs
description: Azure CDN-queryreeks opslaan in cache bepaalt hoe bestanden worden in de cache worden opgeslagen wanneer ze queryreeksen bevatten.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 8d79626fa8516f226a82d3dac693c2033904c91d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings"></a>Besturingselement Azure CDN cachegedrag met queryreeksen
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium van Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Overzicht
Queryreeks opslaan in cache bepaalt hoe bestanden worden in de cache worden opgeslagen wanneer ze queryreeksen bevatten.

> [!IMPORTANT]
> De Standard en Premium-CDN-producten, bieden dezelfde querytekenreeks cache-functionaliteit, maar de gebruikersinterface verschilt.  Dit document beschrijft de interface voor **Azure CDN Standard van Akamai** en **Azure CDN Standard van Verizon**.  Voor deze query opslaan in cache met **Azure CDN Premium van Verizon**, Zie [beheren cachegedrag van CDN aanvragen met querytekenreeksen - Premium](cdn-query-string-premium.md).
> 
> 

Drie beschikbare modi zijn beschikbaar:

* **Querytekenreeksen negeren**: dit is de standaardmodus.  Het CDN edge-knooppunt geeft de queryreeks van de aanvrager aan de oorsprong op de eerste aanvraag en cache de asset.  Alle volgende aanvragen voor de activa die worden aangeboden via het edge-knooppunt kan de query-tekenreeks worden genegeerd totdat de activa in de cache verloopt.
* **Overslaan voor URL's met querytekenreeksen**: In deze modus kan aanvragen met queryreeksen zijn niet in cache opgeslagen op de edge-knooppunt van het CDN.  Het edge-knooppunt ophalen van de asset rechtstreeks vanuit de oorsprong en wordt doorgegeven aan de aanvrager aan elke aanvraag.
* **Elke unieke URL in de cache**: in deze modus wordt elke aanvraag met een queryreeks beschouwd als een unieke activum met een eigen cache.  Bijvoorbeeld: de reactie van de oorsprong van een aanvraag voor *foo.ashx?q=bar* zou worden in de cache opgeslagen op de edge-knooppunt en voor daaropvolgende caches met die dezelfde querytekenreeks geretourneerd.  Een aanvraag voor *foo.ashx?q=somethingelse* zou in de cache opgeslagen als een afzonderlijk actief met een eigen tijd levensduur.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Het wijzigen van de queryreeks opslaan in cache-instellingen voor standaard CDN-profielen
1. Klik in de blade CDN-profiel op het CDN-eindpunt dat u wilt beheren.
   
    ![CDN-profiel blade-eindpunten](./media/cdn-query-string/cdn-endpoints.png)
   
    De blade CDN-eindpunt wordt geopend.
2. Klik op de **configureren** knop.
   
    ![Knop blade CDN-profiel beheren](./media/cdn-query-string/cdn-config-btn.png)
   
    De configuratie van CDN-blade wordt geopend.
3. Selecteer een instelling van de **queryreeks cachegedrag** vervolgkeuzelijst.
   
    ![CDN-queryreeks cacheopties](./media/cdn-query-string/cdn-query-string.png)
4. Nadat u hebt geselecteerd, klikt u op de **opslaan** knop.

> [!IMPORTANT]
> Wijzigingen in de instellingen zijn mogelijk niet direct weergegeven als het duurt om de registratie te geven in CDN.  Profielen van <b>Azure CDN van Akamai</b> worden doorgaans binnen één minuut doorgegeven.  Profielen van <b>Azure CDN van Verizon</b> worden doorgaans binnen 90 minuten doorgegeven, maar in sommige gevallen kan dit langer duren.
> 
> 

