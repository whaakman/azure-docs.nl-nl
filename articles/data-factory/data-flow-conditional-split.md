---
title: Azure Data Factory toewijzing gegevenstransformatie stroom voorwaardelijke splitsen
description: Azure Data Factory stroom voorwaardelijke Split transformatie
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: a4ea79e05165dfae4f79aa6473a07151ba7c00fc
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56727805"
---
# <a name="azure-data-factory-mapping-data-flow-conditional-split-transformation"></a>Azure Data Factory toewijzing gegevenstransformatie stroom voorwaardelijke splitsen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

De voorwaardelijke Split-transformatie kan rijen met gegevens routeren naar verschillende stromen, afhankelijk van de inhoud van de gegevens. De implementatie van de voorwaardelijke Split-transformatie is vergelijkbaar met de beslissingsstructuur van een CASE in een programmeertaal. De transformatie expressies, evalueert en op basis van de resultaten, zorgt ervoor dat de rij met gegevens naar de opgegeven stroom. Deze transformatie biedt ook een standaarduitvoer, zodat als een rij geen expressie overeenkomt met deze wordt omgeleid naar de standaarduitvoer.

![voorwaardelijke split](media/data-flow/cd1.png "voorwaardelijke splitsen")

Aanvullende voorwaarden toevoegen, selecteert u 'Stream toevoegen' in het onderste deelvenster voor configuratie en klikt u op in het tekstvak opbouwfunctie voor expressies om te maken van de expressie.
