---
title: Details van model - aangepaste Translator weergeven
titleSuffix: Azure Cognitive Services
description: Modellen tabblad onder elk project bevat de details van elk model, zoals de modelnaam van het, model de status, BLEU score, training, afstemmen, zin aantal testen.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: v-rada
ms.topic: conceptual
ms.openlocfilehash: 13f3d88ad69d2acc64b9a6469415eceaf22fa491
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57764385"
---
# <a name="view-model-details"></a>Modelgegevens weergeven

Het tabblad modellen onder project bevat alle modellen in dat project. Alle modellen die zijn getraind voor dat project wordt vermeld op dit tabblad.

Voor elk model in het project, worden deze gegevens worden weergegeven.

1.  Modelnaam: Geeft de naam van het model van een bepaald model.

2.  Status: Geeft de status van een bepaald model. Uw nieuwe training heeft een status ingediend totdat het wordt geaccepteerd. De status wordt gewijzigd in gegevensverwerking, terwijl de service beoordeelt de inhoud van uw documenten wat. Wanneer de evaluatie van uw documenten voltooid is wordt de status gewijzigd aan die wordt uitgevoerd en u ziet het aantal zinnen die deel van de training uitmaken, inclusief de afstemmen en tests wordt ingesteld dat automatisch voor u gemaakt. Hieronder volgt een lijst met model de status die de status van de modellen worden beschreven.

    -  Verzonden: Hiermee geeft u op dat de documenten voor dit model wordt verwerkt door de back-end.

    -  TrainingQueued: Hiermee geeft u op dat de training in de wachtrij MT-systeem voor dit model geplaatst is.

    -  Wordt uitgevoerd: Hiermee geeft u op of de training in MT-systeem voor dit model wordt uitgevoerd.

    -  Geslaagd: Hiermee geeft u op dat de training is voltooid in MT-systeem en een model beschikbaar is. In deze status wordt een score BLEU weergegeven voor dit model.

    -  Geïmplementeerd: Hiermee geeft u op dat de geslaagde getrainde model wordt verzonden naar MT-systeem voor de implementatie.

    -  Uitschakeling: Hiermee geeft u op dat de geïmplementeerd model uitschakeling is.

    -  Uitgeschakeld: Hiermee geeft u het proces van de uitschakeling van een model met succes is voltooid.

    -  Training is mislukt: Hiermee geeft u op dat de training is mislukt. Als een training-fout optreedt, voert u de trainingstaak opnieuw uit. Als de fout zich blijft voordoen, neem contact met ons. Verwijder het mislukte model niet.

    - DataProcessingFailed: Hiermee geeft u op dat de verwerking van gegevens is mislukt voor een of meer documenten die behoren tot het model.

    - Implementatie mislukt: Hiermee geeft u op dat de implementatie van het model is mislukt.

    - MigratedDraft: Hiermee geeft u op dat het model heeft de status concept na de migratie van de Hub met aangepaste Translator.

4.  BLEU Score: toont de score BLEU (tweetalige evaluatie Understudy) van het model, de kwaliteit van uw systeem vertaling waarmee wordt aangegeven. Deze score leest u hoe de vertalingen gedaan door de vertaling-systeem die voortvloeien uit deze training overeenkomen met de zinnen verwijzing in de test-gegevensset. De score BLEU wordt weergegeven als training met succes voltooid is. Als training is niet voltooid / mislukt, kunt u een score BLEU niet weergegeven.

5.  Training zin aantal: Totaal aantal geeft aan gebruikt als de trainingset zinnen.

6.  Afstemmen zin aantal: Bevat het totaal aantal zinnen gebruikt als afstemmen instellen.

7.  Training zin aantal: Bevat het totaal aantal zinnen gebruikt als testset.

8.  Mono zin aantal: Bevat het totaal aantal zinnen gebruikt als mono instellen.

9.  Actieknop implementeren: Voor een model is getraind, deze ziet u de knop 'Implementeren' als dit niet geïmplementeerd. Als een model is geïmplementeerd, wordt een knop 'Undeploy' weergegeven.

10. Verwijderen: U kunt deze knop gebruiken als u wilt verwijderen van het model. Verwijderen van een model, niet een van de documenten gebruikt voor het maken van dit model verwijderd.

    ![Modelgegevens weergeven](media/how-to/how-to-view-model-details.png)

>[!Note]
>Als u wilt vergelijken opeenvolgende trainingen voor dezelfde systemen, is het belangrijk is ingesteld dat u de afstemmen en constante testen instellen.

## <a name="view-model-training-details"></a>Details van training model weergeven

Wanneer uw training voltooid is, kunt u meer informatie over de training van de detailpagina te bekijken. Selecteer een project, zoek en selecteer het tabblad modellen en een model kiezen.

De pagina met het model bevat twee tabbladen: Trainingdetails en Test.

1.  **Trainingdetails van de:** Op dit tabblad bevat een lijst van documenten die worden gebruikt in de training:

    -  Naam van de documenten: Dit veld geeft de naam van het document

    -  Document van het Type: Dit veld wordt weergegeven als dit document parallelle / mono.

    -  Aantal zin in source-taal: Dit veld bevat het aantal zinnen zijn er als onderdeel van source-taal.

    -  Aantal zin in doeltaal: Dit veld bevat het aantal zinnen zijn er als onderdeel van de gewenste taal.

    -  Uitgelijnde zinnen: Dit veld bevat het aantal zinnen heeft zijn uitgelijnd met aangepaste Translator tijdens het proces uitlijnen.

    -  Zinnen gebruikt: Dit veld bevat het aantal zinnen is gebruikt door aangepaste Translator tijdens deze training.

    ![Details van model-training](media/how-to/how-to-model-training-details.png)

2.  **Test:** Dit tabblad bevat de details van de test voor een geslaagde training.

## <a name="next-steps"></a>Volgende stappen

- Beoordeling [testresultaten](how-to-view-system-test-results.md) en training resultaten te analyseren.
