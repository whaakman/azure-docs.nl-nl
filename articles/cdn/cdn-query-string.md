---
title: Beheren van Azure CDN-cachinggedrag met queryreeksen - standard-laag | Microsoft Docs
description: Azure CDN van querytekenreeksen besturingselementen hoe bestanden worden opgeslagen wanneer een webaanvraag een queryreeks bevat. Dit artikel wordt beschreven in de Azure CDN standard producten van querytekenreeksen.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: magattus
ms.openlocfilehash: f0dab3dc81c626e3e7f8c79b4142e5eb4f2a1276
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093796"
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---standard-tier"></a>Besturingselement voor Azure CDN caching-gedrag met queryreeksen - standard-laag
> [!div class="op_single_selector"]
> * [Standaardlaag](cdn-query-string.md)
> * [Premiumlaag](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Overzicht
Met Azure Content Delivery Network (CDN), kunt u bepalen hoe bestanden in cache zijn opgeslagen voor een webaanvraag die een queryreeks bevat. In een webaanvraag met een queryreeks is de query-tekenreeks het gedeelte van de aanvraag die wordt weergegeven achter een vraagteken (?). Een query-tekenreeks kan een of meer sleutel / waarde-paren, waarin de naam van het veld en de waarde ervan worden gescheiden door een gelijkteken (=) bevatten. Elke sleutel / waarde-paar wordt gescheiden door een en-teken (&). Bijvoorbeeld: http:\//www.contoso.com/content.mov?field1=value1 & field2 = value2. Als er meer dan één sleutel / waarde-paar in een querytekenreeks van een aanvraag, is de volgorde niet van belang. 

> [!IMPORTANT]
> De Azure CDN standard en premium-producten bieden de dezelfde functionaliteit cachinggedrag van queryreeksen, maar de gebruikersinterface wijkt af. In dit artikel beschrijft de interface voor **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Akamai** en **Azure CDN Standard van Verizon**. Voor de query queryreeksen opslaan in cache met **Azure CDN Premium van Verizon**, Zie [Control Azure CDN caching-gedrag met queryreeksen - premium-laag](cdn-query-string-premium.md).

Drie modi voor query-tekenreeks zijn beschikbaar:

- **Queryreeksen negeren**: standaardmodus. In deze modus wordt het CDN point of presence (POP)-knooppunt geeft de queryreeksen van de aanvrager aan de oorspronkelijke server op de eerste aanvraag en de activa in de cache opslaat. Alle volgende aanvragen voor de activa die worden aangeleverd vanuit de pop-server worden de queryreeksen negeren totdat de activa in de cache verloopt.

- **Queryreeksen in cache opslaan overslaan**: In deze modus kan aanvragen met queryreeksen zijn niet in cache opgeslagen op het CDN POP-knooppunt. Het POP-knooppunt de asset rechtstreeks vanuit de oorspronkelijke server opgehaald en doorgegeven aan de aanvrager met elke aanvraag.

- **Elke unieke URL in de cache**: In deze modus kan elke aanvraag met een unieke URL, met inbegrip van de query-tekenreeks wordt beschouwd als een unieke asset met de eigen cache. Het antwoord van de oorspronkelijke server voor een aanvraag voor example.ashx?q=test1 is bijvoorbeeld in de cache opgeslagen met het POP-knooppunt en voor daaropvolgende caches met dezelfde querytekenreeks geretourneerd. Een aanvraag voor example.ashx?q=test2 is in de cache opgeslagen als een afzonderlijk actief met een eigen time-to-live-instelling.
   
    >[!IMPORTANT] 
    > Gebruik deze modus niet wanneer de query-tekenreeks parameters waarmee bij elke aanvraag, zoals een sessie-ID of een gebruikersnaam, bevat omdat het leidt tot een lage Cachetreffer verhouding.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Instellingen voor standard CDN-profielen van querytekenreeksen wijzigen
1. Open een CDN-profiel, en selecteer vervolgens het CDN-eindpunt dat u wilt beheren.
   
   ![CDN-profiel-eindpunten](./media/cdn-query-string/cdn-endpoints.png)
   
2. Klik in het linkerdeelvenster onder instellingen op **regels voor Caching**.
   
    ![Knop Regels voor CDN-caching](./media/cdn-query-string/cdn-caching-rules-btn.png)
   
3. In de **queryreeks cachegedrag** lijst, selecteert u een query-tekenreeks-modus en klik op **opslaan**.
   
   ![CDN-queryreeks cacheopties](./media/cdn-query-string/cdn-query-string.png)

> [!IMPORTANT]
> Omdat het enige tijd vergt om de registratie door te geven tot en met Azure CDN, wijzigingen in de cache tekenreeks mogelijk niet meteen zichtbaar:
> - Voor profielen van **Azure CDN Standard van Microsoft** is het doorgeven gewoonlijk binnen 10 minuten voltooid. 
> - Profielen van **Azure CDN Standard van Akamai** worden doorgaans binnen één minuut doorgegeven. 
> - Profielen van **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** worden normaal gesproken binnen 10 minuten doorgegeven. 



