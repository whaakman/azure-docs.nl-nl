---
title: Azure Data Factory gegevenstransformatie stroom Lookup toewijzen
description: Azure Data Factory gegevenstransformatie stroom Lookup toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: b156917a9987b023a9bf94e51c0cc14aebb133c7
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56738382"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Azure Data Factory gegevenstransformatie stroom Lookup toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik Lookup om toe te voegen referentiegegevens vanaf een andere bron voor uw gegevensstroom. De Lookup-transformatie is een gedefinieerde bron die verwijst naar de verwijzingstabel en komt overeen met op velden voor sleutels vereist.

![Lookup Transformation](media/data-flow/lookup1.png "Lookup")

Selecteer de velden voor sleutels die u wilt tussen de velden van de referentie-bron en de binnenkomende stream velden overeenkomen. U moet eerst hebt gemaakt een nieuwe bron op het canvas ontwerp gegevensstroom gebruiken als de rechterkant voor de zoekopdracht.

Wanneer overeenkomsten worden gevonden, wordt de resulterende rijen en kolommen uit de Verwijzingsbron worden toegevoegd aan de gegevensstroom. U kunt kiezen welke velden van belang dat u wilt opnemen in uw Sink aan het einde van de gegevensstroom.
