---
title: Voorwaardelijke expressies Azure CDN-regelengine | Microsoft Docs
description: Naslagdocumentatie voor Azure CDN regels criteria voor overeenkomst regelengine en functies.
services: cdn
documentationcenter: ''
author: Lichard
manager: akucer
editor: ''
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 73c41b754c0aca5ddb1a49fcd2794aa41b2fa705
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60324194"
---
# <a name="azure-cdn-rules-engine-conditional-expressions"></a>Voorwaardelijke expressies Azure CDN-regelengine
Dit onderwerp vindt u gedetailleerde beschrijvingen van de voorwaardelijke expressies voor Azure Content Delivery Network (CDN) [regels-Engine](cdn-rules-engine.md).

Het eerste deel van een regel wordt de voorwaardelijke expressie.

Voorwaardelijke expressies | Beschrijving
-----------------------|-------------
ALS | Een expressie als is altijd een onderdeel van de eerste instructie in een regel. Net als alle andere voorwaardelijke expressies, moet deze instructie worden gekoppeld aan een overeenkomst. Als er geen extra voorwaardelijke expressies zijn gedefinieerd, bepaalt deze overeenkomst de criteria die moet worden voldaan voordat een set functies kan worden toegepast op een aanvraag.
EN ALS | Een expressie en als kan alleen worden toegevoegd na de volgende soorten voorwaardelijke expressies: als, en als. Deze geeft aan dat er nog een voorwaarde wordt voldaan voor de eerste instructie.
OF ALS| Een expressie of als Hiermee geeft u een alternatieve voorwaarde die moet worden voldaan voordat een set functies die specifiek zijn voor deze verklaring of als plaatsvindt. De aanwezigheid van een andere instructie geeft aan het einde van de vorige instructie. Het alleen voorwaardelijke expressies die kan worden geplaatst nadat een instructie of als een andere ELSE IF-instructie is. Dit betekent dat een instructie of als alleen kan worden gebruikt om op te geven van een enkele aanvullende voorwaarde die moet worden voldaan.

**Voorbeeld**: ![CDN-voorwaarde voor overeenkomst](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Een volgende regel kan de handelingen die zijn opgegeven door een vorige regel negeren. Voorbeeld: Een catch-all-regel voor beveiliging van alle aanvragen via verificatie op basis van tokens. Een andere regel kan direct hieronder om er een uitzondering voor bepaalde typen aanvragen worden gemaakt.

### <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure CDN](cdn-overview.md)
* [Regels-Engine-verwijzing](cdn-rules-engine-reference.md)
* [Criteria voor overeenkomst regelengine](cdn-rules-engine-reference-match-conditions.md)
* [Regels-Engine-functies](cdn-rules-engine-reference-features.md)
* [Standaardgedrag HTTP met behulp van de regelengine](cdn-rules-engine.md)
