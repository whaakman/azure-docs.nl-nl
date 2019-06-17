---
title: Azure Data Factory-overzicht van stroom toewijzen
description: Een uitleg overzicht van de toewijzing van gegevens stromen in Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 051886f98d6d35594336291bbb2defb2a4acdfc5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233063"
---
# <a name="what-are-mapping-data-flows"></a>Wat zijn de gegevens stromen toewijzen?

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Toewijzing van gegevens stromen zijn visueel ontworpen gegevenstransformatie in Azure Data Factory. Gegevensstromen kunt gegevenstechnici voor het ontwikkelen van grafische gegevenstransformatielogica zonder code te schrijven. De resulterende gegevensstromen worden als activiteiten binnen Azure Data Factory-pijplijnen met uitgebreide Azure Databricks-clusters uitgevoerd.

Het doel van de gegevensstroom van Azure Data Factory is om te voorzien van een volledig visuele ervaring geen codering vereist. Uw gegevens stromen worden uitgevoerd op uw eigen cluster uitvoeren voor de verwerking van uitgebreide gegevens. Azure Data Factory wordt alle van de vertaling, pad optimalisatie en uitvoering van uw flow-taken voor gegevens.

Begint met het maken van gegevensstromen in de foutopsporingsmodus, zodat u kunt uw gegevenstransformatielogica interactief valideren. Een gegevensstroom-activiteit vervolgens toevoegen aan uw pijplijn uit te voeren en test uw gegevens over flow in pijplijn foutopsporing of 'Nu activeren' in de pijplijn gebruiken om uw stroom gegevens van de activiteit van een pijplijn te testen.

U wordt vervolgens plannen en controleren van uw stroomactiviteiten gegevens met behulp van Azure Data Factory-pijplijnen die de activiteit van de gegevensstroom worden uitgevoerd.

De schakeloptie foutopsporingsmodus in-/ uitschakelen op het ontwerpoppervlak gegevensstroom kunt interactieve samenstelling van gegevenstransformaties. Foutopsporingsmodus biedt een data prep gegevensstroom bouw omgeving.
