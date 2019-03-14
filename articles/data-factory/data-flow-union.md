---
title: Azure Data Factory Mapping Data Flow New Branch Transformation
description: Azure Data Factory Mapping Data Flow New Branch Transformation
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: af2225d749283c7124f89d5a7cd735b2f6bfd121
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792613"
---
# <a name="mapping-data-flow-union-transformation"></a>Toewijzing van flow union transformatie

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

UNION wordt meerdere gegevensstromen combineren tot één, met de SQL Union van deze stromen als de nieuwe uitvoer van de Union-transformatie. Alles van het schema van de invoerstroom wordt gecombineerd binnen de gegevensstroom, zonder dat een join-sleutel.

U kunt de n-aantal stromen in de tabel met instellingen combineren door het pictogram '+' naast elke geconfigureerde rij, met inbegrip van zowel gegevens als stromen van bestaande transformaties in de gegevensstroom te selecteren.

![Union transformatie](media/data-flow/union.png "Union")

In dit geval kunt u verschillende metagegevens uit meerdere bronnen (in dit voorbeeld worden drie verschillende bronbestanden) combineren en deze combineren tot één stroom:

![Overzicht van UNION transformatie](media/data-flow/union111.png "Union 1")

Om dit te doen, moet u extra rijen toevoegen in de Union-instellingen door op te nemen alle bron die u wilt toevoegen. Er is niet nodig voor een gemeenschappelijke lookup- of join-sleutel:

![Union transformatie-instellingen](media/data-flow/unionsettings.png "Union-instellingen")

Als u een optie transformatie na uw Union instelt, kunt u zich om overlappende velden of velden die niet met de naam van headerless bronnen te wijzigen. Klik op 'Controleren' om te zien van de metagegevens van de combineren met 132-totaal aantal kolommen in dit voorbeeld uit drie verschillende bronnen:

![Union transformatie laatste](media/data-flow/union333.png "Union 3")

## <a name="name-and-position"></a>De naam en de positie

Als u 'samenvoeging met de naam' kiest, wordt elke waarde in de kolom verwijderen in de bijbehorende kolom van elke bron, met een nieuw metagegevensschema van samengevoegde.

Als u 'union op positie' kiest, wordt elke waarde in de kolom in de oorspronkelijke positie van elke overeenkomstige bron, wat resulteert in een nieuwe gecombineerde stream met gegevens waar de gegevens uit elke bron wordt toegevoegd aan de dezelfde stroom verwijderen:

![Union uitvoer](media/data-flow/unionoutput.png "Union uitvoer")

## <a name="next-steps"></a>Volgende stappen

Verken vergelijkbaar transformaties, waaronder [Join](data-flow-join.md) en [Exists](data-flow-exists.md).
