---
title: Versies in LUIS-apps in Azure beheren | Microsoft Docs
description: Informatie over het beheren van versies in Language Understanding (LUIS)-toepassingen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/29/2017
ms.author: diberry
ms.openlocfilehash: 4941cf533f1b860ead07a416d5af6f62a1978305
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39226591"
---
# <a name="manage-versions"></a>Versies beheren

Telkens wanneer u werkt op het model, maak een andere [versie](luis-concept-version.md) van de app. 

## <a name="set-active-version"></a>Actieve versie instellen
Als u wilt werken met versies, opent u uw app door het selecteren van de naam op **mijn Apps** pagina en selecteer vervolgens **instellingen** in de bovenste balk.

![Pagina-versies](./media/luis-how-to-manage-versions/settings.png)

De **instellingen** op de pagina kunt u instellingen voor de hele app met inbegrip van versies en samenwerkers kunt configureren. 

## <a name="clone-a-version"></a>Een versie klonen
1. Op de **instellingen** pagina nadat de App-instellingen en deelnemers secties, vinden de rij met de versie die u wilt klonen. Selecteer het weglatingsteken (***...*** ) knop helemaal rechts. 

    ![Eigenschappen van de rij versie](./media/luis-how-to-manage-versions/version-section.png)

2. Selecteer **kloon** in de lijst.

    ![Versie rij eigenschappen keuze](./media/luis-how-to-manage-versions/version-three-dots-modal.png)

3. In de **kloon versie** in het dialoogvenster, typ een naam voor de nieuwe versie, zoals '0.2'.

   ![Dialoogvenster van de kloon-versie](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
 > [!NOTE]
 > Versie-ID kan bestaan alleen uit tekens, cijfers of '.' en mag niet langer zijn dan 10 tekens.
 
 Een nieuwe versie met de opgegeven naam gemaakt en ingesteld als de actieve versie.
 
  ![Versie is gemaakt en toegevoegd aan de lijst](./media/luis-how-to-manage-versions/new-version.png)

 > [!NOTE]
 > Zoals weergegeven in de voorgaande afbeelding, een gepubliceerde versie is gekoppeld aan een gekleurde is ingeschakeld, die aangeeft van het type van de site waar deze is gepubliceerd: productie (groen), fasering (rood), en beide (zwart). De training en publiceren datums worden weergegeven voor elke gepubliceerde versie.

## <a name="set-active-version"></a>Actieve versie instellen
1. Op de **instellingen** pagina, in de **versies** lijst, selecteer het weglatingsteken (***...*** ) knop aan de rechterkant.

2. Selecteer in het pop-lijst **instellen als actief**.

    ![Actieve versie instellen](./media/luis-how-to-manage-versions/set-active-version.png)

    De actieve versie wordt gemarkeerd door een lichtblauw, zoals wordt weergegeven in de volgende schermafbeelding:

    ![De actieve versie](./media/luis-how-to-manage-versions/set-active-version-done.png) 


## <a name="import-version"></a>Importversie
U kunt een versie van een JSON-bestand importeren. Nadat u een versie importeert, wordt de nieuwe versie de actieve versie.

**Een versie importeren:**

1. Op de **instellingen** weergeeft, schakelt **de nieuwe versie importeren** knop.

    ![Knop importeren](./media/luis-how-to-manage-versions/import-version.png)

2. Selecteer **Bladeren** en kiest u het JSON-bestand.

    ![Het dialoogvenster versie importeren](./media/luis-how-to-manage-versions/import-version-dialog.png)

U hoeft in te stellen van een versie-ID als de versie in het JSON-bestand al in de app bestaat.

## <a name="export-version"></a>Versie exporteren
U kunt een versie exporteren naar een JSON-bestand.

**Voor het exporteren van een versie:**

1. Op de **instellingen** pagina, in de **versies** lijst, selecteer het weglatingsteken (***...*** ) knop aan de rechterkant.

2. Selecteer **exporteren** in de POP-lijst van acties en selecteer waar u het bestand op te slaan.

## <a name="delete-a-version"></a>Een versie verwijderen
U kunt de versies verwijderen, maar u hebt ten minste één versie van de app wilt behouden. U kunt alle versies, behalve de actieve versie verwijderen. 

1. Op de **instellingen** pagina, in de **versies** lijst, selecteer het weglatingsteken (***...*** ) knop aan de rechterkant.

2. Selecteer **verwijderen** in de POP-lijst van acties en selecteer waar u het bestand op te slaan.

    ![Versie bevestiging van verwijderen](./media/luis-how-to-manage-versions/delete-menu.png) 


## <a name="rename-a-version"></a>Wijzig de naam van een versie
U kunt de naam van versies wijzigen, zolang de versienaam van de nog niet in gebruik.  

1. Op de **instellingen** pagina, in de **versies** lijst, selecteer het weglatingsteken (***...*** ) knop aan de rechterkant.

2. Selecteer **naam** in het pop-lijst met acties.

3. Voer de versienaam van de nieuwe en selecteer **gedaan**.

    ![Wijzig de naam van versie bevestigen](./media/luis-how-to-manage-versions/rename-popup.png) 
