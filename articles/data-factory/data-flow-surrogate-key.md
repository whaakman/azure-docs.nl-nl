---
title: Azure Data Factory stroom vervangend sleutels transformatie toewijzen
description: Azure Data Factory stroom vervangend sleutels transformatie toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 62e9879f6bc75f042669ecb931ca7459d1317cc7
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271849"
---
# <a name="azure-data-factory-mapping-data-flow-surrogate-key-transformation"></a>Azure Data Factory stroom vervangend sleutels transformatie toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik de transformatie van de sleutel Surrogate een willekeurige sleutel waarde voor ophogende niet-zakelijke toevoegen aan uw stroom gegevensrijenset. Dit is handig bij het ontwerpen van dimensietabellen in een stervormig schema analytische gegevensmodel waar elk lid in de dimensietabellen moet een unieke sleutel die een niet-zakelijke sleutel, deel uitmaakt van de Kimball-DW-methode.

![Vervangend sleutels transformatie](media/data-flow/surrogate.png "vervangend sleutels transformatie")

'Sleutelkolom' is de naam die u u aan uw nieuwe surrogate-sleutelkolom krijgt.

'Waarde start' is het beginpunt van de incrementele waarde.
