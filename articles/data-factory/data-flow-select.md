---
title: Azure-Gegevensfactory selecteren gegevenstransformatie stroom toewijzen
description: Azure-Gegevensfactory selecteren gegevenstransformatie stroom toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: bc83b41067d587adce41658a2c4b3d68969750ba
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729335"
---
# <a name="azure-data-factory-mapping-data-flow-select-transformation"></a>Azure-Gegevensfactory selecteren gegevenstransformatie stroom toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik deze transformatie voor kolom selectiviteit (vermindering van het aantal kolommen) of alias kolommen en namen van de stream.

De optie transformatie kunt u tot de alias van een hele stream of kolommen in deze stroom, verschillende namen (aliassen) toewijzen en vervolgens te verwijzen naar de nieuwe namen verderop in de gegevensstroom. Deze transformatie is handig voor scenario's self-join. De manier voor het implementeren van een self-join in ADF gegevensstroom is een stream, vertakking, met "Nieuwe vertakking" en vervolgens direct daarna wordt toegevoegd een transformatie 'Select' uitvoeren. Deze stroom hebben nu een nieuwe naam die u gebruiken kunt om toe te voegen terug naar de oorspronkelijke stroom een self-join maakt:

![Self-join](media/data-flow/selfjoin.png "Self-join")

In het bovenstaande diagram, wordt de optie transformatie is aan de bovenkant. Dit is de oorspronkelijke stroom naar "OrigSourceBatting" aliasing. In de transformatie van de Join Vervang hieronder ziet u dat we deze stroom Selecteer alias als de rechter join gebruiken, zodat we om te verwijzen naar dezelfde sleutel in de linker & de rechterkant van de Inner Join.

Selecteer kan ook worden gebruikt als een manier ongedaan maken kolommen selecteren uit de gegevensstroom. Als u 6 kolommen die zijn gedefinieerd in uw sink hebt, maar u alleen ophalen van een specifieke 3 wilt te transformeren en vervolgens naar de sink stromen, kunt u bijvoorbeeld alleen die 3 selecteren met behulp van de transformatie selecteren.

> [!NOTE]
> U moet uitschakelen "Alles selecteren' alleen bepaalde kolommen kiezen

Opties

De standaardinstelling voor 'Select' is het opnemen van alle binnenkomende kolommen en blijven de oorspronkelijke naam. U kunt alias de stroom door het instellen van de naam van de optie transformatie.

Alias afzonderlijke kolommen, schakelt u 'Alles selecteren' en de kolomtoewijzing gebruiken aan de onderkant.

![Selecteer transformatie](media/data-flow/select001.png "Alias selecteren")
