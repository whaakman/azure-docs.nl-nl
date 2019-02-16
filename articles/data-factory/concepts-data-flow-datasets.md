---
title: Azure Data Factory gegevensstroom gegevenssets toewijzen
description: Azure Data Factory toewijzing gegevensstroom is sepecific gegevensset compatibiliteit
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: ccf4273489d739bb9b0d802b79944efefcd02ff4
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56331190"
---
# <a name="mapping-data-flow-datasets"></a>Toewijzing van gegevensstroom gegevenssets

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gegevenssets zijn een Data Factory-constructie die de vorm van de gegevens waarmee die u in de pijplijn werkt. In de gegevensstroom moet rij- en gegevens op een definitie nog veel meer fijn nauwkeurigere dan is vereist voor gegevenssets in de Controlestroom pijplijn.

Gegevenssets in Flow gegevensbron en Sink transformaties gebruiken voor het definiëren van het schema van de basic. Als u geen schema in uw gegevens, kunt u Schema Drift instellen op voor de bron en Sink. Met het schema is gedefinieerd in de gegevensset, hebt u de gerelateerde gegevenstypen, opmaak van gegevens, locatie en verbindingsinformatie van de bijbehorende gekoppelde Service.

Op dit moment in de gegevensstroom vindt u vier gegevenssets:

* Azure SQL Database
* Azure SQL DW
* Parquet
* Tekst met scheidingstekens

Gegevensstroom gegevenssets scheiden van de bron *type* van het verbindingstype van de gekoppelde Service. Meestal in de Data Factory, kies het verbindingstype (Blob, ADLS, enzovoort) en vervolgens het type van bestand in de gegevensset te definiëren. In de gegevensstroom kiest u de typen gegevensbronnen, die gekoppeld aan verschillende verbindingstypen van de gekoppelde Service worden kunnen.

![Opties voor transformatie](media/data-flow/dataset1.png "bronnen")

Bij het maken van een nieuwe gegevensset, is er een selectievakje met het label 'Data Flow compatibel' in de rechterbovenhoek van het paneel. Op deze knop klikt, wordt alleen de gegevenssets die kunnen worden gebruikt met de gegevens stromen filteren. 

Schema's importeren

Bij het importeren van het schema van de gegevensstroom gegevenssets, ziet u een knop Schema importeren. Op deze knop klikt, geeft u twee opties: Importeren uit de bron of importeren uit een lokaal bestand. In de meeste gevallen zult u het schema importeren rechtstreeks vanuit de bron. Als u een bestaand schemabestand hebt, kunt u verwijzen naar het lokale bestand en Data Factory definieert het schema op basis van dat schemabestand.

