---
title: Hoe kunt u een project maken? -Aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Over het maken van een project in aangepaste Translator?
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.date: 02/21/2019
ms.author: v-rada
ms.topic: conceptual
ms.openlocfilehash: dde1bfbaf810d07a2b992258dd8674efec302dc6
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737158"
---
# <a name="create-a-project"></a>Een project maken

Een project is een container voor een modellen, documenten, en test. Elk project omvat automatisch alle documenten die in deze werkruimte zijn geüpload met de combinatie van de juiste taal.

Project maakt, wordt de eerste stap voor het bouwen van een model.

## <a name="create-a-project"></a>Maak een project:

1.  In de [aangepaste Translator](https://portal.customtranslator.azure.ai) portal, klik op maken project.

    ![Project maken](media/how-to/how-to-create-project.png)

2.  Voer de volgende gegevens over uw project in het dialoogvenster:

    a.  De projectnaam (vereist): Geef uw project een unieke, betekenisvolle naam. Het is niet nodig zijn om te vermelden de talen in de titel.

    b.  Beschrijving: Een kort overzicht van het project. Deze beschrijving heeft geen invloed op de werking van de aangepaste Translator of uw resulterende aangepaste systeem, maar kunt u onderscheid maken tussen verschillende projecten.

    c.  Taalpaar (vereist): Selecteer de taal waarin u bezig bent met het converteren van en naar.

    d.  Categorie (vereist): Selecteer de categorie die het meest geschikt is voor uw project. De categorie beschrijft de terminologie en stijl van de documenten die u van plan bent om te vertalen.

    e.  Categoriebeschrijving: Dit veld gebruiken om te beschrijven beter het bepaalde veld of de branche waarin u werkt. Bijvoorbeeld, als uw categorie arts, kunt u toevoegen een bepaald document, zoals een chirurgie of pediatrics. De beschrijving heeft geen invloed op over de werking van de aangepaste Translator of uw resulterende aangepaste systeem.

    f.  Project-label: De [project label](workspace-and-project.md#project-labels) wordt onderscheid gemaakt tussen projecten met dezelfde combinatie van taal en categorie. Als een best practice wordt gebruikgemaakt van een label *alleen* als u van plan bent voor het bouwen van meerdere projecten voor de dezelfde combinatie van taal en dezelfde categorie en toegang kunt krijgen tot deze projecten met een andere categorie-id. Gebruik dit veld niet als u bij het bouwen van systemen voor slechts één categorie. Een label project is niet vereist en niet nuttig onderscheid maken tussen taal paren. U kunt hetzelfde label gebruiken voor meerdere projecten.

    ![Dialoogvenster project maken](media/how-to/how-to-create-project-dialog.png)

3.  Klik op Maken

## <a name="view-project-details"></a>Projectgegevens weergeven

De startpagina van de aangepaste Translator ziet u de eerste 10 projecten in uw werkruimte. De naam van het project, taalpaar, categorie, status en BLEU score wordt weergegeven.

Na het selecteren van een project, ziet u het volgende op de projectpagina:

- CategoryID: Een categorie-id wordt gemaakt door het samenvoegen van de werkruimte-id, de project-label en de categoriecode. Gebruikt u de categorie-id met de Translator Text-API voor aangepaste vertalingen ophalen.

- Knop van de trein: Gebruik deze knop om te beginnen een [trainen van een model](how-to-train-model.md).

- Knop documenten toevoegen: Gebruik deze knop om [documenten uploaden](how-to-upload-document.md).

- Documenten knop filteren: Gebruik deze knop om te filteren en zoeken naar specifieke documenten.

    ![Projectgegevens weergeven](media/how-to/how-to-view-project.png)

## <a name="next-steps"></a>Volgende stappen

- Informatie over [hoe om te zoeken, bewerken en verwijderen van project](how-to-search-edit-delete-projects.md).
- Informatie over [document uploaden](how-to-upload-document.md) om NAT-modellen te bouwen.
