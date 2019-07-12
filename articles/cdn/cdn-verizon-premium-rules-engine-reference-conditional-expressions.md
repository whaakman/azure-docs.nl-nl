---
title: Azure CDN van Verizon Premium regels voor voorwaardelijke expressies regelengine | Microsoft Docs
description: Naslagdocumentatie voor Azure CDN van Verizon Premium regels criteria voor overeenkomst regelengine en functies.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: f790e37ae876c0640d55ebfb51abb43c6a705f04
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593228"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-conditional-expressions"></a>Azure CDN van Verizon Premium regels voorwaardelijke expressies regelengine

Dit artikel vindt u gedetailleerde beschrijvingen van de voorwaardelijke expressies voor Azure Content Delivery Network (CDN) [regels-Engine](cdn-verizon-premium-rules-engine.md).

Het eerste deel van een regel wordt de voorwaardelijke expressie.

Voorwaardelijke expressies | Description
-----------------------|-------------
ALS | Een expressie als is altijd een onderdeel van de eerste instructie in een regel. Net als alle andere voorwaardelijke expressies, moet deze instructie worden gekoppeld aan een overeenkomst. Als er geen extra voorwaardelijke expressies zijn gedefinieerd, bepaalt deze overeenkomst de criteria die moet worden voldaan voordat een set functies kan worden toegepast op een aanvraag.
EN ALS | Een expressie en als kan alleen worden toegevoegd na de volgende soorten voorwaardelijke expressies: als, en als. Deze geeft aan dat er nog een voorwaarde wordt voldaan voor de eerste instructie.
OF ALS| Een expressie of als Hiermee geeft u een alternatieve voorwaarde die moet worden voldaan voordat een set functies die specifiek zijn voor deze verklaring of als plaatsvindt. De aanwezigheid van een andere instructie geeft aan het einde van de vorige instructie. Het alleen voorwaardelijke expressies die kan worden geplaatst nadat een instructie of als een andere ELSE IF-instructie is. Dit betekent dat een instructie of als alleen kan worden gebruikt om op te geven van een enkele aanvullende voorwaarde die moet worden voldaan.

**Voorbeeld**: ![CDN-voorwaarde voor overeenkomst](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Een volgende regel kan de handelingen die zijn opgegeven door een vorige regel negeren.
   > Voorbeeld: Een catch-all-regel voor beveiliging van alle aanvragen via verificatie op basis van tokens. Een andere regel kan direct hieronder om er een uitzondering voor bepaalde typen aanvragen worden gemaakt.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure CDN](cdn-overview.md)
- [Verwijzing regelengine](cdn-verizon-premium-rules-engine-reference.md)
- [Criteria voor overeenkomst regelengine](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Functies regelengine](cdn-verizon-premium-rules-engine-reference-features.md)
- [Standaardgedrag HTTP met behulp van de regelengine](cdn-verizon-premium-rules-engine.md)