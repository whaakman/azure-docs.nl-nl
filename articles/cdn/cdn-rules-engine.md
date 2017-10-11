---
title: HTTP-instelling met de engine van Azure CDN regels wijzigen | Microsoft Docs
description: "De regelengine voor kunt u aanpassen hoe HTTP-aanvragen worden verwerkt door Azure CDN, zoals de levering van bepaalde typen inhoud blokkeren, een cachebeleid definiëren en aanpassen van HTTP-headers."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 625a912b-91f2-485d-8991-128cc194ee71
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: abfe283476206b181018d187675b47112dc5ad2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="override-http-behavior-using-the-azure-cdn-rules-engine"></a>HTTP-instelling met de engine van Azure CDN regels wijzigen
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht
De regelengine voor kunt u aanpassen hoe HTTP-aanvragen worden verwerkt, zoals de levering van bepaalde typen inhoud blokkeren, een cachebeleid definiëren en HTTP-headers wijzigen.  Deze zelfstudie wordt gedemonstreerd maken van een regel waarmee het cachegedrag van CDN activa wordt gewijzigd.  Er is ook video-inhoud beschikbaar in de '[Zie ook](#see-also)' sectie.

   > [!TIP] 
   > Zie voor een verwijzing naar de syntaxis in detail, [regels Engine verwijzing](cdn-rules-engine-reference.md).
   > 


## <a name="tutorial"></a>Zelfstudie
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop blade CDN-profiel beheren](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
2. Klik op de **HTTP grote** tabblad, gevolgd door **regelengine**.
   
    Opties voor een nieuwe regel worden weergegeven.
   
    ![Opties voor nieuwe CDN](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > De volgorde waarin u meerdere regels staan is van invloed op hoe ze worden verwerkt. Een volgende regel mogen de handelingen die zijn opgegeven door een vorige regel overschrijven.
   > 
   > 
3. Voer een naam in de **naam / beschrijving** textbox.
4. Identificeer het type van de regel wordt toegepast op aanvragen.  Standaard de **altijd** overeen voorwaarde is geselecteerd.  U **altijd** voor deze zelfstudie, dus laat die geselecteerd.
   
   ![CDN overeen voorwaarde](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!TIP]
   > Er zijn veel soorten overeenkomst voorwaarden die beschikbaar zijn in de vervolgkeuzelijst.  Op de blauwe informatief pictogram aan de linkerkant van de conditie van de overeenkomst te klikken, wordt de geselecteerde voorwaarde in detail uitgelegd.
   > 
   >  Zie voor een volledige lijst met voorwaardelijke expressies in detail, [regels Engine voorwaardelijke expressies](cdn-rules-engine-reference-match-conditions.md).
   >  
   > Zie voor een volledige lijst met voorwaarden in detail overeen, [regels overeenkomen met motor](cdn-rules-engine-reference-match-conditions.md).
   > 
   > 
5. Klik op de  **+**  naast **functies** een nieuwe functie toe te voegen.  Selecteer in de vervolgkeuzelijst aan de linkerkant **Force interne-maximumleeftijd**.  Voer in het tekstvak dat wordt weergegeven, **300**.  Laat de overige standaardwaarden.
   
   ![CDN-functie](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > Als met de voorwaarden van de overeenkomst geeft te klikken op de blauwe informatief pictogram aan de linkerkant van de nieuwe functie informatie over deze functie.  In het geval van **Force interne-maximumleeftijd**, wordt bij het overschrijven van de asset **Cache-Control** en **verloopt** headers om te bepalen wanneer de activa van wordt vernieuwd in het CDN edge-knooppunt de oorsprong.  Het voorbeeld van 300 seconden betekent dat het edge-knooppunt van het CDN cache worden opgeslagen door de asset gedurende vijf minuten voordat u de asset van de oorspronkelijke vernieuwt.
   > 
   > Zie voor een volledige lijst met functies in detail [regels Engine functie Details](cdn-rules-engine-reference-features.md).
   > 
   > 
6. Klik op de **toevoegen** om op te slaan van de nieuwe regel.  De nieuwe regel wordt nu in afwachting van goedkeuring. Zodra deze is goedgekeurd, de status verandert van **in behandeling XML** naar **Active XML**.
   
   > [!IMPORTANT]
   > Regels wijzigingen kunnen worden doorgegeven via de CDN tot 90 minuten duren.
   > 
   > 

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure CDN](cdn-overview.md)
* [Regels Engine verwijzing](cdn-rules-engine-reference.md)
* [De overeenkomst motor regels](cdn-rules-engine-reference-match-conditions.md)
* [Regels Engine voorwaardelijke expressies](cdn-rules-engine-reference-conditional-expressions.md)
* [Regels Engine functies](cdn-rules-engine-reference-features.md)
* [Standaardgedrag HTTP met de regelengine van](cdn-rules-engine.md)
* [Azure vrijdag: Azure CDN krachtige nieuwe Premiumfuncties](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)