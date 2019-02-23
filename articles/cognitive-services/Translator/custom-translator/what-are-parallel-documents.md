---
title: Wat zijn parallelle documenten? -Aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Parallelle documenten bestaan uit paren van documenten is, de vertaling van de andere. Een document in het paar zinnen in de source-taal bevat en het andere document bevat deze zinnen vertaald naar de doel-taal.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.date: 02/21/2019
ms.author: v-rada
ms.topic: conceptual
ms.openlocfilehash: fc3e545db0529739659c1019cbed6d434b586f8f
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56726955"
---
# <a name="what-are-parallel-documents"></a>Wat zijn parallelle documenten?

Parallelle documenten bestaan uit paren van documenten is, de vertaling van de andere. Een document in het paar zinnen in de source-taal bevat en het andere document bevat deze zinnen vertaald naar de doel-taal.
Het maakt niet uit welke taal is gemarkeerd als 'bron' en welke taal is gemarkeerd als 'target': een parallelle document kan worden gebruikt voor het trainen van een vertaalsysteem in beide richtingen.

## <a name="requirements"></a>Vereisten

U moet minimaal 10.000 parallelle zinnen met het trainen van een systeem. Als een best practice, kunt u continu meer parallelle inhoud toevoegen en retrain, voor het verbeteren van de kwaliteit van uw vertaalsysteem.

Microsoft vereist dat documenten die zijn geüpload naar de Translator aangepaste eigenschappen voor het auteursrecht of intellectueel eigendom van een derde partij niet schenden. Zie voor meer informatie de [gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/cognitive-services-terms/).
Uploaden van een document met behulp van de portal, wordt het eigendom van het intellectueel eigendom in het document zelf niet gewijzigd.

## <a name="use-of-parallel-documents"></a>Gebruik van parallelle documenten

Parallelle documenten worden gebruikt door het systeem:

1.  Voor meer informatie over hoe woorden, zinnen en zinnen vaak worden toegewezen tussen de twee talen.

2.  Voor informatie over het verwerken van de juiste context, afhankelijk van de omringende zinnen. Een woord mogelijk niet altijd vertalen naar de exacte hetzelfde woord in de andere taal.

Als een best practice, zorg ervoor dat er een zin 1:1-correspondentie tussen de bron en doel taalversies van de documenten.

Als uw project domein (categorie), moeten uw documenten in de terminologie binnen die categorie consistent zijn. De kwaliteit van de resulterende vertaalsysteem, is afhankelijk van het aantal zinnen in uw document instellen en de kwaliteit van de zinnen. Meer voorbeelden uw documenten met diverse toepassingen voor een specifiek zijn voor de categorie woord bevatten, de betere taak het systeem kunt doen tijdens de conversie.

Documenten uploaden zijn exclusief zijn voor elke werkruimte en kunnen worden gebruikt in zo veel projecten of u kunt als u wilt. Zinnen geëxtraheerd uit uw documenten afzonderlijk worden opgeslagen in uw opslagplaats als gewone Unicode-tekstbestanden en zijn beschikbaar voor u verwijderen. Gebruik de aangepaste Translator niet als een document-opslagplaats, kunt u zich niet kunnen downloaden van de documenten die u hebt geüpload in de indeling u deze hebt geüpload.



## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik van een [woordenlijst](what-is-dictionary.md) in aangepaste Translator.
