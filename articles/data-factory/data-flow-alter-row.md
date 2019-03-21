---
title: Azure Data Factory gegevenstransformatie stroom Alter rij toewijzen
description: Het bijwerken van database-doelserver met behulp van Azure Data Factory toewijzing Flow Alter rij gegevenstransformatie
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: d842898ca700490ae99b46140be6609622a144df
ms.sourcegitcommit: 5f41e855d415cfa741d8f710792ea486480df5cf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58133158"
---
# <a name="azure-data-factory-alter-row-transformation"></a>Azure Data Factory Alter rij transformatie

Gebruik de transformatie Alter rij invoegen, verwijderen, bijwerken en upsert beleid instellen op rijen. U kunt een-op-veel voorwaarden toevoegen als een expressie. Elk van deze voorwaarden kan resulteren in een rij (of rijen) wordt ingevoegd, bijgewerkt, verwijderd of upsert. ALTER rij kan leiden tot zowel DDL & DML-acties voor uw database.

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Instellingen van de rij wijzigen](media/data-flow/alter-row1.png "rij-instellingen wijzigen")

> [!NOTE]
> Database sinks in de gegevensstroom wordt alleen ALTER rij transformaties uitgevoerd. De acties die u aan rijen (insert, update, delete, upsert toewijst) wordt niet uitgevoerd tijdens de foutopsporing sessies. U moet een taak uitvoeren gegevensstroom toevoegen aan een pijplijn en gebruik van pijplijn foutopsporing of triggers op te nemen van de beleidsregels van de rij wijzigen op uw database-tabellen.

## <a name="view-policies"></a>Inhoudbeleidsregels weergeven

Overschakelen van de gegevens stromen foutopsporingsmodus bij en bekijk vervolgens de resultaten van uw beleid van de rij wijzigen in het voorbeeld van gegevens. Uitvoeren van een rij wijzigen in gegevens Flow foutopsporingsmodus produceert DDL of DML acties op basis van het doel niet. Om deze acties optreden, voert u de gegevensstroom in een activiteit gegevensstroom uitvoeren binnen een pijplijn.

![Rij beleid ALTER](media/data-flow/alter-row3.png "rij beleid wijzigen")

Hierdoor kunt u om te controleren en de status van elke rij op basis van uw voorwaarden weergeven. Er zijn pictogram vertegenwoordigt voor elke insert, update, delete en upsert actie die wordt uitgevoerd in de gegevensstroom, die aangeeft welke actie zal plaatsvinden tijdens het uitvoeren van de gegevensstroom binnen een pijplijn.

## <a name="sink-settings"></a>Sink-instellingen

U moet een sink-type voor rij werken Alter database hebben. In de sink-instellingen, moet u elke actie moet worden toegestaan instellen.

![Rij sink ALTER](media/data-flow/alter-row2.png "Sink van rij wijzigen")

Het standaardgedrag in ADF gegevensstroom met database sinks is het invoegen van rijen. Als u toestaan van updates, upsert-bewerking en ook worden verwijderd wilt, moet u ook deze vakken in de sink om toe te staan de acties controleren.

> [!NOTE]
> Als uw toevoegingen, updates of upsert-bewerking wijzigt het schema van de doeltabel in de sink, mislukken de gegevensstroom. Als u wilt het doelschema in uw database wijzigen, moet u de optie 'Maak de tabel' in de sink. Dit verwijderen en opnieuw maken van de tabel met de nieuwe schemadefinitie.

## <a name="next-steps"></a>Volgende stappen

Na de transformatie Alter rij, kunt u [sink van uw gegevens naar een doelgegevensarchief](data-flow-sink.md).
