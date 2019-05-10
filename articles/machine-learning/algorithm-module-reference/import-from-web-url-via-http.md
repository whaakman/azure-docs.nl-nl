---
title: 'Importeren van Web-URL via HTTP: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van het importeren van Web-URL via HTTP-module in Azure Machine Learning-service gegevens lezen uit een openbare webpagina voor gebruik in een machine learning-experiment.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: bff913efb38c9e5589c795386dfbbc480d799a37
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411446"
---
# <a name="import-from-web-url-via-http-module"></a>Importeren vanuit de Web-URL via HTTP-module

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module gegevens lezen uit een openbare webpagina voor gebruik in een machine learning-experiment.

De volgende beperkingen gelden voor gegevens die zijn gepubliceerd op een webpagina:

- Gegevens moeten in een van de ondersteunde indelingen zijn: CSV, TSV, ARFF of SvmLight. Andere gegevens leiden tot fouten.
- Geen verificatie is vereist of ondersteund. Gegevens moeten openbaar beschikbaar zijn. 

Er zijn twee manieren om gegevens te verkrijgen: Gebruik de wizard voor het instellen van de gegevensbron, of handmatig configureren.

## <a name="use-the-data-import-wizard"></a>Gebruik de Wizard gegevens importeren

1. Voeg de **importgegevens** module naar het experimentcanvas. U kunt de module vinden in de interface, in de **gegevensinvoer en uitvoer** categorie.

2. Klik op **Wizard importeren starten** en selecteer de Web-URL via HTTP.

3. Plak de URL en selecteer een gegevensindeling.

4. Wanneer de configuratie is voltooid.

Als u wilt bewerken van een bestaande gegevensverbinding, moet u de wizard opnieuw starten. De wizard laadt alle vorige configuratiedetails, zodat u helemaal opnieuw hoeft

## <a name="manually-set-properties-in-the-import-data-module"></a>Handmatig de eigenschappen instellen in de module gegevens importeren

De volgende stappen wordt beschreven hoe u het handmatig configureren van de bron voor het importeren.

1. Voeg de [importgegevens](import-data.md) module naar het experimentcanvas. U kunt de module vinden in de interface, in de **gegevensinvoer en uitvoer** categorie.

2. Voor **gegevensbron**, selecteer **Web-URL via HTTP**.

3. Voor **URL**typt of plakt u de volledige URL van de pagina met de gegevens die u wilt laden.

    De URL moet bevatten de site-URL en het volledige pad, met de naam en extensie hebben, naar de pagina met de gegevens te laden.

    Zo bevat de volgende pagina de Iris-gegevensset van de machine learning-opslagplaats van de universiteit van Californië, Irvine:

    `https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data`

4. Voor **gegevensindeling**, selecteer een van de ondersteunde gegevens in de lijst met indelingen.

    Het is raadzaam dat u vooraf de gegevens altijd controleren om te bepalen van de indeling. De pagina UC Irvine maakt gebruik van de CSV-indeling. Andere van ondersteunde gegevensindelingen zijn TSV, ARFF en SvmLight.

5. Als de gegevens zich bevinden in CSV- of TSV-indeling, gebruikt u de **bestand heeft de rij met koppen** optie om aan te geven of de brongegevens een rij met koppen bevat. De rij met koppen wordt gebruikt voor het toewijzen van de namen van kolommen.

6. Selecteer de **gebruiken in de cache opgeslagen resultaten** opties als u niet verwacht de gegevens dat te veel wijzigen, of als u wilt voorkomen dat opnieuw laden van de gegevens van elke keer dat u het experiment uit te voeren.

    Wanneer deze optie is geselecteerd, worden de gegevens de eerste keer de module wordt uitgevoerd, en daarna een in cache opgeslagen versie van de gegevensset wordt gebruikt door het experiment geladen.

    Als u laden van de gegevensset op elke herhaling van de gegevensset experiment wilt, schakelt u de **gebruiken in de cache opgeslagen resultaten** optie. Resultaten worden ook opnieuw geladen als er wijzigingen in de parameters van [importgegevens](import-data.md).

7. Voer het experiment uit.

## <a name="results"></a>Resultaten

Als u klaar bent, klikt u op de uitvoergegevensset en selecteer **Visualize** om te zien als de gegevens is geïmporteerd.


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 