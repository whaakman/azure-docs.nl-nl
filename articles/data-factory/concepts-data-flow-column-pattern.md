---
title: Azure Data Factory gegevenspatronen stroom kolom toewijzen
description: Azure Data Factory toewijzing gegevens kolom patronen van verkeersstromen worden gebruikt om de patronen gegeneraliseerde sjabloon voor het transformeren van velden in een gegevensstroom ongeacht de metagegevens van het onderliggende schema te maken
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 53d3300ea11a86c34909ba6ce0fd6c8c0c38b4b5
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56726938"
---
# <a name="azure-data-factory-mapping-data-flow-concepts"></a>Azure Data Factory gegevensstroom concepten toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Verschillende transformaties gegevensstroom van Azure Data Factory ondersteunt het idee van "Kolommen patronen" zodat u kunt sjabloon kolommen op basis van patronen in plaats van de namen van vastgelegde kolommen maken. U kunt deze functie binnen de opbouwfunctie voor expressies gebruiken voor het definiëren van patronen zodat deze overeenkomen met kolommen voor transformatie in plaats van dat xact, specifieke veldnamen. Patronen zijn nuttig wanneer binnenkomende bronvelden vaak wijzigen, met name voor het wijzigen van kolommen in de tekstbestanden of NoSQL-databases. Dit wordt soms 'Schema Drift' genoemd.

![kolom patronen](media/data-flow/columnpattern2.png "kolom patronen")

Kolom patronen zijn handig voor het afhandelen van zowel afwijking van Schema's, evenals algemene scenario's. Het is goed voor voorwaarden waar u bent geen volledig weet de naam van elke kolom. U kunt patroon overeenkomen met de naam van kolom en kolomgegevens typt en bouwt u een expressie voor transformatie die door deze bewerking voor elk veld worden uitgevoerd in de gegevensstroom die overeenkomt met uw `name`  &  `type` patronen.

Als u een expressie toevoegt aan een transformatie die patronen accepteert, moet u 'Toevoegen kolom patroon' kiezen. Kolom patronen kunt schema drift kolom overeenkomende patronen.

Bij het bouwen van sjabloon kolom patronen, gebruikt u `$$` in de expressie voor een verwijzing naar elke overeenkomende veld van de stroom inkomende gegevens.

Als u ervoor kiest een van de functies van de reguliere expressie opbouwfunctie voor expressies te gebruiken, kunt u vervolgens later gebruiken $1, $2, $3... om te verwijzen naar de afbakenen om overeenkomende uit de regex-expressie toe.

Een voorbeeld van kolom patroon scenario wordt gebruik van een som met een reeks binnenkomende velden. De totale som-berekeningen zijn in het aggregaat transformatie. U kunt vervolgens OPTELLEN op elke overeenkomst van veldtypen dat overeen "getal" en vervolgens met $ om te verwijzen naar elke overeenkomst in de expressie.
