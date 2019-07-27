---
title: Versies beheren-LUIS
titleSuffix: Azure Cognitive Services
description: Versies kunnen u bouwt en publiceert verschillende modellen. Er is een goede gewoonte om te klonen van de huidige actieve model naar een andere versie van de app voordat u wijzigingen in het model.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/16/2019
ms.author: diberry
ms.openlocfilehash: 1169af85c35f04a302816f564caeb49eaf455d0e
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563657"
---
# <a name="use-versions-to-edit-and-test-without-impacting-staging-or-production-apps"></a>Versies gebruiken om te bewerken en te testen zonder dat dit van invloed is op staging-of productie-apps

Versies kunnen u bouwt en publiceert verschillende modellen. Er is een goede gewoonte om te klonen van de huidige actieve model met een andere [versie](luis-concept-version.md) van de app voordat u wijzigingen in het model. 

Als u wilt werken met versies, opent u uw app door het selecteren van de naam op **mijn Apps** pagina en selecteer vervolgens **beheren** in de bovenste balk, schakelt u vervolgens **versies** in het linkernavigatievenster. 

In de lijst met versies ziet u welke versies worden gepubliceerd, waar ze worden gepubliceerd en welke versie momenteel actief is. 

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

[![Sectie beheren, pagina versies, een versie actie maken](./media/luis-how-to-manage-versions/versions-other.png "Sectie beheren, pagina versies, een versie actie maken")](./media/luis-how-to-manage-versions/versions-other.png#lightbox)

## <a name="import-version"></a>Importversie

1. Selecteer **importeren versie** via de werkbalk. 

2. In de **de nieuwe versie importeren** pop-upvenster, geef de nieuwe naam voor de versie van tien tekens. U hoeft in te stellen van een versie-ID als de versie in het JSON-bestand al in de app bestaat.

    ![Sectie beheren, versies pagina, nieuwe versie importeren](./media/luis-how-to-manage-versions/versions-import-pop-up.png)

    Nadat u een versie importeert, wordt de nieuwe versie de actieve versie.

### <a name="import-errors"></a>Fouten importeren

* Tokenizer-fouten: Als er een **tokenizer-fout optreedt** tijdens het importeren, probeert u een versie te importeren die gebruikmaakt van een andere [tokenizer](luis-language-support.md#custom-tokenizer-versions) dan de app die momenteel gebruikt. Zie voor het oplossen van dit probleem [migreren tussen tokenizer-versies](luis-language-support.md#migrating-between-tokenizer-versions).

<a name = "export-version"></a>

## <a name="other-actions"></a>Andere acties

* Naar **verwijderen** een versie gebruikt, selecteert u een versie in de lijst en vervolgens **verwijderen** via de werkbalk. Selecteer **OK**. 
* Naar **naam** een versie gebruikt, selecteert u een versie in de lijst en vervolgens **Wijzig de naam van** via de werkbalk. Voer nieuwe naam en selecteer **gedaan**. 
* Naar **exporteren** een versie gebruikt, selecteert u een versie in de lijst en vervolgens **Export app** via de werkbalk. Kies JSON om te exporteren voor back-up. Kies **exporteren voor container** om [deze app te gebruiken in een Luis-container](luis-container-howto.md).  

