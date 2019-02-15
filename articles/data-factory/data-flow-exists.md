---
title: Azure Data Factory gegevensstroom toewijzing bestaat transformatie
description: Azure Data Factory gegevensstroom toewijzing bestaat transformatie
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 9d21b304f55ec746da4b7b42194fe0d168261b53
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271861"
---
# <a name="azure-data-factory-mapping-data-flow-exists-transformation"></a>Azure Data Factory gegevensstroom toewijzing bestaat transformatie

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

De transformatie Exists is een rij transformatie die service is gestopt of rijen in uw gegevens door kunnen stromen kunt filteren. De bestaat transformatie is vergelijkbaar met ```SQL WHERE EXISTS``` en ```SQL WHERE NOT EXISTS```. Na een transformatie Filter de resulterende rijen uit de gegevensstroom ofwel bevat alle rijen waarin kolomwaarden van bron 1 aanwezig zijn in de bron 2 of bestaan niet in de bron 2.

![Instellingen bestaat](media/data-flow/exsits.png "1 bestaat")

Kies de tweede bron voor uw Exists zodat gegevensstroom, kunt u de waarden van Stream 1 op basis van Stream 2 vergelijken.

Selecteer de kolom van bron 1 en 2 van de bron waarvan u wilt controleren op bestaat of bestaan niet op basis van de waarden.
