---
title: Details van model - aangepaste Translator weergeven
titleSuffix: Azure Cognitive Services
description: Modellen tabblad onder elk project bevat de details van elk model, zoals de modelnaam van het, model de status, BLEU score, training, afstemmen, zin aantal testen.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 80ee99fc4490f1fdc591093f357c8db55a97abb9
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51627386"
---
# <a name="view-model-details"></a>Details van model weergeven

Het tabblad modellen onder project bevat alle modellen in dat project. Alle modellen die zijn getraind voor dat project wordt vermeld op dit tabblad.

Voor elk model in het project, de volgende modeldetails weergegeven.

1.  Modelnaam: Bevat de naam van het model van een bepaald model.

2.  Status: Toont de status van een bepaald model. Uw nieuwe training heeft een status ingediend totdat het wordt geaccepteerd. De status wordt gewijzigd in gegevensverwerking, terwijl de service beoordeelt de inhoud van uw documenten wat. Wanneer de evaluatie van uw documenten voltooid is wordt de status gewijzigd aan die wordt uitgevoerd en u ziet het aantal zinnen die deel van de training uitmaken, inclusief de afstemmen en tests wordt ingesteld dat automatisch voor u gemaakt. Hieronder volgt een lijst met model de status die de status van de modellen worden beschreven.

    -  Ingediend: Geeft aan dat de documenten voor dit model wordt verwerkt door de back-end.

    -  TrainingQueued: Geeft aan dat de training in de wachtrij MT-systeem voor dit model geplaatst is.

    -  Uitvoeren: Geeft aan dat de training in MT-systeem voor dit model wordt uitgevoerd.

    -  Geslaagd: Geeft aan dat de training is voltooid in MT-systeem en een model beschikbaar is. In deze status wordt een score BLEU weergegeven voor dit model.

    -  Geïmplementeerd: Geeft aan dat de geslaagde getrainde model wordt verzonden naar MT-systeem voor de implementatie.

    -  Uitschakeling: Geeft aan dat de geïmplementeerd model uitschakeling is.

    -  Niet-geïmplementeerde: Hiermee geeft u het proces van de uitschakeling van een model met succes is voltooid.

    -  Training is mislukt: Geeft aan dat de training is mislukt. Als een training-fout optreedt, voert u de trainingstaak opnieuw uit. Als de fout zich blijft voordoen, neem contact met ons. Verwijder het mislukte model niet.

    - DataProcessingFailed: Geeft aan dat de verwerking van gegevens is mislukt voor een of meer documenten die behoren tot het model.

    - Implementatie mislukt: Geeft aan dat de implementatie van het model is mislukt.

    - MigratedDraft: Geeft aan dat het model in de conceptfase na de migratie van de Hub met aangepaste Translator.

4.  BLEU Score: toont de score BLEU (tweetalige evaluatie Understudy) van het model, de kwaliteit van uw systeem vertaling waarmee wordt aangegeven. Deze score leest u hoe de vertalingen gedaan door de vertaling-systeem die voortvloeien uit deze training overeenkomen met de zinnen verwijzing in de test-gegevensset. De score BLEU wordt weergegeven als training met succes voltooid is. Als training is niet voltooid / mislukt, kunt u een score BLEU niet weergegeven.

5.  Aantal zin training: totaal aantal zinnen gebruikt als de trainingset bevat.

6.  Aantal zin afstemmen: totaal aantal zinnen gebruikt als het afstemmen van de set bevat.

7.  Aantal zin training: totaal aantal zinnen gebruikt als het testen van de set bevat.

8.  Mono zin aantal: totaal aantal zinnen gebruikt als mono set bevat.

9.  Knop van de actie implementeren: voor een model is getraind, wordt "Implementeren" knop als dat niet geïmplementeerd. Als een model is geïmplementeerd, wordt een knop 'Undeploy' weergegeven.

10. Verwijderen: U kunt deze knop als u wilt verwijderen van het model. Verwijderen van een model, niet een van de documenten gebruikt voor het maken van dit model verwijderd.

    ![Details van model weergeven](media/how-to/how-to-view-model-details.png)

>[!Note]
>Als u wilt vergelijken opeenvolgende trainingen voor dezelfde systemen, is het belangrijk is ingesteld dat u de afstemmen en constante testen instellen.

## <a name="view-model-training-details"></a>Details van training model weergeven

Wanneer uw training voltooid is, kunt u meer informatie over de training van de detailpagina te bekijken. Selecteer een project, zoek en selecteer het tabblad modellen en een model kiezen.

De pagina met het model bevat twee tabbladen: details van de Training en Test.

1.  **Details van training:** op dit tabblad bevat een lijst van documenten die worden gebruikt in de training:

    -  Naam van de documenten: Dit veld geeft de naam van het document

    -  Documenttype: Dit veld geeft aan of dit document parallelle / mono.

    -  Aantal in de brontaal zin: dit veld bevat het aantal zinnen zijn er als onderdeel van source-taal.

    -  Aantal in doeltaal zin: dit veld bevat het aantal zinnen zijn er als onderdeel van de gewenste taal.

    -  Uitgelijnde zinnen: Dit veld bevat het aantal zinnen heeft zijn uitgelijnd met aangepaste Translator tijdens uitlijnen proces.

    -  Gebruikte zinnen: Dit veld bevat het aantal zinnen is gebruikt door aangepaste Translator tijdens deze training.

    ![Details van model-training](media/how-to/how-to-model-training-details.png)

2.  **Test:** op dit tabblad worden de details van de test voor een geslaagde training.

## <a name="next-steps"></a>Volgende stappen

- Beoordeling [testresultaten](how-to-view-system-test-results.md) en training resultaten te analyseren.
