---
title: Microsoft Translator Hub werkruimte en projecten migreren? -Aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Migreer uw Hub-werkruimte en projecten naar aangepaste Translator.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 378baad0735238dc0921e5e78e2a27b3ae907e19
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51627544"
---
# <a name="migrate-hub-workspace-and-projects-to-custom-translator"></a>Hub-werkruimte en projecten migreren naar aangepaste Translator

U kunt migreren uw [Microsoft Translator Hub](https://hub.microsofttranslator.com/) werkruimte en projecten met aangepaste Translator. Migratie wordt gestart vanaf de Hub.


Deze items worden tijdens het proces gemigreerd:

1.  De definities van projecten.

2.  De definitie van de training wordt gebruikt voor het maken van de modeldefinitie van een nieuw op aangepaste Translator.

3.  Parallelle en ééntalig bestanden die worden gebruikt binnen de trainingen, alle gemigreerd als nieuwe documenten in aangepaste Translator.

4.  De automatisch gegenereerde system-test en het afstemmen van de gegevens worden geëxporteerd en gemaakt als nieuwe documenten in aangepaste Translator.

Aangepaste Translator wordt voor alle geïmplementeerde trainingen, het model zonder kosten te trainen. U hebt de mogelijkheid deze handmatig te implementeren.

>[!Note]
>Aangepaste Translator vereist voor een training te voltooien, minimale 10.000 uitgepakte zinnen. Voor een kleiner aantal opgehaalde zinnen dan de [voorgestelde minimale](sentence-alignment.md#suggested-minimum-number-of-extracted-and-aligned-sentences), aangepaste Translator kan niet een trainen.

Voor alle geslaagde trainingen, die niet zijn geïmplementeerd, worden ze worden gemigreerd als concept in aangepaste Translator.

## <a name="find-custom-translator-workspace-id"></a>Aangepaste Translator werkruimte-Id vinden

Voor het migreren van [Microsoft Translator Hub](https://hub.microsofttranslator.com/) werkruimte moet u de werkruimte-Id in aangepaste Translator bestemming. De doelwerkruimte in aangepaste Translator is waar uw Hub werkruimten en projecten moeten worden gemigreerd naar.

U vindt de bestemming werkruimte-Id op de pagina aangepaste Translator-instellingen: 

1.  Ga naar de pagina met 'Instellingen' in de portal voor aangepaste Translator.

2.  De werkruimte-Id vindt u in de sectie Algemene informatie.

    ![Over het vinden van bestemming werkruimte-ID](media/how-to/how-to-find-destination-ws-id.png)

3. Houd uw doel werkruimte-Id om te verwijzen tijdens het migratieproces.

## <a name="migrate-workspace"></a>Migreren van werkruimte

Wanneer u uw volledige Hub-werkruimte met aangepast translator migreert, krijgen uw projecten, documenten en u kunt met aangepaste translator worden gemigreerd. Vóór de migratie, die u moet kiezen als u wilt migreren alleen geïmplementeerde trainingen of u wilt alle van de geslaagde kunt migreren.

Voor het migreren van een werkruimte:

1.  Aanmelden bij Microsoft Translator-Hub.

2.  Ga naar de pagina 'Instellingen'.

3.  Klik op 'Migreren gegevens in de werkruimte met aangepast Translator' op 'Instellingen'-pagina.

    ![Over het migreren van de Hub](media/how-to/how-to-migrate-workspace-from-hub.png)

4.  Selecteer een van deze twee opties op de volgende pagina:

    a.  U kunt alleen geïmplementeerd: deze optie alleen uw geïmplementeerde systemen en gerelateerde documenten worden gemigreerd.

    b.  U kunt alle geslaagde: Deze optie selecteert, worden gemigreerd alle geslaagde trainingen en gerelateerde documenten.

    c.  Voer uw bestemming werkruimte-ID in aangepaste Translator.

    ![Over het migreren van de Hub](media/how-to/how-to-migrate-from-hub-screen.png)

5.  Klik op aanvraag verzenden.

## <a name="migrate-project"></a>Migreren van project

Als u migreren van uw projecten selectief wilt, geeft u in Microsoft Translator-Hub die mogelijkheid.

Voor het migreren van een project:

1.  Aanmelden bij Microsoft Translator-Hub.

2.  Ga naar de pagina 'Projecten'.

3.  Klik op de koppeling 'Migreren' voor het juiste project.

    ![Over het migreren van de Hub](media/how-to/how-to-migrate-from-hub.png)

4.  Selecteer een van deze twee opties op de volgende pagina:

    a.  U kunt alleen geïmplementeerd: deze optie alleen uw geïmplementeerde systemen en gerelateerde documenten worden gemigreerd. 

    b.  U kunt alle geslaagde: Deze optie selecteert, worden gemigreerd alle geslaagde trainingen en gerelateerde documenten.

    c.  Voer uw bestemming werkruimte-ID in aangepaste Translator.

    ![Over het migreren van de Hub](media/how-to/how-to-migrate-from-hub-screen.png)

5.  Klik op "Aanvraag verzenden".

## <a name="migration-history"></a>Migratiegeschiedenis

Wanneer u hebt aangevraagd werkruimte / migratie van de Hub project, vindt u de migratiegeschiedenis van een op de pagina aangepaste Translator-instellingen. 

Als u de migratiegeschiedenis, de volgende stappen uit:

1.  Ga naar de pagina met 'Instellingen' in de portal voor aangepaste Translator.

2.  Klik in de sectie Geschiedenis van de migratie van de pagina instellingen op Migratiegeschiedenis.

    ![Migratiegeschiedenis](media/how-to/how-to-migration-history.png)

Migratie geschiedenispagina bevat de volgende informatie als overzicht voor elke migratie die u hebt aangevraagd.

1.  Gemigreerde door: Naam en e-mailadres van de gebruiker deze migratieaanvraag ingediend

2.  Op gemigreerd: Datum en tijd stempel van de migratie

3.  Projecten: Aantal projecten aangevraagd voor migratie v/s aantal projecten is gemigreerd.

4.  U kunt: Aantal trainingen aangevraagd voor migratie v/s aantal trainingen gemigreerd.

5.  Documents: Het aantal documenten die zijn aangevraagd voor migratie v/s aantal documenten is gemigreerd.

    ![Details van migratie](media/how-to/how-to-migration-history-details.png)

Als u dat meer gedetailleerde migratierapport over uw projecten, trainingen en documenten wilt, hebt u de optie exporteren als CSV-bestand.

>[!Note]
>Migratie wordt alleen ondersteund voor de taal-paren waarbij NMT talen bestaan. Controleer de lijst met momenteel [ondersteunde talen NMT](https://www.microsoft.com/translator/business/languages/). Voor de taal-paren waarbij NMT-talen niet bestaat, met aangepaste Translator gegevens uit de Hub worden verplaatst, maar trainingen kunnen niet worden uitgevoerd op de taal-paren.

## <a name="next-steps"></a>Volgende stappen

- [Een model te trainen](how-to-train-model.md).
- Start met behulp van het model van uw geïmplementeerde aangepaste vertaling via [Microsoft Translator Text-API V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).