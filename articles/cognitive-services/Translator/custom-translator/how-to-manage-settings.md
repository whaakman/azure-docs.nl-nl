---
title: Over het beheren van instellingen? -Aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Het beheren van instellingen, -werkruimte maken, delen van de werkruimte en abonnementssleutel in aangepaste Translator beheren.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: v-rada
ms.topic: conceptual
ms.openlocfilehash: 6e8406cdd265063622bc5dcebbb9f22549b3633a
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57766798"
---
# <a name="how-to-manage-settings"></a>Over het beheren van instellingen

Binnen de pagina aangepaste Translator-instellingen kunt u een nieuwe werkruimte maken, delen van uw werkruimte en toevoegen of wijzigen van uw abonnementssleutel Microsoft Translation.

Voor toegang tot de instellingenpagina:

1. Aanmelden bij de [aangepaste Translator](https://portal.customtranslator.azure.ai/) portal.
2. Klik op het tandwielpictogram in de zijbalk op aangepaste Translator-portal.

    ![Koppeling instellen](media/how-to/how-to-settings.png)

## <a name="associating-microsoft-translator-subscription"></a>Microsoft Translator-abonnement koppelen

U moet een Microsoft Translator Text-API-abonnementssleutel die zijn gekoppeld aan uw werkruimte te trainen of implementeren van modellen.

Als u geen abonnement hebt, volgt u de volgende stappen uit:

1. Abonneer u op de Microsoft Translator Text-API. Dit artikel leest hoe u zich abonneren op de Microsoft Translator Text-API.
2. Noteer de sleutel voor uw abonnement op translator. Een van de Key1 of Key2 worden geaccepteerd.
3. Ga terug naar de aangepaste Translator-portal.

### <a name="add-existing-key"></a>Bestaande sleutel toevoegen

1.  Ga naar de pagina 'Instellingen' voor uw werkruimte.
2.  Klik op sleutel toevoegen

    ![Abonnementssleutel toevoegen](media/how-to/how-to-add-subscription-key.png)

3. Voer de sleutel voor uw abonnement op translator in het dialoogvenster en klik op de knop 'Toevoegen'.

    ![Abonnementssleutel toevoegen](media/how-to/how-to-add-subscription-key-dialog.png)
4.  Nadat u een sleutel hebt toegevoegd, kunt u wijzigen of verwijderen van de sleutel op elk gewenst moment.

    ![Abonnementssleutel na toevoegen](media/how-to/subscription-key-after-add.png)

## <a name="manage-your-workspace"></a>Uw werkruimte beheren

Een werkruimte is een werkgebied voor samenstellen en het bouwen van uw aangepaste vertaalsysteem. Een werkruimte kan bevatten meerdere projecten, modellen en documenten.

Als ander deel van uw werk worden gedeeld met andere mensen moet, kan het maken van meerdere werkruimten nuttig zijn.

## <a name="create-a-new-workspace"></a>Een nieuwe werkruimte maken

1.  Navigeer naar de pagina van de werkruimte 'Instellingen'.
2.  Klik op de 'nieuwe werkruimte"knop in de sectie 'Nieuwe werkruimte maken'.

    ![Nieuwe werkruimte maken](media/how-to/create-new-workspace.png)

4.  Voer de naam van de nieuwe werkruimte in het dialoogvenster.
5.  Klik op 'Maken'.

    ![Dialoogvenster voor een nieuwe werkruimte maken](media/how-to/create-new-workspace-dialog.png)

## <a name="share-your-workspace"></a>Delen van uw werkruimte

In aangepaste Translator kunt u uw werkruimte delen met anderen, als ander deel van uw werk moet worden gedeeld met andere mensen.

1.  Navigeer naar de pagina van de werkruimte 'Instellingen'.
2.  Klik op de knop 'Delen' in de sectie 'Instellingen voor delen'.

    ![Werkruimte voor delen](media/how-to/share-workspace.png)

3.  Voer een door komma's gescheiden lijst met e-mailadressen die u wilt dat deze werkruimte gedeeld met in het dialoogvenster. Zorg ervoor dat u wilt delen met het e-mailadres dat deze persoon zich aanmeldt bij aangepaste Translator met gebruikt. Selecteer vervolgens het juiste niveau van de machtiging voor delen.

4.  Als uw werkruimte heeft nog steeds de standaardnaam 'Mijn werkruimte', moet u om dit te wijzigen voordat u deelt uw werkruimte.
5.  Klik op 'Opslaan'.

## <a name="sharing-permissions"></a>Machtigingen voor delen

1.  **Lezer:** Een lezer in de werkruimte is mogelijk om alle gegevens in de werkruimte weer te geven.

2.  **Editor:** Een editor in de werkruimte zich documenten toevoegen, het trainen van modellen en het verwijderen van documenten en projecten. Ze kunnen een abonnementssleutel toevoegen, maar kan niet wijzigen die de werkruimte wordt gedeeld met, verwijderen van de werkruimte of wijzig de Werkruimtenaam van de.

3.  **Eigenaar:** Een eigenaar heeft volledige machtigingen tot de werkruimte.

## <a name="change-sharing-permission"></a>Machtiging voor delen wijzigen

Wanneer een werkruimte wordt gedeeld, toont de sectie 'Instellingen voor delen' alle e-mailadressen die deze werkruimte wordt gedeeld met. U kunt bestaande machtiging voor elk e-mailadres delen als u eigenaar toegang tot de werkruimte hebben.

1.  In de sectie 'Instellingen voor delen' voor elk e-mailbericht bevat een vervolgkeuzelijst het huidige machtigingsniveau.

2.  Klik op de vervolgkeuzelijst en selecteert u het nieuwe machtigingsniveau die u wilt toewijzen aan dit e-mailadres.

    ![Machtigingsinstellingen voor delen](media/how-to/sharing-permission-settings.png)

## <a name="next-steps"></a>Volgende stappen

- Informatie over [over het migreren van uw werkruimte en het project](how-to-migrate.md) van [Microsoft Translator-Hub](https://hub.microsofttranslator.com)
