---
title: Azure Data Factory toewijzing gegevens Flow bestaat transformatie
description: Hoe om te controleren op bestaande rijen met behulp van data factory-toewijzingsgegevens stromen met een transformatie Exists
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: b98b7afb21f2f50d44ba93ed793b6efb20f75164
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65235981"
---
# <a name="mapping-data-flow-exists-transformation"></a>Transformatie bestaat toewijzing van de gegevensstroom

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

De transformatie Exists is een rij transformatie die service is gestopt of rijen in uw gegevens door kunnen stromen kunt filteren. De bestaat transformatie is vergelijkbaar met ```SQL WHERE EXISTS``` en ```SQL WHERE NOT EXISTS```. Na de transformatie bestaat de resulterende rijen uit de gegevensstroom ofwel bevat alle rijen waarin kolomwaarden van bron 1 aanwezig zijn in de bron 2 of bestaan niet in de bron 2.

![Instellingen bestaat](media/data-flow/exists.png "1 bestaat")

Kies de tweede bron voor uw Exists zodat gegevensstroom, kunt u de waarden van Stream 1 op basis van Stream 2 vergelijken.

Selecteer de kolom van bron 1 en 2 van de bron waarvan u wilt controleren op bestaat of bestaan niet op basis van de waarden.

## <a name="multiple-exists-conditions"></a>Meerdere bestaat voorwaarden

Naast elke rij in uw voorwaarden kolom voor Exists, vindt u een + aanmelding beschikbaar wanneer u de muisaanwijzer op rij bereikt. Hierdoor kunt u meerdere rijen voor Exists voorwaarden toevoegen. Elke extra voorwaarde is een 'En'.

## <a name="custom-expression"></a>Aangepaste expressie

![Aangepaste instellingen bestaat](media/data-flow/exists1.png "aangepaste bestaat")

U kunt klikken op 'Aangepaste expressie' als u wilt maken in plaats daarvan een vrije-expressie als uw bestaat of voorwaarde niet bestaat. Dit selectievakje kunt u op type in uw eigen expressie als een voorwaarde.

## <a name="next-steps"></a>Volgende stappen

Vergelijkbare transformaties zijn [Lookup](data-flow-lookup.md) en [Join](data-flow-join.md).
