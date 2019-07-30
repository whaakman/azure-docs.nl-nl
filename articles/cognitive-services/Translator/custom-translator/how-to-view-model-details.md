---
title: Model details weer geven-aangepaste vertaler
titleSuffix: Azure Cognitive Services
description: Op het tabblad modellen onder een project ziet u details van elk model, zoals model naam, model status, BLEU Score, training, afstemming, aantal test zinnen.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 64f446c3b331c1aa6ddaae9081b7f61943f74ab2
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595563"
---
# <a name="view-model-details"></a>Modelgegevens weergeven

Op het tabblad modellen onder project worden alle modellen in dat project weer gegeven. Alle modellen die zijn getraind voor dat project, worden op dit tabblad weer gegeven.

Deze gegevens worden weer gegeven voor elk model in het project.

1.  Model naam: Hier wordt de model naam van een bepaald model weer gegeven.

2.  Status: Hiermee wordt de status van een bepaald model weer gegeven. Uw nieuwe training heeft de status verzonden tot deze is geaccepteerd. De status wordt gewijzigd in gegevens verwerking terwijl de inhoud van uw documenten door de service wordt geëvalueerd. Wanneer de evaluatie van uw documenten is voltooid, wordt de status gewijzigd in actief. u kunt nu het aantal zinnen zien die deel uitmaken van de training, inclusief de afstemmings-en test sets die voor u worden gemaakt. Hieronder ziet u een lijst met model status die de status van de modellen beschrijft.

    -  Gestelde Hiermee geeft u op dat de back-end de documenten voor dat model verwerkt.

    -  TrainingQueued: Hiermee geeft u op dat de training in een wachtrij wordt geplaatst voor het MT-systeem voor dat model.

    -  Wordt uitgevoerd: Hiermee geeft u op dat de training wordt uitgevoerd in het MT-systeem voor dat model.

    -  Is voltooid Hiermee geeft u op dat de training is geslaagd in MT-systeem en dat er een model beschikbaar is. In deze status wordt een BLEU-score voor dat model weer gegeven.

    -  Geïmplementeerd Hiermee geeft u op dat het voltooide getrainde model wordt verzonden naar het MT-systeem voor implementatie.

    -  Implementatie ongeïmplementeerd: Hiermee geeft u op dat het geïmplementeerde model niet kan worden geïmplementeerd.

    -  Geïmplementeerde Hiermee geeft u op dat het installatie proces van een model is voltooid.

    -  Training mislukt: Hiermee geeft u op dat de training is mislukt. Als er een trainings fout optreedt, voert u de trainings taak opnieuw uit. Als de fout zich blijft voordoen, neemt u contact met ons op. Verwijder het mislukte model niet.

    - DataProcessingFailed: Hiermee geeft u op dat het verwerken van gegevens is mislukt voor een of meer documenten die tot het model behoren.

    - Heeft Hiermee geeft u op dat de implementatie van het model is mislukt.

    - MigratedDraft: Hiermee geeft u op dat het model de status concept heeft na de migratie van de hub naar het aangepaste conversie programma.

4.  BLEU Score: toont de Score van de BLEU (tweetalige evaluatie) van het model, wat de kwaliteit van uw Vertaal systeem aangeeft. Deze score vertelt u hoe nauw keurig de vertalingen die door het Vertaal systeem worden uitgevoerd en die het resultaat zijn van deze training overeenkomen met de referentie zinnen in de set test data. De BLEU-score wordt weer gegeven als de training is voltooid. Als training niet is voltooid of mislukt, ziet u geen BLEU Score.

5.  Aantal trainings zinnen: Toont het totale aantal zinnen dat als Trainingsset wordt gebruikt.

6.  Aantal zinnen afstemmen: Geeft het totale aantal zinnen weer dat wordt gebruikt als afstemmings.

7.  Aantal trainings zinnen: Geeft het totale aantal zinnen weer dat wordt gebruikt als testset.

8.  Aantal mono-zinnen: Geeft het totale aantal zinnen weer dat wordt gebruikt als mono-set.

9.  Actie knop implementeren: Voor een goed opgeleid model wordt de knop implementeren weer gegeven als deze niet is geïmplementeerd. Als er een model wordt geïmplementeerd, wordt de knop ' de implementatie opzeggen ' weer gegeven.

10. Verwijderd U kunt deze knop gebruiken als u het model wilt verwijderen. Als u een model verwijdert, worden de documenten waarmee het model is gemaakt, niet verwijderd.

    ![Modelgegevens weergeven](media/how-to/how-to-view-model-details.png)

>[!Note]
>Als u opeenvolgende trainingen voor dezelfde systemen wilt vergelijken, is het belang rijk om de afstemmings en de ingestelde constante te testen.

## <a name="view-model-training-details"></a>Details van model training weer geven

Wanneer uw training is voltooid, kunt u details over de training bekijken op de pagina Details. Selecteer een project, zoek en selecteer het tabblad modellen en kies een model.

De model pagina heeft twee tabbladen: Trainings Details en testen.

1.  **Cursus Details:** Dit tabblad bevat de lijst met documenten die in de training worden gebruikt:

    -  Naam van documenten: Dit veld bevat de naam van het document

    -  Document type: Dit veld geeft aan of dit document parallel/mono is.

    -  Aantal zinnen in bron taal: In dit veld wordt het aantal zinnen weer gegeven als onderdeel van de bron taal.

    -  Aantal zinnen in doel taal: In dit veld wordt het aantal zinnen weer gegeven als onderdeel van de doel taal.

    -  Uitgelijnde zinnen: In dit veld wordt het aantal zinnen weer gegeven dat is afgestemd op het aangepaste conversie programma tijdens het uitlijnen.

    -  Gebruikte zinnen: In dit veld wordt het aantal zinnen weer gegeven dat is gebruikt door aangepaste Translator tijdens deze training.

    ![Model trainings Details](media/how-to/how-to-model-training-details.png)

2.  **Proeven** Dit tabblad bevat de test gegevens voor een geslaagde training.

## <a name="next-steps"></a>Volgende stappen

- Bekijk de [test resultaten](how-to-view-system-test-results.md) en analyseer de resultaten van de training.
