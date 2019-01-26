---
title: Samenwerking
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS-apps moeten de eigenaar van een enkele en optionele deelnemers toestaan dat meerdere mensen schrijven voor één app.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 4b886dd3b2ecbdcd481eb3ee1d6fd8e1b270f479
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54910762"
---
# <a name="collaborating-with-other-authors"></a>Samenwerken met andere auteurs

LUIS biedt samenwerking om toe te staan een groep personen op het ontwerpen van een app.

## <a name="luis-account"></a>LUIS-account
Een LUIS-account is gekoppeld aan één [Microsoft Live](https://login.live.com/) account. Elk account LUIS krijgt een gratis [ontwerpen sleutel](luis-concept-keys.md#authoring-key) te gebruiken voor het ontwerpen van alle LUIS-apps die het account toegang heeft tot. 

Een LUIS-account mogelijk veel LUIS-apps.

Zie [Azure Active Directory-tenant gebruiker](luis-how-to-collaborate.md#azure-active-directory-tenant-user) voor meer informatie over Active Directory-gebruikersaccounts. 

## <a name="luis-app-owner"></a>LUIS-app-eigenaar
Het account dat wordt gemaakt van een app is eigenaar van de. Elke app heeft één eigenaar. De eigenaar wordt vermeld in de app voor  **[instellingen](luis-how-to-collaborate.md)**. Dit is het account dat de app kunt verwijderen. Dit is ook het account dat e-mailbericht ontvangt wanneer het quotum eindpunt 75% van de maandelijkse limiet is bereikt. 

## <a name="authorization-roles"></a>Autorisatie-rollen
LUIS biedt geen ondersteuning voor verschillende rollen voor eigenaren en samenwerkers met één uitzondering. De eigenaar is het enige account dat de app kunt verwijderen.

Als u geïnteresseerd bent in het beheren van toegang tot het model, kunt u overwegen segmentering van het model in kleinere LUIS-apps, waarbij elke kleinere app een beperkte set van deelnemers heeft. Gebruik [verzending](https://aka.ms/dispatch-tool) om toe te staan een bovenliggende LUIS-app voor het beheren van de coördinatie tussen bovenliggende en onderliggende apps.

## <a name="transfer-ownership"></a>Eigendom overdragen
LUIS biedt geen overdracht van eigendom, maar samenwerker kunt exporteren van de app en maak vervolgens een app door het te importeren. Houd er rekening mee dat de nieuwe app heeft een andere App-ID. De nieuwe app moet worden getraind, gepubliceerd, en het nieuwe eindpunt wordt gebruikt.

## <a name="luis-app-collaborators"></a>LUIS-app deelnemers
Eigenaar van een app kunt deelnemers toevoegen aan een app. De eigenaar moet van de samenwerker e-mailadres toevoegen aan app  **[instellingen](luis-how-to-collaborate.md)**. De samenwerker heeft volledige toegang tot de app. Als de samenwerker Hiermee verwijdert u de app, wordt de app wordt verwijderd uit de account van de samenwerker maar blijft in het account van de eigenaar. 

Als u meerdere apps delen met uw collega's wilt, moet elke app van de samenwerker e-mailbericht toegevoegd. 

## <a name="managing-multiple-authors"></a>Beheer van meerdere auteurs
De [LUIS](luis-reference-regions.md#luis-website) website niet op dit moment biedt voor het transactieniveau ontwerpen. Auteurs kunnen werken op onafhankelijke versies van een basisversie kunt u toestaan. Twee verschillende methoden worden beschreven in de volgende secties.

## <a name="manage-multiple-versions-inside-the-same-app"></a>Meerdere versies binnen dezelfde app beheren
Beginnen met het [klonen](luis-how-to-manage-versions.md#clone-a-version), van een basisversie voor elke auteur maken. 

Elke auteur brengt wijzigingen aan zijn of haar eigen versie van de app. Wanneer elke auteur maken door het model is, moet u de nieuwe versies exporteren naar JSON-bestanden.  

Geëxporteerde apps zijn JSON-indeling bestanden, die wijzigingen kunnen worden vergeleken. Combineer de bestanden voor het maken van een enkel JSON-bestand van de nieuwe versie. Wijzig de **versionId** eigenschap in de JSON om de nieuwe versie van de samengevoegde aan te geven. Die versie importeren in de oorspronkelijke app. 

Deze methode kunt u een actieve versie, één fase-versie en een gepubliceerde versie hebben. U kunt de resultaten in het deelvenster met interactieve testen voor de drie versies vergelijken.

## <a name="manage-multiple-versions-as-apps"></a>Beheer van meerdere versies als apps
[Exporteren](luis-how-to-manage-versions.md#export-version) de basisversie. Elke auteur importeert de-versie. De persoon die de invoer van de app is de eigenaar van de versie. Wanneer ze klaar zijn voor het wijzigen van de app, de versie exporteren. 

Geëxporteerde apps zijn JSON-indeling bestanden, die kunnen worden vergeleken met de basis exporteren voor wijzigingen. Combineer de bestanden voor het maken van een enkel JSON-bestand van de nieuwe versie. Wijzig de **versionId** eigenschap in de JSON om de nieuwe versie van de samengevoegde aan te geven. Die versie importeren in de oorspronkelijke app.

## <a name="next-steps"></a>Volgende stappen

Inzicht in [versiebeheer](luis-concept-version.md) concepten. 

Zie [App-instellingen](luis-how-to-collaborate.md) voor informatie over het beheren van deelnemers in uw LUIS-app.

Zie [e-mailadres toevoegen aan de toegangslijst](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58fcccdd5aca2f08a4104342) met de API's voor ontwerpen.