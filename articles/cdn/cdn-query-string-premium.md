---
title: Beheren van Azure CDN-cachinggedrag met queryreeksen - premium-laag | Microsoft Docs
description: Azure CDN van querytekenreeksen besturingselementen hoe bestanden worden opgeslagen wanneer een webaanvraag een queryreeks bevat. Dit artikel wordt beschreven in de Azure CDN Premium van Verizon product cachinggedrag van queryreeksen.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: magattus
ms.openlocfilehash: 2f0a361d53489e22ccc8e41406e5b86b423ea2f6
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091399"
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium-tier"></a>Besturingselement voor Azure CDN caching-gedrag met queryreeksen - premium-laag
> [!div class="op_single_selector"]
> * [Standaardlaag](cdn-query-string.md)
> * [Premiumlaag](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Overzicht
Met Azure Content Delivery Network (CDN), kunt u bepalen hoe bestanden in cache zijn opgeslagen voor een webaanvraag die een queryreeks bevat. In een webaanvraag met een queryreeks is de query-tekenreeks het gedeelte van de aanvraag die wordt weergegeven achter een vraagteken (?). Een query-tekenreeks kan een of meer sleutel / waarde-paren, waarin de naam van het veld en de waarde ervan worden gescheiden door een gelijkteken (=) bevatten. Elke sleutel / waarde-paar wordt gescheiden door een en-teken (&). Bijvoorbeeld: http:\//www.contoso.com/content.mov?field1=value1 & field2 = value2. Als er meer dan één sleutel / waarde-paar in een querytekenreeks van een aanvraag, is de volgorde niet van belang. 

> [!IMPORTANT]
> De standard en premium CDN-producten, bieden de dezelfde functionaliteit cachinggedrag van queryreeksen, maar de gebruikersinterface wijkt af. In dit artikel beschrijft de interface voor **Azure CDN Premium van Verizon**. Voor query-tekenreeks in cache opslaan met Azure CDN standard van producten, Zie [Control Azure CDN caching-gedrag met queryreeksen - standard-laag](cdn-query-string.md).
>


Drie modi voor query-tekenreeks zijn beschikbaar:

- **Standard-cache**: standaardmodus. In deze modus wordt het CDN point of presence (POP)-knooppunt geeft de queryreeksen van de aanvrager aan de oorspronkelijke server op de eerste aanvraag en de activa in de cache opslaat. Alle volgende aanvragen voor de activa die worden aangeleverd vanuit de POP-server worden de queryreeksen negeren totdat de activa in de cache verloopt.

    >[!IMPORTANT] 
    > Als het token autorisatie voor elk pad voor dit account is ingeschakeld, is standard-cachemodus de enige modus die kan worden gebruikt. 

- **geen cache**: In deze modus kan aanvragen met queryreeksen zijn niet in cache opgeslagen op het CDN POP-knooppunt. Het POP-knooppunt de asset rechtstreeks vanuit de oorspronkelijke server opgehaald en doorgegeven aan de aanvrager met elke aanvraag.

- **de unieke cache**: In deze modus kan elke aanvraag met een unieke URL, met inbegrip van de query-tekenreeks wordt beschouwd als een unieke asset met de eigen cache. Het antwoord van de oorspronkelijke server voor een aanvraag voor example.ashx?q=test1 is bijvoorbeeld in de cache opgeslagen met het POP-knooppunt en voor daaropvolgende caches met dezelfde querytekenreeks geretourneerd. Een aanvraag voor example.ashx?q=test2 is in de cache opgeslagen als een afzonderlijk actief met een eigen time-to-live-instelling.
   
    >[!IMPORTANT] 
    > Gebruik deze modus niet wanneer de query-tekenreeks parameters waarmee bij elke aanvraag, zoals een sessie-ID of een gebruikersnaam, bevat omdat het leidt tot een lage Cachetreffer verhouding.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Instellingen voor premium CDN-profielen van querytekenreeksen wijzigen
1. Open een CDN-profiel en klik vervolgens op **beheren**.
   
    ![Knop voor CDN-profiel beheren](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    De CDN-beheerportal wordt geopend.
2. Beweeg de muisaanwijzer over de **HTTP grote** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Cache-instellingen** flyout menu. Klik op **queryreeks Caching**.
   
    Opties voor opslaan in cache queryreeksen worden weergegeven.
   
    ![CDN-queryreeks cacheopties](./media/cdn-query-string-premium/cdn-query-string.png)
3. Selecteer een query-tekenreeks-modus en klik vervolgens op **Update**.

> [!IMPORTANT]
> Omdat het enige tijd vergt om de registratie door te geven via het CDN, wijzigingen in de cache tekenreeks mogelijk niet meteen zichtbaar. Doorgifte doorgaans binnen 10 minuten.
 

