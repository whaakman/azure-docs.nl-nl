---
title: Azure Data Factory toewijzing gegevensstroom visueel bewaken
description: Visueel bewaken Azure Data Factory gegevens stromen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 1497e6b85d3f577064b7a90fb1bcf5cbeb4a1f40
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56213349"
---
# <a name="monitor-data-flows"></a>Gegevensoverdrachten van monitor

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Nadat u ontwikkeling en foutopsporing van uw gegevensstroom hebt voltooid, wilt u de gegevensstroom uit te voeren volgens een schema in de context van een pijplijn plannen. U kunt de pijplijn van Azure Data Factory met behulp van Triggers plannen. Of u de optie nu activeren in de opbouwfunctie voor Azure Data Factory-pijplijn kunt gebruiken voor het uitvoeren van een uitvoering van één uitvoeren als u wilt testen van de gegevensstroom binnen de context van de pijplijn.

Als u uw pijplijn uitvoert, kunt u zich voor het bewaken van de pijplijn en alle activiteiten die zijn opgenomen in de pijplijn met inbegrip van de activiteit gegevens stromen. Klik op het pictogram van de monitor in het linkerdeelvenster van Azure Data Factory-gebruikersinterface. U ziet een scherm zoals hieronder wordt weergegeven. De gemarkeerde pictogrammen kunt u inzoomen op de activiteiten in de pijplijn, met inbegrip van de activiteit gegevens stromen.

<img src="media/data-flow/mon001.png" width="800">

U ziet statistieken op dit niveau als goed inculding de uitvoeringstijden en -status. De ID worden uitgevoerd op het activiteitenniveau van de verschilt dat de Run-ID op het pijplijnniveau van de. De ID worden uitgevoerd op het niveau van de vorige is voor de pijplijn. Te klikken op de bril krijgt u uitgebreide informatie op de uitvoering van de stroom van uw gegevens.

<img src="media/data-flow/mon002.png" width="800">

Wanneer u zich in het grafische knooppunt controle weergeven, ziet u een vereenvoudigde weergave alleen-lezen versie van uw gegevensstroom-grafiek.

<img src="media/data-flow/mon003.png" width="800">

## <a name="view-data-flow-execution-plans"></a>Weergave Data-abonnementen voor uitvoering van Flow

Wanneer de stroom van gegevens wordt uitgevoerd in Databricks, bepaalt de Azure Data Factory optimale codepaden op basis van de entirity van de gegevensstroom. De uitvoering van paden kunnen ook optreden op verschillende knooppunten van de scale-out en gegevenspartities. De controle grafiek vertegenwoordigt daarom het ontwerp van uw stroom, rekening houdend met het uitvoeringspad van transformaties. Wanneer u op de afzonderlijke knooppunten klikt, ziet u 'groeperingen", waarbij de code die samen op het cluster is uitgevoerd. De tijdsinstellingen en aantallen die u ziet staan voor deze groepen in plaats van de afzonderlijke stappen in uw ontwerp.

<img src="media/data-flow/mon004.png" width="800"> 

* Wanneer u op de open ruimte in het venster bewaking klikt, de statistieken in het onderste deelvenster timing wordt weergegeven en rij voor elke Sink- en de transformaties die hebben geleid tot de sink-gegevens voor transformatie afkomst telt.

* Wanneer u afzonderlijke transformaties selecteert, ontvangt u aanvullende feedback in het rechter deelvenster waarin statistieken voor partitie, telt het aantal kolommen, asymmetrie (hoe gelijkmatig is de gegevens verdeeld over meerdere partities), en kurtosis (hoe spikey zijn de gegevens).

* Wanneer u op de Sink in de weergave van het knooppunt klikt, ziet u afkomst van de kolom. Er zijn drie verschillende methoden die kolommen in de gegevensstroom naar komt u in de Sink wordt geteld. Dit zijn:

  * Berekend: Gebruik de kolom voor voorwaardelijke verwerking of in een expressie in de gegevensstroom, maar niet terechtkomen in de Sink
  * Afgeleid: De kolom is een nieuwe kolom die u hebt gegenereerd in de stroom, dat wil zeggen het is niet aanwezig in de bron
  * Toegewezen: De kolom afkomstig is van de bron- en u deze toewijzen aan een sink-veld
  
## <a name="monitor-icons"></a>Monitorpictogrammen

Dit pictogram geeft aan dat de van Transformatiegegevens al in de cache op het cluster, opgeslagen is zodat de tijdsinstellingen en het uitvoeringspad die in aanmerking hebben genomen:

<img src="media/data-flow/mon005.png" width="800"> 

U ziet ook de groene cirkel pictogrammen in de transformatie. Ze vertegenwoordigen een telling van het aantal sinks die gegevens worden doorgestuurd naar.
