---
title: Over het uploaden van een document - aangepaste Translator
titleSuffix: Azure Cognitive Services
description: U kunt met behulp van de functie voor uploaden parallelle document uploaden voor uw trainingen. Parallelle documenten bestaan uit paren van documenten is, de vertaling van de andere. Een document in het paar zinnen in de source-taal bevat en het andere document bevat deze zinnen vertaald naar de doel-taal.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.date: 02/21/2019
ms.author: v-rada
ms.topic: conceptual
ms.openlocfilehash: 83d6791740cd6aebb9da14b86e0d5272ab78f43a
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737583"
---
# <a name="upload-a-document"></a>Een document uploaden

In [aangepaste Translator](https://portal.customtranslator.azure.ai), kunt u uw omzettingsmodellen van de trein parallelle documenten uploaden. [Parallelle documenten](what-are-parallel-documents.md) zijn paren van documenten, een vertaling van de andere is. Een document in het paar zinnen in de source-taal bevat en het andere document bevat deze zinnen vertaald naar de doel-taal.

Bekijk voordat u uw documenten te uploaden, de [indelingen en naming convention richtlijnen](document-formats-naming-convention.md) om te controleren of het bestand-indeling wordt ondersteund in aangepaste Translator.

## <a name="how-to-upload-document"></a>Het document uploaden?

Van [aangepaste Translator](https://portal.customtranslator.azure.ai) portal, klik op het tabblad "Documenten" naar documenten pagina te gaan.

![Koppeling naar document uploaden](media/how-to/how-to-upload-1.png)


1.  Klik op de knop voor uploaden van bestanden op de pagina documenten.

    ![Documentpagina uploaden](media/how-to/how-to-upload-2.png)

2.  Op de opvulling van het dialoogvenster in de volgende informatie:

    a.  Document van het type:

    -  Training: Deze documenten wordt gebruikt voor de trainingset.
    -  Tuning: Deze documenten wordt gebruikt voor het afstemmen van de set.
    -  Testen: Deze documenten wordt gebruikt voor het testen van de set.
    -  Woordgroep woordenboek: Deze documenten wordt gebruikt voor de zinsnede woordenlijst.
    -  Woordenlijst voor zin: Deze documenten worden gebruikt voor de zin woordenlijst

    b.  Combinatie van taal

    c.  Document overschrijven als bestaat: Schakel dit selectievakje in als u wilt overschrijven van bestaande documenten met dezelfde naam.

    d.  Vul in de betreffende sectie voor parallelle gegevens of gegevens van de keuzelijst met invoervak.

    -  Parallelle gegevens:
        -  Bronbestand: Selecteer source-taalbestand vanaf uw lokale computer.
        -  Doelbestand: Selecteer doeltaal bestand vanaf uw lokale computer.
        -  Naam van het document: Alleen gebruikt als u parallelle bestanden uploadt.

    - Keuzelijst met invoervak voor gegevens:
        -  Keuzelijst met invoervak voor bestand: Selecteer het bestand keuzelijst met invoervak voor vanaf uw lokale computer. Het bestand keuzelijst met invoervak voor bevat zowel de bron- en doeltaal zinnen. [Naamgevingsconventie](document-formats-naming-convention.md) is belangrijk voor de keuzelijst met invoervak voor bestanden.

    e.  Klik op uploaden

    ![Dialoogvenster document uploaden](media/how-to/how-to-upload-dialog.png)

3.  Op dit moment bent Wij verwerken van uw documenten en er wordt geprobeerd om op te halen van zinnen. U kunt klikken op 'Weergave uploadvoortgang' om te controleren of de status van uw documenten als ze worden verwerkt.

    ![Dialoogvenster van de verwerking van document uploaden](media/how-to/how-to-upload-processing-dialog.png)

4.  Deze pagina wordt weergegeven de status en eventuele fouten voor elk bestand in uw uploaden. U kunt op elk gewenst moment voorbij Uploadstatus weergeven door te klikken op het tabblad 'Geschiedenis uploaden'.

    ![Dialoogvenster van de geschiedenis document uploaden](media/how-to/how-to-upload-document-history.png)


## <a name="view-upload-history"></a>Upload-geschiedenis weergeven

In de geschiedenis van uploadpagina vindt u de geschiedenis van alle document uploaden van gegevens, zoals uploaden documenttype, een combinatie van taal, status, enzovoort.

1. Uit de [aangepaste Translator](https://portal.customtranslator.azure.ai) portal, klik op tabblad Geschiedenis uploaden om geschiedenis weer te geven.

    ![Tabblad Geschiedenis uploaden](media/how-to/how-to-upload-history-1.png)

2. Deze pagina bevat de status van al uw afgelopen uploads. Uploaden van bestanden wordt weergegeven in de meest recente tot de minst recente. Voor elke uploaden wordt de naam van het document, Uploadstatus, de datum uploaden, het aantal bestanden die zijn geüpload, type bestand geüpload en de combinatie van taal van het bestand.

    ![Geschiedenispagina uploaden](media/how-to/how-to-document-history-2.png)

3. Klik op een geschiedenisrecord uploaden. In de uploadpagina geschiedenis details, kunt u de bestanden die zijn geüpload als onderdeel van het uploaden, geüploade status van het bestand, de taal van het bestand en de fout bericht (als er een fout is opgetreden in uploaden) weergeven.

## <a name="next-steps"></a>Volgende stappen

- Gebruik de [pagina met details van document](how-to-view-document-details.md) lijst uitgepakte zinnen wilt bekijken.
- [Hoe u een model te trainen](how-to-train-model.md).
