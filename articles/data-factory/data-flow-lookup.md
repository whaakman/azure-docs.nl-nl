---
title: Azure Data Factory gegevenstransformatie stroom Lookup toewijzen
description: Azure Data Factory gegevenstransformatie stroom Lookup toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 28c8f2b0005641934f7fcd9549f1cf004b206d80
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271723"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Azure Data Factory gegevenstransformatie stroom Lookup toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik Lookup om toe te voegen referentiegegevens vanaf een andere bron voor uw gegevensstroom. De Lookup-transformatie is een gedefinieerde bron die verwijst naar de verwijzingstabel en komt overeen met op velden voor sleutels vereist.

![Lookup Transformation](media/data-flow/lookup1.png "Lookup")

Selecteer de velden voor sleutels die u wilt tussen de velden van de referentie-bron en de binnenkomende stream velden overeenkomen. U moet eerst hebt gemaakt een nieuwe bron op het canvas ontwerp gegevensstroom gebruiken als de rechterkant voor de zoekopdracht.

Wanneer overeenkomsten worden gevonden, wordt de resulterende rijen en kolommen uit de Verwijzingsbron worden toegevoegd aan de gegevensstroom. U kunt kiezen welke velden van belang dat u wilt opnemen in uw Sink aan het einde van de gegevensstroom.
