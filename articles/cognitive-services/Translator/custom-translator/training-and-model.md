---
title: Wat is training en het model? -Aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Een model is het systeem, waarmee u vertaling voor een paar specifieke taal. Het resultaat van een geslaagde training is een model. Bij het trainen van een model, zijn drie sluiten elkaar wederzijds uit gegevenssets vereist gegevensset training, afstemmen van de gegevensset en het testen van de gegevensset.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: v-rada
ms.openlocfilehash: bd4921cb959e3ea2e893c6837fb47792d3585ca9
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220124"
---
# <a name="what-are-trainings-and-models"></a>Wat zijn trainingen en -modellen?

Een model is het systeem, waarmee u vertaling voor een paar specifieke taal.
Het resultaat van een geslaagde training is een model. Bij het trainen van een model, drie sluiten elkaar wederzijds uit gegevenssets zijn vereist: training gegevensset, afstemmen van de gegevensset en het testen van de gegevensset. Woordenlijst gegevens kunnen ook worden opgegeven.

Als er slechts trainingsgegevens wordt opgegeven als een training queuing, samen aangepaste Translator automatisch afstemmen- en testsets. Hiermee worden 5000 zinnen uitsluiten van de trainingsgegevens en gebruikt 2.500 elke voor het samenstellen van een afstemmen en tests wordt ingesteld.

## <a name="training-dataset-for-custom-translator"></a>Training-gegevensset voor aangepaste Translator

Documenten die zijn opgenomen in de trainingset worden gebruikt door de aangepaste Translator als basis voor het bouwen van uw model. Tijdens het uitvoeren van de training, worden zinnen die aanwezig in deze documenten zijn uitgelijnd (of gekoppeld). U kunt vrijheden bij het schrijven van de verzameling van Trainingsdocumenten op te nemen. U kunt opnemen documenten die u denkt dat ze zijn van raakt relevantie in één model. Sluit u deze opnieuw in de andere het effect in [BLEU (tweetalige evaluatie Understudy) score](what-is-bleu-score.md). Als u de afstemmen set en test set constante wilt behouden, gerust om te experimenteren met de samenstelling van de trainingsset. Deze aanpak is een effectieve manier om te wijzigen van de kwaliteit van uw vertaalsysteem.

U kunt uitvoeren van meerdere trainingen in een project en vergelijk de [BLEU scores](what-is-bleu-score.md) voor alle cursussen uitvoert. Wanneer u meerdere trainingen voor vergelijking worden uitgevoerd, zorg ervoor dat dezelfde afstemmen / testgegevens telkens is opgegeven. Controleer ook of te inspecteren ook de resultaten handmatig in de ['Test'](how-to-view-system-test-results.md) tabblad.

## <a name="tuning-dataset-for-custom-translator"></a>Gegevensset voor aangepaste Translator afstemmen

Parallelle documenten die zijn opgenomen in deze groep worden gebruikt door de aangepaste Translator om af te stemmen van het systeem vertaling voor optimale resultaten.

De afstemmen set wordt gebruikt tijdens de training alle parameters en het gewicht van het systeem converteren naar de optimale waarden aan te passen. Kies uw afstemmen moet zorgvuldig instellen: de afstemmen set moet een vertegenwoordiger van de inhoud van de documenten die u van plan bent om te vertalen in de toekomst. De afstemmen set heeft een grote invloed hebben op de kwaliteit van de vertaling die wordt geproduceerd. Afstemmen, kunt het systeem vertaling voor de vertaling op die zich het dichtst bij de voorbeelden die u in de gegevensset afstemmen opgeeft. U hoeft niet meer dan 2500 zinnen als set afstemmen. Voor optimale vertaalkwaliteit, is het aanbevolen om te selecteren van de afstemmen set handmatig door het kiezen van de meest vooraanstaande selectie van zinnen.

Bij het maken van uw afstemmen instellen, kiest u zinnen die een betekenis en representatieve lengte van de toekomstige zinnen die u verwacht te vertalen. U moet ook zinnen met woorden en zinnen die u van plan bent om te vertalen in de geschatte distributie die u in de toekomst vertalingen verwacht kiezen. In de praktijk, wordt een zinlengte van 8 tot en met 18 woorden de beste resultaten produceren omdat deze zinnen voldoende context om te laten zien belangrijk en bieden van een woordgroep lengte die belangrijk is, zonder dat erg complex bevatten.

Een goede beschrijving van het type van zinnen om te gebruiken in de afstemmen set is prose: werkelijke fluent zinnen. Geen cellen in de tabel, niet gedichten, niet een lijst met dingen, niet alleen interpunctie of getallen in een zin - reguliere taal.

Als u handmatig uw afstemmen gegevensset selecteert, moet er een van de dezelfde zinnen als uw gegevens training en testen. De afstemmen set heeft een aanzienlijke invloed op de kwaliteit van de vertalingen - zinnen zorgvuldig te kiezen.

Als u niet zeker weet wat u moet kiezen voor de verzameling afstemmen, selecteer de training en aangepaste Translator Selecteer uw afstemmen voor u laten. Wanneer u de aangepaste Translator automatisch kiezen van de afstemmen set brengt, wordt het gebruik van een willekeurige subset van zinnen uit uw Trainingsdocumenten tweetalige en deze zinnen uitsluiten van het trainingsmateriaal zelf.

## <a name="testing-dataset-for-custom-translator"></a>Gegevensset voor aangepaste Translator testen

Parallelle documenten die zijn opgenomen in de set met tests worden gebruikt voor het berekenen van de score BLEU (tweetalige evaluatie Understudy). Deze score geeft aan dat de kwaliteit van uw vertaalsysteem. Deze score daadwerkelijk kunt u zien hoe de vertalingen gedaan door de vertaling-systeem die voortvloeien uit deze training overeenkomen met de zinnen verwijzing in de test-gegevensset.

De score BLEU is een meting van de verschillen tussen de automatische vertaling en de omzetting van de referentie. De waarde varieert van 0 tot 100. Een score van 0 geeft aan dat één woord van de verwijzing wordt weergegeven in de vertaling. Een score van 100 geeft aan dat de automatische vertaling exact overeenkomt met de verwijzing: hetzelfde woord is op exact dezelfde positie. De score is het gemiddelde van de score BLEU voor alle zinnen van testset.

De test moet parallelle documenten waar de doeltaal zinnen de meest wenselijk vertalingen van de bijbehorende zinnen source-taal in het paar zijn bevat. U wilt gebruiken dezelfde criteria die u hebt gebruikt voor het opstellen van de set afstemmen. Testset heeft echter geen invloed op de kwaliteit van de NAT-systeem. Exclusief voor het genereren van de score BLEU voor u en niets anders wordt gebruikt.

U hoeft niet meer dan 2500 zinnen als testset. Wanneer u het systeem automatisch de testset kiezen brengt, wordt het gebruik van een willekeurige subset van zinnen uit uw Trainingsdocumenten tweetalige en deze zinnen uitsluiten van het trainingsmateriaal zelf.

U kunt de aangepaste vertalingen van testset weergeven en deze vergelijken met de opgegeven in uw testset door te navigeren naar het tabblad testen in een model vertalingen.
