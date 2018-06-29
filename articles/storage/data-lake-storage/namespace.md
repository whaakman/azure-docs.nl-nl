---
title: Azure Data Lake Storage Gen2 Preview hiërarchische Namespace
description: Beschrijving van het concept van hiërarchische naamruimte voor de Preview van Azure Data Lake Storage Gen2
services: storage
author: jamesbak
manager: twooley
ms.service: storage
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: f7c3820624a4ef27e2ece4d902f2c033b6a6f48f
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061217"
---
# <a name="azure-data-lake-storage-gen2-preview-hierarchical-namespace"></a>Azure Data Lake Storage Gen2 Preview hiërarchische naamruimte

Een mechanisme waarmee Azure Data Lake Storage Gen2 Preview om de prestaties van het bestandssysteem op schaal van object-opslag en prijzen, is de toevoeging van een **hiërarchische naamruimte**. Hierdoor kan de verzameling van objecten/bestanden binnen een account in een hiërarchie van mappen en geneste submappen worden georganiseerd op dezelfde manier waarop het bestandssysteem op uw computer zijn geordend. Met de hiërarchische naamruimte die is ingeschakeld, biedt Data Lake Storage Gen2 de schaalbaarheid en het rendement van opslag van objecten met file system-semantiek waarmee analytics motoren en frameworks bekend zijn.

## <a name="the-benefits-of-the-hierarchical-namespace"></a>De voordelen van de hiërarchische naamruimte

> [!NOTE]
> Tijdens de openbare preview van Azure Data Lake Storage Gen2 kunnen enkele van de hieronder vermelde functies verschillen in de beschikbaarheid ervan. Als u nieuwe functies en regio's worden uitgebracht tijdens de preview-programma, wordt deze informatie doorgegeven via onze gespecialiseerde Yammer-groep.  

De volgende voordelen zijn gekoppeld aan bestandssystemen die een hiërarchische naamruimte via blob-gegevens implementeren:

- **Atomic Directory manipulatie:** Object winkels geschatte een directory-hiërarchie door een conventie voor het insluiten van slashes (/) in de naam van het object om padsegmenten aan te geven. Deze overeenkomst werkt goed voor het ordenen van objecten, biedt de overeenkomst geen ondersteuning voor bewerkingen zoals het verplaatsen, hernoemen of verwijderen van mappen. Zonder echte mappen moeten toepassingen verwerken mogelijk miljoenen afzonderlijke blobs als u de taken op het niveau van de directory. De hiërarchische naamruimte wordt daarentegen deze taken verwerkt door het bijwerken van een afzonderlijke vermelding (de bovenliggende map). 

    Deze optimalisatie indrukwekkende is vooral belangrijk voor veel frameworks voor big data-analyses. Hulpprogramma's zoals Hive, Spark, enzovoort vaak uitvoer schrijven naar tijdelijke locatie en wijzig de naam van de locatie aan het einde van de taak. Zonder de hiërarchische naamruimte kan deze Wijzig de naam vaak langer duren dan het analytics-proces zelf. Lagere latentie van de taak is gelijk aan lagere totale eigendomskosten (TCO) voor analytics werkbelastingen.

- **De stijl van de Interface bekend:** bestandssystemen goed worden begrepen door ontwikkelaars en gebruikers. Er is niet nodig voor meer informatie over een nieuwe opslag paradigma wanneer u naar de cloud verplaatst, omdat de bestand system interface die door Data Lake Storage Gen2 het dezelfde paradigma gebruikt door computers, kleine en grote.

Een van de redenen dat object winkels hiërarchische naamruimten niet in het verleden hebt ondersteund is dat hiërarchische naamruimten scale beperkt. De Data Lake Storage Gen2 hiërarchische naamruimte zijn echter wordt lineair geschaald en geen nadelige invloed op de gegevenscapaciteit of de prestaties.

## <a name="when-to-enable-the-hierarchical-namespace"></a>Wanneer moet u de hiërarchische naamruimte inschakelen

Inschakelen van de hiërarchische naamruimte wordt aanbevolen voor opslag werkbelastingen die zijn ontworpen voor bestandssystemen die mappen bewerken. Dit omvat alle werkbelasting die hoofdzakelijk voor het analytics-verwerking. Gegevenssets waarvoor een hoge mate van de organisatie profiteren ook doordat de hiërarchische naamruimte.

De redenen voor het inschakelen van de hiërarchische naamruimte worden bepaald door een analyse van totale Eigendomskosten. Verbeteringen in werkbelasting latentie als gevolg van opslag versnelling moeten normaal gesproken rekenresources voor minder tijd. Latentie bij veel werkbelastingen kan worden verbeterd vanwege atomic directory bewerken dat is ingeschakeld door de hiërarchische naamruimte. In veel werkbelastingen de rekenbron vertegenwoordigt > 85% van de totale kosten en dus ook een matige vermindering van de werkbelasting latentie is gelijk aan een aanzienlijke hoeveelheid totale Eigendomskosten bespaard. Zelfs in gevallen waar opslagkosten voor het inschakelen van de hiërarchische naamruimte verhoogt, wordt de totale Eigendomskosten nog steeds verlaagd vanwege beperkte compute-kosten.

## <a name="when-to-disable-the-hierarchical-namespace"></a>Wanneer de hiërarchische naamruimte uitschakelen

Sommige werkbelastingen van de store object kunnen niet geen voordeel doordat de hiërarchische naamruimte krijgen. Voorbeelden van deze werkbelastingen zijn back-ups, afbeeldingen en andere toepassingen waarin object organisatie afzonderlijk van de objecten zelf opgeslagen (*bijvoorbeeld*, in een aparte database).

> [!NOTE]
> Als u de hiërarchische naamruimte inschakelt, wordt met de preview-versie er geen interoperabiliteit van gegevens of bewerkingen tussen de Blob- en Data Lake Storage Gen2 REST-API's. Deze functionaliteit wordt toegevoegd tijdens de preview.

## <a name="next-steps"></a>Volgende stappen

- [Een opslagaccount maken](./quickstart-create-account.md)