---
title: 'Gegevens importeren uit bestand: Azure Machine Learning Studio | Microsoft Docs'
description: Informatie over het uploaden van een gegevensbestand training van de harde schijf voor Azure Machine Learning Studio. Hiermee maakt u een gegevensset-module in de werkruimte.
keywords: gegevens, de indeling, gegevenstypen, gegevensbronnen, trainingsgegevens importeren
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: c0dd9e90-23c4-4f64-8b8f-489ad79f047b
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: 77f08bd81b9b0cefc0bb956f38856bcb6d0ad572
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53093383"
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a>Trainingsgegevens importeren uit een bestand op uw harde schijf in Machine Learning Studio

Informatie over het uploaden van een gegevensbestand van de harde schijf om te gebruiken als het trainen van gegevens in Azure Machine Learning Studio. Door het importeren van het gegevensbestand, hebt u een module gegevensset klaar voor gebruik in uw werkruimte.

## <a name="steps-to-import-data-from-a-local-file"></a>Stappen voor het importeren van gegevens uit een lokaal bestand
Als u wilt gegevens importeren uit een lokale vaste schijf, het volgende doen:

1. Klik op **+ nieuw** aan de onderkant van het venster Machine Learning Studio.
2. Selecteer **GEGEVENSSET** en **vanuit het lokale bestand**.
3. In de **uploaden van een nieuwe gegevensset** dialoogvenster, blader naar het bestand dat u wilt uploaden
4. Voer een naam, het gegevenstype te identificeren en optioneel een beschrijving invoeren. Een beschrijving wordt aanbevolen: Hiermee kunt u om vast te leggen van alle kenmerken van de gegevens die u wilt Houd er rekening mee bij het gebruik van de gegevens in de toekomst.
5. Het selectievakje **dit is de nieuwe versie van een bestaande gegevensset** kunt u een bestaande gegevensset bijwerken met nieuwe gegevens. Klik op dit selectievakje in en geef vervolgens de naam van een bestaande gegevensset.

![Upload een nieuwe gegevensset](./media/import-data-from-local-file/upload-dataset.png)

Tijdens het uploaden ziet u een bericht dat uw bestand wordt geüpload. Uploaden tijd is afhankelijk van de grootte van uw gegevens en de snelheid van de verbinding met de service. Als u weet dat het bestand wordt een lange tijd in beslag nemen, kunt u andere dingen in Machine Learning Studio doen terwijl u wacht. Echter, de browser te sluiten zorgt ervoor dat het uploaden van gegevens mislukt.

## <a name="dataset-module-is-ready-for-use"></a>Module van de gegevensset is gereed voor gebruik
Nadat uw gegevens is geüpload, wordt opgeslagen in een gegevensset-module en is beschikbaar voor een experiment in uw werkruimte.

Wanneer u een experiment bewerkt, vindt u de gegevenssets die u hebt gemaakt in de **mijn gegevenssets** lijst onder de **opgeslagen gegevenssets** lijst in het modulepalet. U kunt slepen en neerzetten van de gegevensset naar het experimentcanvas wanneer u wilt gebruiken van de gegevensset voor verdere analyse en machine learning.
