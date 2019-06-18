---
title: 'Converteren naar CSV: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van de converteren naar CSV-module in Azure Machine Learning-service voor een gegevensset converteren naar een CSV-indeling die kan worden gedownload, geëxporteerd of gedeeld met R of Python-script-modules.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 8b8b6758cc2df7a092ce36e9507f84ac534d0e3d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028723"
---
# <a name="convert-to-csv-module"></a>Converteren naar CSV-module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om een gegevensset converteren naar een CSV-indeling die kan worden gedownload, geëxporteerd of gedeeld met R of Python-script-modules.

### <a name="more-about-the-csv-format"></a>Meer informatie over de CSV-indeling 

De CSV-indeling, die staat voor 'met door komma's gescheiden waarden', is een bestandsindeling die wordt gebruikt door veel externe hulpmiddelen voor machine learning. CSV is een algemene indeling voor berichtenuitwisseling bij het werken met open source-talen, zoals R of Python.

Zelfs als u de meeste van uw werk in Azure Machine Learning, zijn er tijden wanneer is het wellicht handig is voor uw gegevensset converteren naar CSV voor gebruik in externe hulpprogramma's. Bijvoorbeeld:

+ Download het CSV-bestand om dit te openen in Excel of deze in een relationele database importeren.  
+ Sla het CSV-bestand voor cloudopslag en verbinding ermee maken vanuit Power BI om visualisaties te maken.  
+ Gebruik de CSV-indeling van het voorbereiden van gegevens voor gebruik in R en Python. Alleen met de rechtermuisknop op de uitvoer van de module voor het genereren van de code die nodig zijn voor toegang tot de gegevens rechtstreeks vanuit Python of een Jupyter-notebook. 

Wanneer u een gegevensset naar CSV converteren, wordt het bestand wordt opgeslagen in uw Azure ML-werkruimte. U kunt een hulpprogramma voor Azure-opslag te openen en het bestand rechtstreeks gebruiken of u kunt met de rechtermuisknop op de uitvoer van de module en download het CSV-bestand naar uw computer of worden gebruikt in R of Python-code.  

## <a name="how-to-configure-convert-to-csv"></a>Converteren naar CSV configureren

1.  Voeg de [converteren naar CSV](./convert-to-csv.md) module naar het experimentcanvas. U vindt deze module in de **indeling Gegevensconversies** groep in de interface. 

2. Verbinden met elke module die een gegevensset uitvoert.   
  
3.  Voer het experiment uit.

### <a name="results"></a>Resultaten
  

Dubbelklik op de uitvoer van [converteren naar CSV](./convert-to-csv.md), en selecteer een van deze opties.  

 + **Resultaat gegevensset -> Download**: Een kopie van de gegevens wordt onmiddellijk wordt geopend in CSV-indeling die u kunt opslaan op een lokale map. Als u een map opgeeft, een standaard-bestandsnaam wordt toegepast en het CSV-bestand wordt opgeslagen in de lokale **Downloads** bibliotheek.


 + **Resultaat gegevensset -> Opslaan als gegevensset**: Hiermee slaat het CSV-bestand terug naar de Azure ML-werkruimte als een afzonderlijke gegevensset.

 + **Code voor gegevenstoegang genereren**: Azure ML genereert twee sets met code voor toegang tot de gegevens met behulp van Python of met behulp van R. Voor toegang tot de gegevens, kopieert u het codefragment in uw toepassing. (*Code voor gegevenstoegang genereren komen binnenkort beschikbaar.* )

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 