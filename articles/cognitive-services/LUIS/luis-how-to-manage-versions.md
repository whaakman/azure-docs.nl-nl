---
title: Versies beheren
titleSuffix: Language Understanding - Azure Cognitive Services
description: Versies kunnen u bouwt en publiceert verschillende modellen. Er is een goede gewoonte om te klonen van de huidige actieve model naar een andere versie van de app voordat u wijzigingen in het model.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 73f962cc8e0d4605c0c5f0bace79553033ebe082
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58110899"
---
# <a name="use-versions-to-edit-and-test-without-impacting-staging-or-production-apps"></a>Versies gebruiken om te bewerken en te testen zonder dat dit fasering of productie-apps

Versies kunnen u bouwt en publiceert verschillende modellen. Er is een goede gewoonte om te klonen van de huidige actieve model met een andere [versie](luis-concept-version.md) van de app voordat u wijzigingen in het model. 

Als u wilt werken met versies, opent u uw app door het selecteren van de naam op **mijn Apps** pagina en selecteer vervolgens **beheren** in de bovenste balk, schakelt u vervolgens **versies** in het linkernavigatievenster. 

De lijst met versies laten zien welke versies worden gepubliceerd, waarin ze worden gepubliceerd, en welke versie momenteel actief is. 

[![Beheersectie, pagina-versies](./media/luis-how-to-manage-versions/versions-import.png "gedeelte beheren, pagina-versies")](./media/luis-how-to-manage-versions/versions-import.png#lightbox)

## <a name="clone-a-version"></a>Een versie klonen

1. Selecteer de versie die u wilt klonen Selecteer **kloon** via de werkbalk. 

2. In de **kloon versie** in het dialoogvenster, typ een naam voor de nieuwe versie, zoals '0.2'.

   ![Dialoogvenster van de kloon-versie](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
     > [!NOTE]
     > Versie-ID kan bestaan alleen uit tekens, cijfers of '.' en mag niet langer zijn dan 10 tekens.
 
   Een nieuwe versie met de opgegeven naam gemaakt en ingesteld als de actieve versie.

## <a name="set-active-version"></a>Actieve versie instellen

Selecteer een versie in de lijst en selecteer vervolgens **Maak actief** via de werkbalk. 

[![Beheersectie, pagina-versies, maken een actie versie](./media/luis-how-to-manage-versions/versions-other.png "beheersectie, pagina-versies, een versie-actie maken")](./media/luis-how-to-manage-versions/versions-other.png#lightbox)

## <a name="import-version"></a>Importversie

1. Selecteer **importeren versie** via de werkbalk. 

2. In de **de nieuwe versie importeren** pop-upvenster, geef de nieuwe naam voor de versie van tien tekens. U hoeft in te stellen van een versie-ID als de versie in het JSON-bestand al in de app bestaat.

    ![Sectie, pagina-versies, nieuwe versie importeren beheren](./media/luis-how-to-manage-versions/versions-import-pop-up.png)

    Nadat u een versie importeert, wordt de nieuwe versie de actieve versie.

<a name = "export-version"></a>

## <a name="other-actions"></a>Andere acties

* Naar **verwijderen** een versie gebruikt, selecteert u een versie in de lijst en vervolgens **verwijderen** via de werkbalk. Selecteer **OK**. 
* Naar **naam** een versie gebruikt, selecteert u een versie in de lijst en vervolgens **Wijzig de naam van** via de werkbalk. Voer nieuwe naam en selecteer **gedaan**. 
* Naar **exporteren** een versie gebruikt, selecteert u een versie in de lijst en vervolgens **Export app** via de werkbalk. Het bestand wordt gedownload naar uw lokale computer. 

