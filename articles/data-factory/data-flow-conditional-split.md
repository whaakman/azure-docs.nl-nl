---
title: Azure Data Factory toewijzing gegevenstransformatie stroom voorwaardelijke splitsen
description: Azure Data Factory stroom voorwaardelijke Split transformatie
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: f9fd346d4c4eaed0797d564fe52dd44e9f0e240a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65795643"
---
# <a name="mapping-data-flow-conditional-split-transformation"></a>Toewijzing van de gegevensstroom voorwaardelijke transformatie splitsen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![voorwaardelijke splitsen werkset](media/data-flow/conditionalsplit2.png "voorwaardelijke werkset splitsen")

De voorwaardelijke Split-transformatie kan rijen met gegevens routeren naar verschillende stromen, afhankelijk van de inhoud van de gegevens. De implementatie van de voorwaardelijke Split-transformatie is vergelijkbaar met de beslissingsstructuur van een CASE in een programmeertaal. De transformatie expressies, evalueert en op basis van de resultaten, zorgt ervoor dat de rij met gegevens naar de opgegeven stroom. Deze transformatie biedt ook een standaarduitvoer, zodat als een rij geen expressie overeenkomt met deze wordt omgeleid naar de standaarduitvoer.

![voorwaardelijke split](media/data-flow/conditionalsplit1.png "voorwaardelijke opties voor splitsen")

## <a name="multiple-paths"></a>Meerdere paden

Aanvullende voorwaarden toevoegen, selecteert u 'Stream toevoegen' in het onderste deelvenster voor configuratie en klikt u op in het tekstvak opbouwfunctie voor expressies om te maken van de expressie.

![voorwaardelijke splitsen multi](media/data-flow/conditionalsplit3.png "voorwaardelijke multi splitsen")

## <a name="next-steps"></a>Volgende stappen

Algemene stroom gegevenstransformaties gebruikt met voorwaardelijke splitsen: [Deelnemen aan de transformatie](data-flow-join.md), [Loopup transformatie](data-flow-lookup.md), [transformatie selecteren](data-flow-select.md)
