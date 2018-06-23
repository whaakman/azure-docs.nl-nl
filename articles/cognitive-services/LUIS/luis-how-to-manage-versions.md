---
title: Versies in LUIS apps in Azure beheren | Microsoft Docs
description: Informatie over het beheren van versies in Language Understanding (LUIS)-toepassingen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/29/2017
ms.author: v-geberr
ms.openlocfilehash: 672f7991be0fc236e39daf7d1ce1d6080b31815b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344514"
---
# <a name="manage-versions"></a>Versies beheren

Telkens wanneer u werkt op het model, maakt een andere [versie](luis-concept-version.md) van de app. 

## <a name="set-active-version"></a>Stel de actieve versie
Om te werken met versies, opent u uw app door de naam ervan selecteren op **mijn Apps** pagina en selecteer vervolgens **instellingen** in de bovenste balk.

![Pagina-versies](./media/luis-how-to-manage-versions/settings.png)

De **instellingen** pagina kunt u instellingen configureren voor de hele app met inbegrip van versies en deelnemers. 

## <a name="clone-a-version"></a>Klonen van een versie
1. Op de **instellingen** pagina nadat de App-instellingen en deelnemers secties, zoek de rij met de versie die u wilt klonen. Selecteer de drie puntjes (...) op de meest rechtse. 

    ![Eigenschappen van de rij versie](./media/luis-how-to-manage-versions/version-section.png)

2. Selecteer **kloon** uit de lijst.

    ![Versie rij eigenschappen keuze](./media/luis-how-to-manage-versions/version-three-dots-modal.png)

3. In de **kloon versie** in het dialoogvenster een naam voor de nieuwe versie, zoals '0,2'.

   ![Dialoogvenster van de kloon-versie](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
 > [!NOTE]
 > Versie-ID kan bestaan alleen uit tekens, cijfers of '.' en mag niet langer zijn dan 10 tekens bevatten.
 
 Een nieuwe versie met de opgegeven naam is gemaakt en worden ingesteld als de actieve versie.
 
  ![Versie is gemaakt en toegevoegd aan de lijst](./media/luis-how-to-manage-versions/new-version.png)

 > [!NOTE]
 > Zoals u in de voorgaande afbeelding, een gepubliceerde versie is gekoppeld aan een gekleurde is ingeschakeld, die het type van de site waar deze is gepubliceerd aangeeft: productie (groen), fasering (rood) en beide (zwart). De training en publishing datums worden weergegeven voor elke gepubliceerde versie.

## <a name="set-active-version"></a>Stel de actieve versie
1. Op de **instellingen** pagina in de **versies** , selecteert u de drie puntjes (...) aan de rechterkant.

2. Selecteer in het pop-lijst **ingesteld as actief**.

    ![Stel de actieve versie](./media/luis-how-to-manage-versions/set-active-version.png)

    De actieve versie is gemarkeerd met een lichte blauw, zoals wordt weergegeven in de volgende schermafbeelding:

    ![De actieve versie](./media/luis-how-to-manage-versions/set-active-version-done.png) 


## <a name="import-version"></a>Versie importeren
U kunt een versie van een JSON-bestand importeren. Nadat u een versie importeert, wordt de nieuwe versie de actieve versie.

**Een versie importeren:**

1. Op de **instellingen** pagina **importeren nieuwe versie** knop.

    ![Knop importeren](./media/luis-how-to-manage-versions/import-version.png)

2. Selecteer **Bladeren** en kiest u het JSON-bestand.

    ![Dialoogvenster van de versie importeren](./media/luis-how-to-manage-versions/import-version-dialog.png)

Alleen moet u een versie-ID ingesteld als de versie in het JSON-bestand al in de app bestaat.

## <a name="export-version"></a>Versie exporteren
U kunt een versie exporteren naar een JSON-bestand.

**Exporteren van een versie:**

1. Op de **instellingen** pagina in de **versies** , selecteert u de drie puntjes (...) aan de rechterkant.

2. Selecteer **exporteren** in het pop-lijst van acties en selecteer waar u het bestand.

## <a name="delete-a-version"></a>Een versie verwijderen
Kunt u versies verwijderen, maar u hebt ten minste één versie van de app wilt behouden. U kunt alle versies met uitzondering van de actieve versie verwijderen. 

1. Op de **instellingen** pagina in de **versies** , selecteert u de drie puntjes (...) aan de rechterkant.

2. Selecteer **verwijderen** in het pop-lijst van acties en selecteer waar u het bestand.

    ![Bevestiging van versie verwijderen](./media/luis-how-to-manage-versions/delete-menu.png) 


## <a name="rename-a-version"></a>Wijzig de naam van een versie
Als de versienaam nog niet is gebruikt, kunt u versies wijzigen.  

1. Op de **instellingen** pagina in de **versies** , selecteert u de drie puntjes (...) aan de rechterkant.

2. Selecteer **naam** in het pop-lijst van acties.

3. Voer de versienaam van de nieuwe en selecteer **gedaan**.

    ![Wijzig de naam van de bevestiging versie](./media/luis-how-to-manage-versions/rename-popup.png) 
