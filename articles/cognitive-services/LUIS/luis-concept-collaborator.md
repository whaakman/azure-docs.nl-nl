---
title: Inzicht in samenwerking in de LUIS-app - Azure | Microsoft Docs
description: LUIS-apps moeten de eigenaar van een enkele en optionele deelnemers.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: 684507b58e5ac766e9b42d3aa53cb0867a131a93
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222669"
---
# <a name="collaborating"></a>Samenwerken

LUIS biedt samenwerking om toe te staan een groep personen op het ontwerpen van een app.

## <a name="luis-account"></a>LUIS-account
Een LUIS-account is gekoppeld aan één [Microsoft Live](https://login.live.com/) account. Elk account LUIS krijgt een gratis [ontwerpen sleutel](luis-concept-keys.md#authoring-key) te gebruiken voor het ontwerpen van alle LUIS-apps die het account toegang heeft tot. 

Een LUIS-account mogelijk veel LUIS-apps.

Zie [Azure Active Directory-tenant gebruiker](luis-how-to-account-settings.md#azure-active-directory-tenant-user) voor meer informatie over Active Directory-gebruikersaccounts. 

## <a name="luis-app-owner"></a>LUIS-app-eigenaar
Het account dat wordt gemaakt van een app is eigenaar van de. Elke app heeft één eigenaar. De eigenaar wordt vermeld in de app voor  **[instellingen](luis-how-to-collaborate.md)**. Dit is het account dat de app kunt verwijderen. Dit is ook het account dat e-mailbericht ontvangt wanneer het quotum eindpunt 75% van de maandelijkse limiet is bereikt. 

## <a name="transfer-ownership"></a>Eigendom overdragen
LUIS biedt geen overdracht van eigendom, maar samenwerker kunt exporteren van de app en maak vervolgens een app door het te importeren. Houd er rekening mee dat de nieuwe app heeft een andere App-ID. De nieuwe app moet worden getraind, gepubliceerd, en het nieuwe eindpunt wordt gebruikt.

## <a name="luis-app-collaborators"></a>LUIS-app deelnemers
Eigenaar van een app kunt deelnemers toevoegen aan een app. De eigenaar moet van de samenwerker e-mailadres toevoegen aan app  **[instellingen](luis-how-to-collaborate.md)**. De samenwerker heeft volledige toegang tot de app. Als de samenwerker Hiermee verwijdert u de app, wordt de app wordt verwijderd uit de account van de samenwerker maar blijft in het account van de eigenaar. 

Als u meerdere apps delen met uw collega's wilt, moet elke app van de samenwerker e-mailbericht toegevoegd. 

## <a name="managing-multiple-authors"></a>Beheer van meerdere auteurs
De [LUIS](luis-reference-regions.md#luis-website) website niet op dit moment biedt voor het transactieniveau ontwerpen. Auteurs kunnen werken op onafhankelijke versies van een basisversie kunt u toestaan. Twee verschillende methoden worden beschreven in de volgende secties.

### <a name="manage-multiple-versions-inside-the-same-app"></a>Meerdere versies binnen dezelfde app beheren
Beginnen met het [klonen](luis-how-to-manage-versions.md#clone-a-version), van een basisversie voor elke auteur maken. 

Elke auteur brengt wijzigingen aan zijn of haar eigen versie van de app. Wanneer elke auteur maken door het model is, moet u de nieuwe versies exporteren naar JSON-bestanden.  

Geëxporteerde apps zijn JSON-indeling bestanden, die wijzigingen kunnen worden vergeleken. Combineer de bestanden voor het maken van een enkel JSON-bestand van de nieuwe versie. Wijzig de **versionId** eigenschap in de JSON om de nieuwe versie van de samengevoegde aan te geven. Die versie importeren in de oorspronkelijke app. 

Deze methode kunt u een actieve versie, één fase-versie en een gepubliceerde versie hebben. U kunt de resultaten in het deelvenster met interactieve testen voor de drie versies vergelijken.

### <a name="manage-multiple-versions-as-apps"></a>Beheer van meerdere versies als apps
[Exporteren](luis-how-to-manage-versions.md#export-version) de basisversie. Elke auteur importeert de-versie. De persoon die de invoer van de app is de eigenaar van de versie. Wanneer ze klaar zijn voor het wijzigen van de app, de versie exporteren. 

Geëxporteerde apps zijn JSON-indeling bestanden, die kunnen worden vergeleken met de basis exporteren voor wijzigingen. Combineer de bestanden voor het maken van een enkel JSON-bestand van de nieuwe versie. Wijzig de **versionId** eigenschap in de JSON om de nieuwe versie van de samengevoegde aan te geven. Die versie importeren in de oorspronkelijke app.

## <a name="next-steps"></a>Volgende stappen

Inzicht in [versiebeheer](luis-concept-version.md) concepten. 

Zie [App-instellingen](luis-how-to-collaborate.md) voor informatie over het beheren van deelnemers in uw LUIS-app.

Zie [e-mailadres toevoegen aan de toegangslijst](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58fcccdd5aca2f08a4104342) met de API's voor ontwerpen.