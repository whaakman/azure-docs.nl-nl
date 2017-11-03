---
title: Azure CDN regels engine voorwaardelijke expressies | Microsoft Docs
description: Documentatie bij Azure CDN regels overeen motor en onderdelen.
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 57e56c38e003cb83dcf44f455c4451d159db8a59
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cdn-rules-engine-conditional-expressions"></a>Voorwaardelijke expressies in de engine Azure CDN-regels
Dit onderwerp vindt u gedetailleerde beschrijvingen van de voorwaardelijke expressies voor Azure Content Delivery Network (CDN) [regelengine](cdn-rules-engine.md).

Het eerste deel van een regel is de conditionele expressie.

Voorwaardelijke expressies | Beschrijving
-----------------------|-------------
ALS | Een expressie als is altijd een deel van de eerste instructie in een regel. Net als alle andere voorwaardelijke expressies, moet deze instructie worden gekoppeld aan een overeenkomst. Als er geen aanvullende voorwaardelijke expressies zijn opgegeven, bepaalt deze treffer het criterium dat moet worden voldaan voordat een aantal functies kan worden toegepast op een aanvraag.
EN ALS | EN als expressie kan alleen worden toegevoegd na de volgende soorten voorwaardelijke expressies: als, en als. Dit geeft aan dat er nog een voorwaarde die moet worden voldaan om de eerste instructie.
ANDERS ALS| Een expressie ELSE IF Hiermee geeft u een alternatieve voorwaarde die moet worden voldaan voordat een verzameling functies die specifiek zijn voor deze instructie ELSE IF plaatsvindt. De aanwezigheid van een ELSE IF-instructie geeft het einde van de vorige instructie. De alleen voorwaardelijke expressies die kan worden geplaatst nadat een ELSE IF-instructie een andere ELSE IF-instructie is. Dit betekent dat een ELSE IF-instructie kan alleen worden gebruikt om op te geven van een enkele aanvullende voorwaarde die moet worden voldaan.

**Voorbeeld**: ![CDN overeenkomen met de voorwaarde](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Een volgende regel mogen de handelingen die zijn opgegeven door een vorige regel overschrijven. Voorbeeld: Een regel catch all-beveiligt alle aanvragen via verificatie op basis van tokens. Een andere regel kan rechtstreeks onder om er een uitzondering voor bepaalde typen aanvragen worden gemaakt.

### <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure CDN](cdn-overview.md)
* [Regels Engine verwijzing](cdn-rules-engine-reference.md)
* [De overeenkomst motor regels](cdn-rules-engine-reference-match-conditions.md)
* [Regels Engine functies](cdn-rules-engine-reference-features.md)
* [Standaardgedrag HTTP met de regelengine van](cdn-rules-engine.md)
