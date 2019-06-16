---
title: Azure Data Factory gegevenspatronen stroom kolom toewijzen
description: Informatie over het gebruik van Azure Data Factory kolom patronen in de gegevensstroom toewijzen om gegeneraliseerde sjabloon patronen voor het transformeren van velden in een gegevensstroom ongeacht de metagegevens van het onderliggende schema te maken
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 08cdaafe00b7dc586ea75f6ff03fdb89107edee9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66430750"
---
# <a name="azure-data-factory-mapping-data-flows-column-patterns"></a>Azure data factory toewijzingsgegevens stromen kolom patronen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Verschillende transformaties gegevensstroom van Azure Data Factory ondersteunt het idee van "Kolommen patronen" zodat u kunt sjabloon kolommen op basis van patronen in plaats van de namen van vastgelegde kolommen maken. U kunt deze functie binnen de opbouwfunctie voor expressies gebruiken voor het definiëren van patronen zodat deze overeenkomen met kolommen voor transformatie in plaats van exacte, specifieke veldnamen vereisen. Patronen zijn handig als binnenkomende bronvelden vaak wijzigen, met name voor het wijzigen van kolommen in de tekstbestanden of NoSQL-databases. Deze voorwaarde wordt ook wel 'Schema Drift' genoemd.

![kolom patronen](media/data-flow/columnpattern2.png "kolom patronen")

Kolom patronen zijn handig voor het afhandelen van zowel afwijking van Schema's, evenals algemene scenario's. Het is goed voor voorwaarden waar u bent geen volledig weet de naam van elke kolom. U kunt patroon overeenkomen met de naam van kolom en kolomgegevens typt en bouwt u een expressie voor transformatie die door deze bewerking voor elk veld worden uitgevoerd in de gegevensstroom die overeenkomt met uw `name`  &  `type` patronen.

Als u een expressie toevoegt aan een transformatie die patronen accepteert, moet u 'Toevoegen kolom patroon' kiezen. Kolom patronen kunt schema drift kolom overeenkomende patronen.

Bij het bouwen van sjabloon kolom patronen, gebruikt u `$$` in de expressie voor een verwijzing naar elke overeenkomende veld van de stroom inkomende gegevens.

Als u ervoor kiest een van de functies van de reguliere expressie opbouwfunctie voor expressies te gebruiken, kunt u vervolgens later gebruiken $1, $2, $3... komt overeen met van de regex-expressie om te verwijzen naar de onderliggende patronen.

Een voorbeeld van kolom patroon scenario wordt gebruik van een som met een reeks binnenkomende velden. De totale som-berekeningen zijn in het aggregaat transformatie. U kunt vervolgens som gebruiken op elke overeenkomst van veldtypen die overeenkomen met 'geheel getal zijn' en $ vervolgens gebruiken om te verwijzen naar elke overeenkomst in de expressie.
