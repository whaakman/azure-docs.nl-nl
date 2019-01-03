---
title: De resultaten van systeem en implementatie - aangepaste Translator weergeven
titleSuffix: Azure Cognitive Services
description: Wanneer uw training is voltooid, controleert u systeemtests voor het analyseren van de resultaten van uw training. Als u tevreden met de resultaten van de training bent, plaatst u een implementatieaanvraag voor het getrainde model.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 701f97a006e7457fcbdcc835f87832b269f986a8
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53630339"
---
# <a name="view-system-test-results"></a>Testresultaten voor systeem weergeven

Wanneer uw training is voltooid, controleert u systeemtests voor het analyseren van de resultaten van uw training. Als u tevreden met de resultaten van de training bent, plaatst u een implementatieaanvraag voor het getrainde model. 

## <a name="system-test-results-page"></a>Pagina met zoekresultaten systeem testen

Selecteer een project en selecteer vervolgens het tabblad modellen van het project, zoekt u het model dat u wilt gebruiken en tot slot selecteert u het tabblad testen.

De test-tabblad ziet u:

1.  **De resultaten van systeem:** het resultaat van het testproces in de trainingen. Het testproces produceert de score BLEU.

    **Aantal zin:** Het aantal parallelle zinnen zijn gebruikt in de testset.

     **BLEU Score:** BLEU score gegenereerd voor een model na voltooiing van de training.

    **Status:** Geeft aan of de test voltooid is of wordt uitgevoerd.

    ![De resultaten van systeem](media/how-to/how-to-system-test-results.png)

2.  Klik op de resultaten van het systeem en dat gaat u naar de pagina met details van resultaat testen. Deze pagina bevat het vertalen van zinnen die deel van de testgegevensset uitmaken.

3.  De tabel op de detailpagina van de test-resultaat heeft twee kolommen: één voor elke taal in het paar. De kolom voor de source-taal geeft de zin worden omgezet. De kolom voor de doeltaal bevat twee zinnen in elke rij.

    **REF:** Deze zin is de omzetting van de referentie van de bron-zin zoals vermeld in de testgegevensset.

    **MT:** Deze zin is de automatische omzetting van de bron-zin gedaan door het model gebouwd na de training werd uitgevoerd.

    ![Systeem test resultaten vergelijken](media/how-to/how-to-system-test-results-2.png)

## <a name="download-test"></a>Test downloaden

Klik op de koppeling vertalingen downloaden als een zipbestand wilt downloaden. Het ZIP-bestand bevat de machine vertalingen van bron zinnen in de test-gegevensset.

![Test downloaden](media/how-to/how-to-system-test-download.png)

Deze gedownloade zip-archief bevat drie bestanden.

1.  **Custom.MT.txt:** Dit bestand bevat automatische vertalingen van zinnen source-taal in de doeltaal gedaan door het model is getraind met gegevens van de gebruiker.

2.  **Ref.txt:** Dit bestand bevat door de gebruiker opgegeven vertalingen van zinnen source-taal in de doel-taal.

3.  **Source.txt:** Dit bestand bevat zinnen in de source-taal.

    ![De resultaten van gedownloade system](media/how-to/how-to-download-system-test.png)

## <a name="deploy-a-model"></a>Een model implementeren

Om aan te vragen een implementatie:

1.  Selecteer een project, gaat u naar het tabblad modellen.

2. Voor een model is getraind, deze ziet u de knop 'Implementeren' als dit niet geïmplementeerd.

    ![Model implementeren](media/how-to/how-to-deploy-model.png)

3.  Klik op implementeren.
4.  Selecteer **geïmplementeerd** voor de regio('s) waar u uw model om te worden geïmplementeerd en klik op opslaan. U kunt selecteren **geïmplementeerd** voor meerdere regio's.

    ![Model implementeren](media/how-to/how-to-deploy-model-regions.png)

5.  U kunt de status van uw model bekijken in de kolom 'Status'.

## <a name="update-deployment-settings"></a>Implementatie-instellingen bijwerken

Implementatie-instellingen bijwerken:

1.  Selecteer een project en Ga naar de **modellen** tabblad.

2. Voor een model is geïmplementeerd, wordt een **Update** knop.

    ![Model implementeren](media/how-to/how-to-update-undeploy-model.png)

3.  Selecteer **Update**.
4.  Selecteer **geïmplementeerd** of **Undeployed** van de regio('s) waar u uw model geïmplementeerd of uitgeschakeld, en klik op **opslaan**.

    ![Model implementeren](media/how-to/how-to-undeploy-model.png)

>[!Note]
>Als u selecteert **Undeployed** voor alle regio's, het model is uitgeschakeld in alle regio's, en in een niet-geïmplementeerde status geplaatst. Het is nu niet beschikbaar voor gebruik.

## <a name="next-steps"></a>Volgende stappen

- Start met behulp van het model van uw geïmplementeerde aangepaste vertaling via [Microsoft Translator Text-API V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).
- Informatie over [over het beheren van instellingen](how-to-manage-settings.md) beheren abonnementssleutel voor het delen van uw werkruimte.
- Informatie over [over het migreren van uw werkruimte en het project](how-to-migrate.md) van [Microsoft Translator-Hub](https://hub.microsofttranslator.com)