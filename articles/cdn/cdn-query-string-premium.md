---
title: Azure CDN cachegedrag met queryreeksen - premium-laag controleren | Microsoft Docs
description: Azure CDN-queryreeks opslaan in cache bepaalt hoe bestanden worden opgeslagen wanneer een webaanvraag een queryreeks bevat. Dit artikel wordt beschreven queryreeks opslaan in cache in de Azure CDN Premium van Verizon product.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: mazha
ms.openlocfilehash: 9d92602ef5071579e0c741dd24a4e3e9f7b2c747
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium-tier"></a>Besturingselement Azure CDN cachegedrag met queryreeksen - premium-laag
> [!div class="op_single_selector"]
> * [Standaardlaag](cdn-query-string.md)
> * [Premiumlaag](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a>Overzicht
Met deze query opslaan in cache, bepaalt Azure Content Delivery Network (CDN) hoe bestanden in cache zijn opgeslagen voor een webaanvraag die een queryreeks bevat. In een webaanvraag met een queryreeks bevat is de query-tekenreeks het gedeelte van de aanvraag die wordt uitgevoerd na een vraagteken (?). Een queryreeks bevat, kan een of meer sleutel-waardeparen, waarbij de veldnaam en de waarde ervan worden gescheiden door een gelijkteken (=) bevatten. Elke sleutel / waarde-paar wordt gescheiden door een en-teken (&). Bijvoorbeeld: http:\//www.contoso.com/content.mov?field1=value1 & field2 = value2. Als er meer dan één sleutel / waarde-paar in een queryreeks van een aanvraag, is de volgorde niet van belang. 

> [!NOTE]
> De Azure CDN standard en premium producten dezelfde querytekenreeks cache-functionaliteit bieden, maar de gebruikersinterface is.  In dit artikel beschrijft de interface voor **Azure CDN Premium van Verizon**. Voor deze query opslaan in cache met **Azure CDN Standard van Akamai** en **Azure CDN Standard van Verizon**, Zie [besturingselement Azure CDN cachegedrag met queryreeksen - standaardcategorie](cdn-query-string.md).
>


Drie beschikbare modi voor query-tekenreeks zijn beschikbaar:

- **Standard-cache**: standaardmodus. In deze modus wordt het knooppunt CDN point-of-presence (POP) geeft de queryreeksen van de aanvrager met de oorspronkelijke server op de eerste aanvraag en plaatst de asset. Alle volgende aanvragen voor de asset die worden aangeboden via de POP-server negeert de queryreeksen totdat de activa in de cache verloopt.

    >[!IMPORTANT] 
    > Als het token autorisatie voor elk willekeurig pad voor dit account is ingeschakeld, is standaard cachemodus alleen de modus die kan worden gebruikt. 

- **Er is geen cache**: In deze modus kan aanvragen met queryreeksen zijn niet in cache opgeslagen op het knooppunt CDN pop-locatie. Het POP-knooppunt de asset rechtstreeks vanaf de oorspronkelijke server opgehaald en doorgegeven aan de aanvrager aan elke aanvraag.

- **unieke cache**: In deze modus kan elke aanvraag met een unieke URL, met inbegrip van de queryreeks wordt behandeld als een unieke activum met een eigen cache. Het antwoord op de bronserver voor een aanvraag voor example.ashx?q=test1 is bijvoorbeeld in cache bij het POP-knooppunt en voor daaropvolgende caches met dezelfde querytekenreeks geretourneerd. Een aanvraag voor example.ashx?q=test2 in cache wordt opgeslagen als een afzonderlijk actief door een eigen time-to-live-instelling.
   
    >[!IMPORTANT] 
    > Gebruik deze modus niet de queryreeks bevat de parameters die met elke aanvraag, zoals een sessie-ID of een gebruikersnaam verandert, omdat het tot een lage Cachetreffer verhouding leiden zal.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Het wijzigen van de queryreeks opslaan in cache-instellingen voor premium-CDN-profielen
1. Een CDN-profiel te openen en klik vervolgens op **beheren**.
   
    ![Knop voor CDN-profiel beheren](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
2. Beweeg de muisaanwijzer over de **HTTP grote** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Cache-instellingen** doel. Klik op **queryreeks Caching**.
   
    Queryreeks cache-opties worden weergegeven.
   
    ![CDN-queryreeks cacheopties](./media/cdn-query-string-premium/cdn-query-string.png)
3. Selecteer een modus voor query-tekenreeks en klik vervolgens op **Update**.

> [!IMPORTANT]
> Omdat het duurt om de registratie te geven in CDN, wijzigingen in de cache tekenreeks mogelijk niet meteen zichtbaar. Voor **Azure CDN Premium van Verizon** profielen, doorgeven voltooit meestal binnen 90 minuten.
 

