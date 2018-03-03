---
title: HTTP-instelling met de engine van Azure CDN regels wijzigen | Microsoft Docs
description: "De regelengine voor kunt u aanpassen hoe HTTP-aanvragen worden verwerkt door Azure CDN, zoals de levering van bepaalde typen inhoud blokkeren, een cachebeleid definiëren en aanpassen van HTTP-headers."
services: cdn
documentationcenter: 
author: dksimpson
manager: akucer
editor: 
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: mazha
ms.openlocfilehash: fe3df703f7eb244a52756c4d015e9ea598224ce1
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="override-http-behavior-using-the-azure-cdn-rules-engine"></a>HTTP-instelling met de engine van Azure CDN regels wijzigen
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht
De engine van Azure CDN regels kunt u aanpassen hoe HTTP-aanvragen worden verwerkt. Bijvoorbeeld, blokkeert de levering van bepaalde typen inhoud, het definiëren van een beleid of het wijzigen van een HTTP-header. Deze zelfstudie laat zien hoe een regel maken waarmee het cachegedrag van CDN activa wordt gewijzigd. Zie voor meer informatie over de syntaxis voor regels engine [Azure CDN regels engine verwijzing](cdn-rules-engine-reference.md).

## <a name="access"></a>Access
Voor toegang tot de regelengine voor, moet u eerst selecteren **beheren** vanaf de bovenkant van de **CDN-profiel** pagina voor toegang tot de pagina Azure CDN-beheer. Afhankelijk van of uw eindpunt is geoptimaliseerd voor dynamische siteregistratie acceleration (DSA), moet u dan de regelengine met de set regels die geschikt is voor het type van het eindpunt toegang:

- Eindpunten die zijn geoptimaliseerd voor de levering van algemene webtoepassingen of andere niet-DSA-optimalisatie: 
    
    Selecteer de **HTTP grote** tabblad en selecteer vervolgens **regelengine**.

    ![Regels voor HTTP-engine](./media/cdn-rules-engine/cdn-http-rules-engine.png)

- Eindpunten die zijn geoptimaliseerd voor DSA: 
    
    Selecteer de **ADN** tabblad en selecteer vervolgens **regelengine**. 
    
    ADN is een term die wordt gebruikt door Verizon DSA-inhoud opgeven. Alle regels die u hier maakt, worden genegeerd door geen eindpunten die niet zijn geoptimaliseerd voor DSA in uw profiel. 

    ![Regels voor DSA-engine](./media/cdn-rules-engine/cdn-dsa-rules-engine.png)

## <a name="tutorial"></a>Zelfstudie
1. Van de **CDN-profiel** pagina **beheren**.
   
    ![Knop CDN-profiel beheren](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
2. Selecteer de **HTTP grote** tabblad en selecteer vervolgens **regelengine**.
   
    De opties voor een nieuwe regel worden weergegeven.
   
    ![Opties voor nieuwe CDN](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > De volgorde waarin u meerdere regels staan is van invloed op hoe ze worden verwerkt. Een volgende regel mogen de handelingen die zijn opgegeven door een vorige regel overschrijven.
   > 
3. Voer een naam in de **naam / beschrijving** textbox.
4. Identificeer het type van de regel geldt voor aanvragen. De voorwaarde van de overeenkomst standaard gebruiken **altijd**. 
   
   ![CDN-regelvoorwaarde overeen](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!NOTE]
   > Meerdere overeenkomst voorwaarden zijn beschikbaar in de vervolgkeuzelijst. Voor informatie over de geselecteerde overeenkomst voorwaarde, selecteer het blauw informatief pictogram aan de linkerkant.
   > 
   >  Zie voor een gedetailleerde lijst met voorwaardelijke expressies, [regels engine voorwaardelijke expressies](cdn-rules-engine-reference-match-conditions.md).
   >  
   > Zie voor een gedetailleerde lijst met voorwaarden van de overeenkomst, [regels overeen motor](cdn-rules-engine-reference-match-conditions.md).
   > 
   > 
1. Selecteer om een nieuwe functie toe de  **+**  naast **functies**.  Selecteer in de vervolgkeuzelijst aan de linkerkant **Force interne-maximumleeftijd**.  Voer in het tekstvak dat wordt weergegeven, **300**. De overige standaardwaarden niet gewijzigd.
   
   ![CDN regel functie](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > Meerdere functies zijn beschikbaar in de vervolgkeuzelijst. Voor informatie over de geselecteerde functie, selecteer het blauw informatief pictogram aan de linkerkant. 
   >
   > Voor **Force interne-maximumleeftijd**, de asset `Cache-Control` en `Expires` headers worden overschreven om te bepalen wanneer de activa van de oorsprong in het CDN edge-knooppunt wordt vernieuwd. In dit voorbeeld plaatst het CDN edge-knooppunt de asset 300 seconden, of 5 minuten, voordat het de asset vanuit de oorsprong wordt vernieuwd.
   > 
   > Zie voor een gedetailleerde lijst met functies [regels engine functies](cdn-rules-engine-reference-features.md).
   > 
   > 
1. Klik op de **toevoegen** om op te slaan van de nieuwe regel.  De nieuwe regel wordt nu in afwachting van goedkeuring. Nadat deze is goedgekeurd, wordt de status verandert van **in behandeling XML** naar **Active XML**.
   
   > [!IMPORTANT]
   > Regels kan worden doorgegeven via de CDN tot 90 minuten duren.
   > 
   > 

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure CDN](cdn-overview.md)
* [Regels engine verwijzing](cdn-rules-engine-reference.md)
* [De overeenkomst motor regels](cdn-rules-engine-reference-match-conditions.md)
* [Regels engine voorwaardelijke expressies](cdn-rules-engine-reference-conditional-expressions.md)
* [Regels engine functies](cdn-rules-engine-reference-features.md)
* [Azure vrijdag: Azure CDN krachtige nieuwe Premiumfuncties](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)