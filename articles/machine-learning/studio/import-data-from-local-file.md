---
title: Gegevens importeren uit een bestand in Azure Machine Learning Studio | Microsoft Docs
description: Informatie over het uploaden van een gegevensbestand training van de harde schijf naar Azure Machine Learning Studio. Hiermee maakt u een gegevensset-module in de werkruimte.
keywords: gegevens, gegevensindeling, gegevenstypen, gegevensbronnen, trainingsgegevens importeren
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: c0dd9e90-23c4-4f64-8b8f-489ad79f047b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;bradsev
ms.openlocfilehash: 67f66f9b8703f2cab93a2274a90c161a55848c34
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a>Trainingsgegevens importeren uit een bestand op de harde schijf naar Machine Learning Studio
[!INCLUDE [import-data-into-aml-studio-selector](../../../includes/machine-learning-import-data-into-aml-studio.md)]

Informatie over het uploaden van een gegevensbestand van de vaste schijf om te gebruiken als trainingsgegevens in Azure Machine Learning Studio. Door het importeren van het gegevensbestand, hebt u een gegevensset module klaar voor gebruik in uw werkruimte.

## <a name="steps-to-import-data-from-a-local-file"></a>Stappen voor het importeren van gegevens uit een lokaal bestand
Om gegevens te importeren uit een lokale vaste schijf, het volgende doen:

1. Klik op **+ nieuw** aan de onderkant van het Machine Learning Studio-venster.
2. Selecteer **GEGEVENSSET** en **vanuit het lokale bestand**.
3. In de **uploaden van een nieuwe gegevensset** dialoogvenster, blader naar het bestand dat u wilt uploaden
4. Voer een naam, het gegevenstype te identificeren en optioneel een beschrijving invoeren. Een beschrijving wordt aanbevolen - Hiermee kunt u voor het vastleggen van alle kenmerken van de gegevens die u dat wilt bij gebruik van de gegevens in de toekomst onthoudt.
5. Het selectievakje **dit is de nieuwe versie van een bestaande gegevensset** kunt u een bestaande gegevensset bijwerken met nieuwe gegevens. Klik op dit selectievakje in en geef vervolgens de naam van een bestaande gegevensset.

![Uploaden van een nieuwe gegevensset](./media/import-data-from-local-file/upload-dataset.png)

Tijdens het uploaden ziet u een bericht dat uw bestand wordt geüpload. Uploaden tijd is afhankelijk van de grootte van uw gegevens en de snelheid van de verbinding met de service. Als u dat het bestand wordt lange tijd duren, kunt u andere dingen in Machine Learning Studio doen terwijl u wacht. Echter, de browser te sluiten zorgt ervoor dat het uploaden van gegevens is mislukt.

## <a name="dataset-module-is-ready-for-use"></a>DataSet-module is gereed voor gebruik
Nadat uw gegevens worden geüpload, wordt opgeslagen in een module gegevensset en is beschikbaar voor alle experimenten in uw werkruimte.

Wanneer u een experiment bewerkt, vindt u de gegevenssets die u hebt gemaakt in de **mijn gegevenssets** lijst onder de **gegevenssets opgeslagen** lijst in het modulepalet. U kunt slepen en neerzetten van de gegevensset naar het experimentcanvas wanneer u wilt de gegevensset gebruiken voor verdere analyse en machine learning.
