---
title: Hiërarchische naam ruimte Azure Data Lake Storage Gen2
description: Hierin wordt het concept van de hiërarchische naam ruimte voor Azure Data Lake Storage Gen2 beschreven
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: normesta
ms.reviewer: jamesbak
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 0b98892bd31b097e3dc217d54f52f12550599d32
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68847144"
---
# <a name="azure-data-lake-storage-gen2-hierarchical-namespace"></a>Hiërarchische naam ruimte Azure Data Lake Storage Gen2

Een belang rijk mechanisme waarmee Azure Data Lake Storage Gen2 bestandssysteem prestaties kan bieden bij het schalen van object opslag en prijzen is de toevoeging van een **hiërarchische naam ruimte**. Hierdoor kan de verzameling van objecten/bestanden binnen een account worden georganiseerd in een hiërarchie van directory's en geneste submappen op dezelfde manier als het bestands systeem op uw computer is ingedeeld. Als de hiërarchische naam ruimte is ingeschakeld, wordt een opslag account in staat gesteld om de schaal baarheid en kosten effectiviteit van object opslag te bieden, met bestandssysteem semantiek die bekend zijn met analyse-engines en frameworks.

## <a name="the-benefits-of-the-hierarchical-namespace"></a>De voor delen van de hiërarchische naam ruimte

De volgende voor delen zijn gekoppeld aan bestands systemen die een hiërarchische naam ruimte implementeren via BLOB-gegevens:

- **Atomic Directory-manipulatie:** Het object slaat een Directory-hiërarchie in een benadering op door een Conventie voor het insluiten van slashes (/) in de object naam in te stellen om padsegmenten aan te duiden. Hoewel deze Conventie werkt voor het organiseren van objecten, biedt de Conventie geen hulp voor acties zoals verplaatsen, naam wijzigen of verwijderen van mappen. Zonder echte directory's moeten toepassingen mogelijk miljoenen afzonderlijke blobs verwerken om taken op Directory niveau te verzorgen. Daarentegen worden deze taken door de hiërarchische naam ruimte verwerkt door één vermelding (de bovenliggende map) bij te werken.

    Deze dramatische Optima Lise ring is met name van belang voor veel big data Analytics-frameworks. Hulpprogram ma's als Hive, Spark, enzovoort schrijven vaak uitvoer naar tijdelijke locaties en de naam van de locatie aan het einde van de taak. Zonder de hiërarchische naam ruimte kan deze naam vaak langer duren dan het analytische proces zelf. De lagere taak latentie is gelijk aan de lagere total cost of ownership (TCO) voor analyse werkbelastingen.

- **Vertrouwde interface stijl:** Bestands systemen zijn duidelijk van toepassing op ontwikkel aars en gebruikers. Het is niet nodig om een nieuw opslag paradigma te leren wanneer u naar de Cloud gaat als de bestandssysteem interface die wordt weer gegeven door Data Lake Storage Gen2, hetzelfde paradigma is dat wordt gebruikt door computers, groot en klein.

Een van de redenen waarom object archieven niet historisch worden ondersteund een hiërarchische naam ruimte is dat een hiërarchische naam ruimte limiet schaalbaar is. De Data Lake Storage Gen2 hiërarchische naam ruimte schaalt echter lineair en verlaagt niet de capaciteit of de prestaties van de gegevens.

## <a name="when-to-enable-the-hierarchical-namespace"></a>Wanneer de hiërarchische naam ruimte inschakelen

U wordt aangeraden de hiërarchische naam ruimte in te scha kelen voor werk belastingen voor opslag die zijn ontworpen voor bestands systemen waarmee directory's worden gemanipuleerd. Dit omvat alle werk belastingen die hoofd zakelijk voor analyse verwerking zijn. Gegevens sets die een hoge mate van organisatie vereisen, hebben ook voor deel door de hiërarchische naam ruimte in te scha kelen.

De redenen voor het inschakelen van de hiërarchische naam ruimte worden bepaald door een TCO-analyse. Over het algemeen moeten de verbeteringen in de latentie van de werk belasting vanwege de opslag versnelling minder tijd in beslag nemen. Een latentie voor veel werk belastingen kan worden verbeterd vanwege een Atomic-Directory bewerking die is ingeschakeld door de hiërarchische naam ruimte. In veel werk belastingen vertegenwoordigt de reken Resource > 85% van de totale kosten, waardoor zelfs een bescheiden verkleinings latentie gelijk is aan een aanzienlijke besparing van de totale eigendoms kosten. Zelfs in gevallen waarin het inschakelen van de hiërarchische naam ruimte de opslag kosten verhoogt, wordt de TCO nog steeds verlaagd door lagere reken kosten.

## <a name="when-to-disable-the-hierarchical-namespace"></a>Wanneer de hiërarchische naam ruimte uitschakelen

Sommige werk belastingen van een object archief kunnen geen voor deel verwerven door de hiërarchische naam ruimte in te scha kelen. Voor beelden zijn onder meer back-ups, opslag van installatie kopieën en andere toepassingen waarbij object organisaties onafhankelijk van de objecten zelf worden opgeslagen (bijvoorbeeld: in een afzonderlijke data base).

## <a name="next-steps"></a>Volgende stappen

- [Een opslag account maken](./data-lake-storage-quickstart-create-account.md)
