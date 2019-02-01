---
title: Azure Data Lake Storage Gen2 Preview hiërarchische Namespace
description: Beschrijving van het concept van de hiërarchische naamruimte voor de Preview van Azure Data Lake Storage Gen2
services: storage
author: jamesbak
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: jamesbak
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 967e24ae6e004fe6ce2b1c0aa6c039f46be2598c
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55244501"
---
# <a name="azure-data-lake-storage-gen2-preview-hierarchical-namespace"></a>Azure Preview van Data Lake Storage Gen2 hiërarchische naamruimte

Een mechanisme waarmee Azure Data Lake Storage Gen2 Preview voor prestaties op schaal van de opslag van object en de prijzen van het bestandssysteem is de toevoeging van een **hiërarchische naamruimte**. Hiermee wordt de verzameling van objecten/bestanden binnen een account om te worden ingedeeld in een hiërarchie van mappen en geneste submappen op dezelfde manier als dat het bestandssysteem op uw computer is ingedeeld. Met de hiërarchische naamruimte die is ingeschakeld, wordt een opslagaccount kan de schaalbaarheid en rentabiliteit van objectopslag, voorzien van semantiek van het bestandssysteem die bekend bij de analyse-engines en frameworks zijn.

## <a name="the-benefits-of-the-hierarchical-namespace"></a>De voordelen van de hiërarchische naamruimte

De volgende voordelen zijn gekoppeld aan bestandssystemen die een hiërarchische naamruimte worden geïmplementeerd via blob-gegevens:

- **Atomische directory bewerken:** Object winkels schatting maken van een directory-hiërarchie door een overeenkomst voor het insluiten van schuine streep (/) in de objectnaam om aan te duiden padsegmenten vast te stellen. Deze overeenkomst werkt voor het ordenen van objecten, biedt de overeenkomst geen ondersteuning voor bewerkingen zoals het verplaatsen, naam wijzigen of verwijderen van mappen. Zonder echte mappen moeten toepassingen verwerken mogelijk miljoenen afzonderlijke blobs om taken op het niveau van de directory. De hiërarchische naamruimte worden deze taken, daarentegen, verwerkt door het bijwerken van een afzonderlijke vermelding (de bovenliggende map).

    Deze indrukwekkende optimalisatie is met name belangrijk voor veel big data analytics-frameworks. Hulpprogramma's, zoals Hive, Spark, enz. vaak uitvoer schrijven naar tijdelijke locaties en vervolgens de naam van de locatie aan het einde van de taak. Zonder de hiërarchische naamruimte, kan deze naam vaak langer duren dan de analytics-proces zelf. Lagere latentie van de taak is gelijk aan lagere totale eigendomskosten (TCO) voor werkbelastingen voor gegevensanalyse.

- **Stijl van de vertrouwde-Interface:** Bestandssystemen worden goed begrepen door ontwikkelaars en gebruikers. Er is niet nodig voor meer informatie over een nieuwe opslag paradigma wanneer u naar de cloud verplaatst, omdat de bestand system interface beschikbaar is gemaakt door Data Lake Storage Gen2 het dezelfde paradigma die door computers, grote als kleine volumes is.

Een van de redenen dat object winkels hiërarchische naamruimten niet in het verleden hebt ondersteund is dat hiërarchische naamruimten schaal beperkt. De Data Lake Storage Gen2 hiërarchische naamruimte zijn echter schaalt lineair en geen nadelige invloed op de gegevenscapaciteit- of Prestatieweergave.

## <a name="when-to-enable-the-hierarchical-namespace"></a>Bij het inschakelen van de hiërarchische naamruimte

Inschakelen van de hiërarchische naamruimte wordt aanbevolen voor opslag-werkbelastingen die zijn ontworpen voor bestandssystemen die mappen bewerken. Dit omvat alle werkbelastingen die voornamelijk voor analyseverwerking. Gegevenssets waarvoor een hoge mate van de organisatie profiteren ook doordat de hiërarchische naamruimte.

De redenen voor het inschakelen van de hiërarchische naamruimte worden bepaald door een TCO-analyse. Verbeteringen in de workload latentie vanwege storage acceleration moet in het algemeen, compute-resources voor minder tijd. Latentie voor veel werkbelastingen kan worden verbeterd vanwege atomic directory-manipulaties die is ingeschakeld door de hiërarchische naamruimte. In veel werkbelastingen staat > 85% van de totale kosten voor de compute-resource en zelfs een matige vermindering van de werkbelasting latentie komt overeen met een aanzienlijke hoeveelheid TCO-besparingen. De totale Eigendomskosten is zelfs in gevallen waar de hiërarchische naamruimte inschakelen verhoogt de kosten voor opslag, nog steeds het vanwege de verminderde computerkosten verlaagd.

## <a name="when-to-disable-the-hierarchical-namespace"></a>Wanneer u de hiërarchische naamruimte uitschakelen

Sommige werkbelastingen van de store object kunnen niet een voordeel krijgen door in te schakelen de hiërarchische naamruimte. Voorbeelden van deze workloads zijn back-ups, afbeeldingopslag en andere toepassingen waar object organisatie afzonderlijk van de objecten zelf opgeslagen (*bijvoorbeeld*, in een afzonderlijke database).


## <a name="next-steps"></a>Volgende stappen

- [Een opslagaccount maken](./data-lake-storage-quickstart-create-account.md)
