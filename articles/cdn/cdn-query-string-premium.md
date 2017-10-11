---
title: Beheren van Azure CDN cachegedrag met queryreeksen - Premium | Microsoft Docs
description: Azure CDN-queryreeks opslaan in cache bepaalt hoe bestanden worden in de cache worden opgeslagen wanneer ze queryreeksen bevatten.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 145067c2ce50b41c4783f4de4052c0e7cb529fc7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium"></a>Besturingselement Azure CDN cachegedrag met queryreeksen - Premium
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium van Verizon](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Overzicht
Queryreeks opslaan in cache bepaalt hoe bestanden worden in de cache worden opgeslagen wanneer ze queryreeksen bevatten.

> [!IMPORTANT]
> De Standard en Premium-CDN-producten, bieden dezelfde querytekenreeks cache-functionaliteit, maar de gebruikersinterface verschilt.  Dit document beschrijft de interface voor **Azure CDN Premium van Verizon**.  Voor deze query opslaan in cache met **Azure CDN Standard van Akamai** en **Azure CDN Standard van Verizon**, Zie [beheren cachegedrag van CDN aanvragen met querytekenreeksen](cdn-query-string.md).
> 
> 

Drie beschikbare modi zijn beschikbaar:

* **Standard-cache**: dit is de standaardmodus.  Het CDN edge-knooppunt geeft de queryreeks van de aanvrager aan de oorsprong op de eerste aanvraag en cache de asset.  Alle volgende aanvragen voor de activa die worden aangeboden via het edge-knooppunt kan de query-tekenreeks worden genegeerd totdat de activa in de cache verloopt.
* **Er is geen cache**: In deze modus kan aanvragen met queryreeksen zijn niet in cache opgeslagen op de edge-knooppunt van het CDN.  Het edge-knooppunt ophalen van de asset rechtstreeks vanuit de oorsprong en wordt doorgegeven aan de aanvrager aan elke aanvraag.
* **unieke cache**: in deze modus wordt elke aanvraag met een queryreeks beschouwd als een unieke activum met een eigen cache.  Bijvoorbeeld: de reactie van de oorsprong van een aanvraag voor *foo.ashx?q=bar* zou worden in de cache opgeslagen op de edge-knooppunt en voor daaropvolgende caches met die dezelfde querytekenreeks geretourneerd.  Een aanvraag voor *foo.ashx?q=somethingelse* zou in de cache opgeslagen als een afzonderlijk actief met een eigen tijd levensduur.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Het wijzigen van de queryreeks opslaan in cache-instellingen voor premium-CDN-profielen
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop blade CDN-profiel beheren](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
2. Beweeg de muisaanwijzer over de **HTTP grote** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Cache-instellingen** doel.  Klik op **queryreeks Caching**.
   
    Queryreeks cache-opties worden weergegeven.
   
    ![CDN-queryreeks cacheopties](./media/cdn-query-string-premium/cdn-query-string.png)
3. Nadat u hebt geselecteerd, klikt u op de **Update** knop.

> [!IMPORTANT]
> Wijzigingen in de instellingen zijn mogelijk niet direct weergegeven als het duurt om de registratie te geven in CDN.  Profielen van <b>Azure CDN van Verizon</b> worden doorgaans binnen 90 minuten doorgegeven, maar in sommige gevallen kan dit langer duren.
> 
> 

