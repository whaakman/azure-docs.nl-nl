---
title: Met tags in Azure inhoud beheerder | Microsoft Docs
description: Inhoud beheerder omvat standaardtags en kunt u aangepaste labels voor toezicht houden van inhoud die specifiek zijn voor uw bedrijf.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: add4c685c07c63944ae89f48a47ac78df28c1623
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344454"
---
# <a name="about-tags"></a>Over Tags #

Naast de twee standaardtags (een – isadult en r – isracy), kunt u aangepaste labels maken voor meer gericht scannen. Deze aangepaste labels zijn vervolgens beschikbaar voor menselijke revisoren toewijzen aan installatiekopieën of tekst.

## <a name="create-tags"></a>Labels maken ##

1.  Selecteer de labels van het tabblad instellingen.

  ![Inhoud toezicht labels](images/tags-1.png)

2.  Voer een korte twee letters-code voor het label.
3.  Voer een naam voor het label. Houd de naam kort en beschrijvend. Bijvoorbeeld 'isNudity'.
4.  Voer een beschrijving.
5.  Klik op Toevoegen.
6.  Klik op opslaan wanneer u klaar bent met labels maken.

![Inhoud toezicht labels definiëren](images/tags-2-define.png)

## <a name="using-custom-tags"></a>Gebruik aangepaste labels ##

Aangepaste labels worden gebruikt tijdens menselijke revisie. Ze worden weergegeven op de Preview-versie en de revisor geselecteerd door erop te klikken.

![Met behulp van de inhoud toezicht labels](images/tags-3-use.png)

U kunt verschillende tags voor verschillende revisies uitschakelen door het in- of uitschakelt zichtbaar Is.
 
![Inhoud toezicht labels uitschakelen](images/tags-4-disable.png)

Terwijl u de twee standaardtags, isadult en isracy niet verwijderen, kunt u een aangepaste labels die u hebt gedefinieerd verwijderen. Klik op de garbage collector zijn, kunnen naast het label dat u wilt verwijderen.

![Inhoud toezicht labels verwijderen](images/tags-5-delete.png)

## <a name="next-steps"></a>Volgende stappen ##

Zie voor meer informatie over het gebruik van de labels voor controle van de installatiekopie, [hoe controleert met toezicht installatiekopieën](Review-Moderated-Images.md).
