---
title: Beheren van Azure CDN cachegedrag met queryreeksen - standaardcategorie | Microsoft Docs
description: Azure CDN-queryreeks opslaan in cache bepaalt hoe bestanden worden opgeslagen wanneer een webaanvraag een queryreeks bevat. Dit artikel wordt beschreven queryreeks opslaan in cache in Azure CDN standard producten.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: v-deasim
ms.openlocfilehash: aa553dfc04a755be1169fa117ec66dd10ea75b54
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260427"
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---standard-tier"></a>Besturingselement Azure CDN cachegedrag met queryreeksen - standard-laag
> [!div class="op_single_selector"]
> * [Standaardlaag](cdn-query-string.md)
> * [Premiumlaag](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Overzicht
Met Azure Content Delivery Network (CDN), kunt u bepalen hoe bestanden in cache zijn opgeslagen voor een webaanvraag die een queryreeks bevat. In een webaanvraag met een queryreeks bevat is de query-tekenreeks het gedeelte van de aanvraag die wordt uitgevoerd na een vraagteken (?). Een queryreeks bevat, kan een of meer sleutel-waardeparen, waarbij de veldnaam en de waarde ervan worden gescheiden door een gelijkteken (=) bevatten. Elke sleutel / waarde-paar wordt gescheiden door een en-teken (&). Bijvoorbeeld: http:\//www.contoso.com/content.mov?field1=value1 & field2 = value2. Als er meer dan één sleutel / waarde-paar in een queryreeks van een aanvraag, is de volgorde niet van belang. 

> [!IMPORTANT]
> De Azure CDN standard en premium producten dezelfde querytekenreeks cache-functionaliteit bieden, maar de gebruikersinterface is. In dit artikel beschrijft de interface voor **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Akamai** en **Azure CDN Standard van Verizon**. Voor deze query opslaan in cache met **Azure CDN Premium van Verizon**, Zie [besturingselement Azure CDN cachegedrag met queryreeksen - premium-laag](cdn-query-string-premium.md).

Drie beschikbare modi voor query-tekenreeks zijn beschikbaar:

- **Querytekenreeksen negeren**: standaardmodus. In deze modus wordt het knooppunt CDN point-of-presence (POP) geeft de queryreeksen van de aanvrager met de oorspronkelijke server op de eerste aanvraag en plaatst de asset. Alle volgende aanvragen voor de asset die worden aangeboden via de pop-server negeert de queryreeksen totdat de activa in de cache verloopt.

- **Overslaan voor querytekenreeksen**: In deze modus kan aanvragen met queryreeksen zijn niet in cache opgeslagen op het knooppunt CDN pop-locatie. Het POP-knooppunt de asset rechtstreeks vanaf de oorspronkelijke server opgehaald en doorgegeven aan de aanvrager aan elke aanvraag.

- **Elke unieke URL in de cache**: In deze modus kan elke aanvraag met een unieke URL, met inbegrip van de queryreeks wordt behandeld als een unieke activum met een eigen cache. Het antwoord op de bronserver voor een aanvraag voor example.ashx?q=test1 is bijvoorbeeld in cache bij het POP-knooppunt en voor daaropvolgende caches met dezelfde querytekenreeks geretourneerd. Een aanvraag voor example.ashx?q=test2 in cache wordt opgeslagen als een afzonderlijk actief door een eigen time-to-live-instelling.
   
    >[!IMPORTANT] 
    > Gebruik deze modus niet de queryreeks bevat de parameters die met elke aanvraag, zoals een sessie-ID of een gebruikersnaam verandert, omdat het tot een lage Cachetreffer verhouding leiden zal.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Het wijzigen van de queryreeks opslaan in cache-instellingen voor standaard CDN-profielen
1. Een CDN-profiel te openen en selecteer vervolgens het CDN-eindpunt dat u wilt beheren.
   
   ![CDN-profiel-eindpunten](./media/cdn-query-string/cdn-endpoints.png)
   
2. Klik in het linkerdeelvenster onder instellingen op **regels opslaan in cache**.
   
    ![Knop Regels voor CDN-caching](./media/cdn-query-string/cdn-caching-rules-btn.png)
   
3. In de **queryreeks cachegedrag** lijst, selecteert u een query-tekenreeks-modus en klik op **opslaan**.
   
   ![CDN-queryreeks cacheopties](./media/cdn-query-string/cdn-query-string.png)

> [!IMPORTANT]
> Omdat het duurt om de registratie te verspreiden via Azure CDN, wijzigingen in de cache tekenreeks mogelijk niet onmiddellijk zichtbaar zijn:
> - Voor profielen van **Azure CDN Standard van Microsoft** is het doorgeven gewoonlijk binnen 10 minuten voltooid. 
> - Profielen van **Azure CDN Standard van Akamai** worden doorgaans binnen één minuut doorgegeven. 
> - Voor **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** profielen, doorgeven voltooid gewoonlijk in 10 minuten. 



