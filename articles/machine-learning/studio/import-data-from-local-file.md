---
title: Gegevens importeren uit bestand
titleSuffix: Azure Machine Learning Studio
description: Informatie over het uploaden van een gegevensbestand training van de harde schijf voor Azure Machine Learning Studio. Hiermee maakt u een gegevensset-module in de werkruimte.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 03/20/2017
ms.openlocfilehash: fe09eabe399af2d3ddc776843f39742774e6ea3c
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55495884"
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-azure-machine-learning-studio"></a>Trainingsgegevens importeren uit een bestand op uw harde schijf in Azure Machine Learning Studio

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
