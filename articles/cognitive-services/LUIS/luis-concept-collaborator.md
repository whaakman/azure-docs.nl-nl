---
title: LUIS app collaboration - Azure begrijpen | Microsoft Docs
description: LUIS apps vereisen één eigenaar en optionele deelnemers.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 51b3958f83cd110ace27f6ee42571c05843f5aa2
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265062"
---
# <a name="collaborating"></a>Samenwerken

LUIS biedt samenwerking zodat een groep personen schrijven van een app.

## <a name="luis-account"></a>LUIS account
Een LUIS-account is gekoppeld aan één [Microsoft Live](https://login.live.com/) account. Elk account LUIS krijgt een gratis [authoring sleutel](luis-concept-keys.md#authoring-key) gebruiken voor alle LUIS apps ontwerpen het account toegang heeft. 

Een account LUIS mogelijk veel LUIS apps.

## <a name="luis-app-owner"></a>LUIS app eigenaar
Het account dat wordt gemaakt van een app is de eigenaar. Elke app heeft één eigenaar. De eigenaar wordt vermeld in de app voor  **[instellingen](luis-how-to-collaborate.md)**. Dit is de account die de app kunt verwijderen. Dit is ook het account dat e-mailbericht ontvangen wanneer het quotum eindpunt 75% van de maandelijkse limiet bereikt. 

## <a name="transfer-ownership"></a>Eigendom overdragen
LUIS biedt geen eigendom, echter een medewerker die de app exporteren en vervolgens een app maken door het te importeren. Houd er rekening mee dat de nieuwe app heeft een andere App-ID. De nieuwe app moet worden getraind, worden gepubliceerd, en het nieuwe eindpunt dat wordt gebruikt.

## <a name="luis-app-collaborators"></a>LUIS app deelnemers
De eigenaar van een app kunt deelnemers toevoegen aan een app. De eigenaar moet de medewerker e-mailadres toevoegen in de app voor  **[instellingen](luis-how-to-collaborate.md)**. De medewerker heeft volledige toegang tot de app. Als de medewerker die de app verwijdert, wordt de app is verwijderd uit de medewerker account maar blijft in het account van de eigenaar. 

Als u meerdere apps delen met uw collega's wilt, moet elke app de medewerker e toegevoegd. 

## <a name="managing-multiple-authors"></a>Het beheren van meerdere auteurs
De [LUIS] [ LUIS] website niet op dit moment biedt voor het transactieniveau ontwerpen. U kunt auteurs om te werken op onafhankelijke versies van een basisversie toestaan. Twee verschillende methoden worden beschreven in de volgende secties.

### <a name="manage-multiple-versions-inside-the-same-app"></a>Beheer van meerdere versies binnen dezelfde app
Beginnen met het [klonen](luis-how-to-manage-versions.md#clone-a-version), van een basisversie voor elke auteur. 

Elke auteur brengt wijzigingen aan zijn of haar eigen versie van de app. Wanneer elke auteur tevreden met het model is, moet u de nieuwe versies exporteren naar JSON-bestanden.  

Geëxporteerde apps zijn JSON-indeling bestanden, die kunnen worden vergeleken op wijzigingen. De bestanden voor het maken van een enkele JSON-bestand van de nieuwe versie combineren. Wijzig de **versionId** eigenschap in de JSON om de nieuwe versie van de samengevoegde aan te geven. Deze versie in de oorspronkelijke app importeren. 

Deze methode kunt u een actieve versie, één fase-versie en een gepubliceerde versie. U kunt de resultaten in het deelvenster voor interactieve testen in alle drie versies vergelijken.

### <a name="manage-multiple-versions-as-apps"></a>Meerdere versies als apps beheren
[Exporteren](luis-how-to-manage-versions.md#export-version) de basisversie. Elke auteur importeert de versie. De persoon die de app importeert, is de eigenaar van de versie. Wanneer ze klaar bent de versie wijzigen van de app exporteren. 

Geëxporteerde apps zijn JSON-indeling bestanden, die kunnen worden vergeleken met de base exporteren voor wijzigingen. De bestanden voor het maken van een enkele JSON-bestand van de nieuwe versie combineren. Wijzig de **versionId** eigenschap in de JSON om de nieuwe versie van de samengevoegde aan te geven. Deze versie in de oorspronkelijke app importeren.

## <a name="next-steps"></a>Volgende stappen

Begrijpen [versioning](luis-concept-version.md) concepten. 

Zie [Appinstellingen](luis-how-to-collaborate.md) voor informatie over het beheren van de deelnemers in uw app LUIS.

Zie [e-mailadres toevoegen aan lijst](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58fcccdd5aca2f08a4104342) met de API's voor ontwerpen.

[luis-reference-prebuilt-domains]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-domains
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website