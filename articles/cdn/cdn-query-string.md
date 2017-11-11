---
title: Beheren van Azure CDN cachegedrag met queryreeksen | Microsoft Docs
description: Azure CDN-queryreeks opslaan in cache bepaalt hoe bestanden worden opgeslagen wanneer ze queryreeksen bevatten.
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
ms.date: 11/09/2017
ms.author: mazha
ms.openlocfilehash: 28e724f34c32edb0d5641b24f9ffedb7dc5f9680
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="control-azure-content-delivery-network-caching-behavior-with-query-strings"></a>Besturingselement Azure Content Delivery Network cachegedrag met queryreeksen
> [!div class="op_single_selector"]
> * [Standard](cdn-query-string.md)
> * [Azure CDN Premium van Verizon](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Overzicht
Met Azure Content Delivery Network (CDN), kunt u bepalen hoe bestanden in cache zijn opgeslagen voor een webaanvraag die een queryreeks bevat. In een webaanvraag met een queryreeks bevat de queryreeks is het gedeelte van de aanvraag die deze gebeurtenis treedt op nadat de `?` teken. Een queryreeks kunt bevatten een of meer parameters die worden gescheiden door een `&` teken. Bijvoorbeeld `http://www.domain.com/content.mov?data1=true&data2=false`. Als er meer dan een queryreeksparameter opgeven in een aanvraag, is de volgorde van de parameters niet van belang. 

> [!IMPORTANT]
> De standard en premium-CDN-producten dezelfde querytekenreeks cache-functionaliteit bieden, maar de gebruikersinterface is.  In dit artikel beschrijft de interface voor **Azure CDN Standard van Akamai** en **Azure CDN Standard van Verizon**. Voor deze query opslaan in cache met **Azure CDN Premium van Verizon**, Zie [beheren cachegedrag van CDN aanvragen met querytekenreeksen - Premium](cdn-query-string-premium.md).

Drie beschikbare modi voor query-tekenreeks zijn beschikbaar:

- **Querytekenreeksen negeren**: standaardmodus. In deze modus wordt het CDN edge-knooppunt geeft de queryreeksen van de aanvrager naar de oorsprong van de eerste aanvraag en plaatst de asset. Alle volgende aanvragen voor de asset die worden aangeboden via het edge-knooppunt negeert de queryreeksen totdat de activa in de cache verloopt.
- **Overslaan voor querytekenreeksen**: In deze modus kan aanvragen met queryreeksen zijn niet in cache opgeslagen op de edge-knooppunt van het CDN. Het edge-knooppunt ophalen van de asset rechtstreeks vanuit de oorsprong en wordt doorgegeven aan de aanvrager aan elke aanvraag.
- **Elke unieke URL in de cache**: In deze modus kan elke aanvraag met een unieke URL, met inbegrip van de queryreeks wordt behandeld als een unieke activum met een eigen cache. Bijvoorbeeld: de reactie van de oorsprong van een aanvraag voor `example.ashx?q=test1` is opgeslagen in de cache op de edge-knooppunt en voor daaropvolgende caches met dezelfde querytekenreeks geretourneerd. Een aanvraag voor `example.ashx?q=test2` in cache wordt opgeslagen als een afzonderlijk actief door een eigen time-to-live-instelling.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Het wijzigen van de queryreeks opslaan in cache-instellingen voor standaard CDN-profielen
1. Een CDN-profiel te openen en selecteer vervolgens het CDN-eindpunt dat u wilt beheren.
   
   ![CDN-profiel-eindpunten](./media/cdn-query-string/cdn-endpoints.png)
   
2. Klik op instellingen **Cache**.
   
    ![Knop voor CDN-profiel-Cache](./media/cdn-query-string/cdn-cache-btn.png)
   
3. In de **queryreeks cachegedrag** lijst, selecteert u een query-tekenreeks-modus en klik op **opslaan**.
   
  <!--- Replace screen shot after general caching goes live ![CDN query string caching options](./media/cdn-query-string/cdn-query-string.png) --->

> [!IMPORTANT]
> Omdat het duurt om de registratie te geven in CDN, wijzigingen in de cache tekenreeks mogelijk niet meteen zichtbaar. Profielen van **Azure CDN van Akamai** worden doorgaans binnen één minuut doorgegeven. Profielen van **Azure CDN van Verizon** worden doorgaans binnen 90 minuten doorgegeven, maar in sommige gevallen kan dit langer duren.


