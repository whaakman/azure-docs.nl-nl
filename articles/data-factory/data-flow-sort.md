---
title: Azure Data Factory stroom sorteren transformatie
description: Azure Data Factory gegevens sorteren Join transformatie
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: 7adc8c80ecca12364fb7f761092b1a5d9f528e19
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729760"
---
# <a name="azure-data-factory-data-sort-transformations"></a>Azure Data Factory-gegevenstransformaties sorteren

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Instellingen voor sorteren](media/data-flow/sort.png "sorteren")

De transformatie sorteren kunt u de binnenkomende rijen op de huidige gegevensstroom sorteren. De uitgaande rijen uit de transformatie sorteren volgt later het bestelproces door u ingestelde regels. U kunt afzonderlijke kolommen kiezen en deze sorteren, ASC of DEC, met behulp van de indicator van de pijl naast elk veld. Als u de kolom wijzigen wilt voordat u de sortering toepast, klikt u op 'Berekende kolommen' om te starten van de expressie-editor. Dit biedt de mogelijkheid om te maken van een expressie voor de sorteerbewerking in plaats van gewoon toepassen van een kolom voor het sorteren.

U kunt inschakelen 'Hoofdlettergevoelig' als u hoofdlettergevoeligheid niet wilt bij het sorteren van velden tekenreeks of tekst.

'Sorteren alleen in partities' maakt gebruik van Spark partitioneren van gegevens. Gegevens stromen sorteren gepartitioneerde gegevens in plaats van de gehele gegevensstroom sorteren door te sorteren van binnenkomende gegevens slechts binnen elke partitie.

Elk van de voorwaarden sorteren van de transformatie sorteren kan opnieuw worden gerangschikt. Dus als u verplaatsen van een kolom die hoger in de volgorde sorteren wilt, pak die rij met de muis en hoger of lager in de lijst sorteren verplaatsen.

Partitioneren van de gevolgen voor sorteren

ADF-gegevensstroom wordt uitgevoerd op big data Spark-clusters met de gegevens worden verspreid over meerdere knooppunten en partities. Het is belangrijk dat u er rekening mee houden dat bij het ontwerpen van uw gegevensstroom als u zijn afhankelijk van de transformatie sorteren om gegevens te bewaren in die volgorde. Als u kiest voor het partitioneren van uw gegevens in een latere transformatie, verliest u mogelijk de sortering vanwege die reshuffling van gegevens.
