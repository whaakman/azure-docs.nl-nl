---
title: Gegevensstroom van Azure Data Factory toewijzing tabblad optimaliseren
description: Azure Data Factory gegevensstromen met optimaliseren tabblad partitie-instellingen toewijzen optimaliseren
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 31aca6f4ccfe9991a1dcf2808815a4543e2c6fe6
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56326456"
---
# <a name="mapping-data-flow-transformation-optimize-tab"></a>Toewijzing van Flow gegevenstransformatie tabblad optimaliseren

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Elke transformatie gegevensstroom heeft een tabblad 'Optimaliseren'. Het tabblad optimaliseren bevat optionele instellingen voor het configureren van de partitioneringsschema's voor gegevensoverdrachten.

<img src="media/data-flow/opt001.png" width="800">

De standaardinstelling is 'Gebruik huidige partitionering'. Huidige partitioneren zorgt Azure Data Factory het partitieschema van systeemeigen naar gegevens stromen die worden uitgevoerd op Spark in Azure Databricks gebruiken. Dit is gewoonlijk de aanbevolen aanpak.

Er zijn echter gevallen waarin u desgewenst kunt om aan te passen de partitionering. Bijvoorbeeld, als u uitvoeren naar een bestand in de lake transformaties wilt, kiest vervolgens 'één partitie' op het tabblad optimaliseren voor het partitioneren van in de Sink-transformatie.

Er is een andere aanvraag waar u kunt desgewenst controle hebben over de partitioneringsschema's die wordt gebruikt voor uw gegevenstransformaties wat betreft prestaties. Aanpassen van het partitioneren van gegevens biedt een niveau van controle over de distributie van uw gegevens voor compute-knooppunten en gegevens plaats optimalisaties die kunnen zowel positief als negatief effect op de prestaties van uw algehele gegevensstroom hebben.

Als u wijzigen op een transformatie partitioneren wilt, klik op het tabblad optimaliseren en selecteer het keuzerondje 'Ingesteld partitioneren'. U krijgt vervolgens met een reeks opties voor het partitioneren van. De beste methode voor het implementeren van partitioneren kan variëren op basis van uw gegevensvolumes, kandidaatsleutels, null-waarden en kardinaliteit. Aanbevolen procedure is om te beginnen met het standaard partitioneren en probeer de verschillende opties voor partitioneren. U kunt testen met behulp van de foutopsporing uitvoeren in de pijplijn en bekijk vervolgens de tijd die in elke transformatie groeperen en gebruik van de partitie van de weergave bewaking.

<img src="media/data-flow/opt002.png" width="600">

### <a name="round-robin"></a>Round robin

Round Robin is eenvoudige partitie die gegevens automatisch gelijkmatig over meerdere partities verdeelt. Gebruik Round Robin als u geen sleutel geschikt voor het implementeren van een solide, intelligente partitioneringsstrategie hebben. U kunt het aantal fysieke partities instellen.

### <a name="hash"></a>Hash

Azure Data Factory levert een hash van de kolommen voor het produceren van uniform partities, zodat de rijen met vergelijkbare waarden vallen in dezelfde partitie. Wanneer u de Hash-optie gebruikt, kunt u testen voor mogelijke partitie scheeftrekken. U kunt het aantal fysieke partities instellen.

### <a name="dynamic-range"></a>Dynamisch bereik

Dynamisch bereik maakt gebruik van Spark dynamische bereiken op basis van de kolommen of expressies die u opgeeft. U kunt het aantal fysieke partities instellen. 

### <a name="fixed-range"></a>Vaste bereik

U moet een expressie waarmee een vaste bereik van waarden in de kolommen gepartitioneerde gegevens op te bouwen. U hebt een goed begrip van uw gegevens voordat u deze optie om te voorkomen dat scheeftrekken partitie. De waarde die voor de expressie invoert, zal worden gebruikt als onderdeel van een partitiefunctie. U kunt het aantal fysieke partities instellen.

### <a name="key"></a>Sleutel

Hebt u een goed begrip van de kardinaliteit van uw gegevens, kan het partitioneren van sleutels een strategie voor een goede partitie zijn. Belangrijkste partitioneren om partities voor elke unieke waarde in de kolom te maken. U kunt het aantal partities niet instellen omdat het nummer wordt gebaseerd op unieke waarden in de gegevens.
