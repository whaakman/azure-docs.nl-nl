---
title: Azure Data Factory toewijzing van de gegevensstroom controleren en voorbeeld van gegevens
description: Azure Data Factory toewijzing van de gegevensstroom is een deelvenster waarin gegevensstroom metagegevens (inspecteren) en voorbeeld van gegevens in de foutopsporingsmodus (voorbeeld van gegevens)
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 47cde50e0874f0f73523925b6d1b2f8ee4abaea9
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56727822"
---
# <a name="azure-data-factory-mapping-data-flow-transformation-inspect-tab"></a>Azure Data Factory toewijzing gegevenstransformatie-stroom controleren tabblad

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Controleren van venster](media/data-flow/agg3.png "deelvenster controleren")

Het deelvenster inspecteren biedt inzicht in de metagegevens van de stroom van de gegevens die u transformeren. Kunt u zich om te zien van het aantal kolommen, kolommen hebt gewijzigd, kolommen toegevoegd, gegevenstypen, schema en kolomverwijzingen. 'Controleren' is een alleen-lezen weergave van de metagegevens. U hoeft niet te hebben foutopsporingsmodus ingeschakeld als u wilt bekijken metadate in het deelvenster inspecteren.

Als u de vorm van uw gegevens via transformaties wijzigt, ziet u de metagegevens van de stroom door het deelvenster inspecteren wordt gewijzigd. Als er geen een gedefinieerd schema in uw bron-transformatie, kan de metagegevens wordt niet meer zichtbaar in het deelvenster inspecteren. Ontbreken van metagegevens is gebruikelijk in Schema Drift scenario's.

![Voorbeeld van gegevens](media/data-flow/datapreview.png "voorbeeldweergave van gegevens")

Voorbeeld van gegevens is een deelvenster dat zorgt voor een alleen-lezen weergave van uw gegevens wanneer deze wordt gewijzigd. U kunt de uitvoer van de transformatie en expressies voor het valideren van de gegevensstroom kunt weergeven. U kunt de foutopsporingsmodus ontstoken om te zien van voorbeelden van gegevens moet hebben. Wanneer u op de kolommen in het gegevensraster preview klikt, ziet u een volgende deelvenster aan de rechterkant. Het pop-out-deelvenster ziet u de profiel-statistieken over elk van de kolommen die u selecteert.

![Gegevensvoorbeeld grafiek](media/data-flow/chart.png "Gegevensvoorbeeld grafiek")

