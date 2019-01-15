---
title: Aangepaste tags gebruiken voor inhoudstoezicht - Content Moderator
titlesuffix: Azure Cognitive Services
description: Content Moderator bevat standaardtags en kunt u aangepaste labels voor toezicht houden op inhoud die specifiek is voor uw bedrijf.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: c1a547f99995d25d19dafb03276306c50a544c9a
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54264713"
---
# <a name="create-and-use-moderation-tags"></a>Maak en gebruik toezicht op Tags

Naast de twee standaard-tags, **isadult** (**een**) en **isracy** (**r**), kunt u aangepaste labels maken meer gericht scannen. Deze aangepaste labels zijn vervolgens beschikbaar voor menselijke revisoren toewijzen aan installatiekopieën of tekst.

## <a name="create-tags"></a>Tags maken

1.  Tags selecteren op het tabblad instellingen.

  ![Content Moderation Tags](images/tags-1.png)

2.  Voer een korte code van twee letters voor de tag.
3.  Voer een naam voor het label. Houd de naam kort en beschrijvend. Bijvoorbeeld, **isbullying**.
4.  Voer een beschrijving.
5.  Klik op Toevoegen.
6.  Klik op opslaan wanneer u klaar bent met het maken van het tags.

![Content Moderation Tags definiëren](images/tags-2-define.png)

## <a name="using-custom-tags"></a>Met behulp van aangepaste labels

Aangepaste labels worden gebruikt tijdens menselijke beoordeling. Ze worden weergegeven op de Preview-versie en de revisor geselecteerd door erop te klikken.

![Met behulp van labels voor Content Moderation](images/tags-3-use.png)

U kunt verschillende labels voor verschillende revisies uitschakelen door het in- of uitschakelt zichtbaar Is.
 
![Content Moderation labels uitschakelen](images/tags-4-disable.png)

Terwijl u de twee standaard-tags, niet verwijderen **isadult** en **isracy**, kunt u een aangepaste labels die u hebt gedefinieerd verwijderen. Klik op de Prullenbak naast het label dat u wilt verwijderen.

![Content Moderation labels verwijderen](images/tags-5-delete.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van tags voor beheer van afbeeldingen, [revisie images voorgezeten](Review-Moderated-Images.md).
