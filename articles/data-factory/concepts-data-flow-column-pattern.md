---
title: Kolom patronen voor het toewijzen van gegevens stromen Azure Data Factory
description: Gegeneraliseerde gegevens transformatie patronen maken met behulp van Azure Data Factory kolom patronen in de toewijzing van gegevens stromen
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 076c3318a68a50e6bd1b4f9f2a4a4b9a034533c6
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68346571"
---
# <a name="azure-data-factory-mapping-data-flows-column-patterns"></a>Kolom patronen voor gegevens stromen van Azure data factory-toewijzing

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Verschillende Azure Data Factory gegevensstroom transformaties ondersteunen het idee van ' column patronen ', zodat u sjabloon kolommen kunt maken op basis van patronen in plaats van in code vastgelegde kolom namen. U kunt deze functie in de opbouw functie voor expressies gebruiken om patronen te definiëren die overeenkomen met kolommen voor trans formatie in plaats van exacte, specifieke veld namen te vereisen. Patronen zijn nuttig als inkomende bron velden vaak veranderen, met name in het geval van het wijzigen van kolommen in tekst bestanden of NoSQL-data bases. Dit probleem wordt ook wel ' schema drift ' genoemd.

![kolom patronen](media/data-flow/columnpattern2.png "Kolom patronen")

Kolom patronen zijn handig voor het afhandelen van zowel schema-en algemene scenario's. Het is een goed idee om de kolom naam niet volledig te kennen. U kunt overeenkomen met de kolom naam en kolom gegevens type en een expressie maken voor trans formatie waarmee deze bewerking wordt uitgevoerd voor elk veld in de gegevens stroom dat `name` overeenkomt met uw  &  `type` patronen.

Wanneer u een expressie toevoegt aan een trans formatie die patronen accepteert, kiest u kolom patroon toevoegen. Kolom patronen kunnen schema-drift kolom matching-patronen.

Bij het bouwen van sjabloon kolom patronen `$$` gebruikt u in de expressie om een verwijzing naar elk overeenkomend veld van de invoer gegevens stroom weer te geven.

Als u een van de regex-functies van Expression Builder wilt gebruiken, kunt u vervolgens $1, $2, $3 gebruiken... om te verwijzen naar de Subpatronen die overeenkomen met uw regex-expressie.

Een voor beeld van een scenario met een kolom patroon is het gebruik van SUM met een reeks inkomende velden. De cumulatieve som-berekeningen bevinden zich in de geaggregeerde trans formatie. U kunt vervolgens SUM gebruiken bij elke overeenkomst met veld typen die ' Integer ' overeenkomen en vervolgens $ $ gebruiken om te verwijzen naar elke overeenkomst in uw expressie.

## <a name="match-columns"></a>Kolommen vergelijken
![type kolom patroon](media/data-flow/pattern2.png "Patroon typen")

Als u patronen wilt bouwen op basis van kolommen, kunt u overeenkomen met de kolom naam, het type, de stroom of de positie en een combi natie hiervan gebruiken met expressie functies en reguliere expressies.

![kolom positie](media/data-flow/position.png "Kolom positie")

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [taal](https://aka.ms/dataflowexpressions) van de ADF-toewijzings gegevens voor gegevens transformaties
