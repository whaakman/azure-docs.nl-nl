---
title: Teams en subteams in de inhoud beheerder API | Microsoft Docs
description: Informatie over het gebruik van teams en subteams in de inhoud beheerder-API voor cognitieve Services.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/22/2017
ms.author: sajagtap
ms.openlocfilehash: 161c7cd8bac07d5ffc138297d98a40317a8d88fc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344460"
---
# <a name="team-and-subteams"></a>Team en Subteams #

Voordat u inhoud beheerder moet u een Team maken. Wanneer u zich omhoog en naam van uw team, wordt dit team van uw standaardteam geworden. U kunt slechts één team in het hulpprogramma voor beoordeling hebben. U kunt echter meerdere subteams maken. Subteams zijn handig voor het maken van escalatie teams of teams die zijn toegewezen aan de hand van specifieke categorieën van inhoud. U kunt bijvoorbeeld inhoud voor volwassenen verzenden naar een ander team voor meer revisie.

In dit onderwerp wordt uitgelegd hoe subteams maken en snel toewijzen beoordelingen onderweg. U kunt echter [werkstromen](workflows.md) beoordelingen op basis van specifieke criteria toewijzen.

## <a name="go-to-the-teams-setting"></a>Ga naar de Teams instelling ##

Selecteer om te beginnen over het maken van een subteams bepalen de **Teams** optie onder instellingen.

![Instellingen voor team](images/0-teams-1.png)

## <a name="create-subteams"></a>Subteams maken ##

Het standaard-team bevat alle potentiële revisoren; subteams zijn subsets van het standaardteam. U kunt iemand toewijzen aan een subteams bepalen als ze nog geen lid van het standaardteam, dus moet u nu alle revisoren toevoegen aan de standaardteam. Klik op de knop uitnodigen op de pagina Team.

![Gebruikers uitnodigen](images/invite-users.png)

### <a name="1-create-a-subteam"></a>1. Maak een subteams bepalen.
Schuif omlaag op de pagina Team naar de sectie subteams bepalen. Klik op de knop toevoegen subteams bepalen. 

![Toevoegen van subteams bepalen](images/1-teams-1.png)

### <a name="2-name-your-subteam"></a>2. Naam van uw subteams bepalen.
Geef de naam van uw subteams bepalen in het dialoogvenster en klik op opslaan.

![De naam van de subteams bepalen](images/1-Teams-2.PNG)

### <a name="3-assign-members-from-your-default-team"></a>3. Leden van uw standaardteam toewijzen.
Klik op de knop lid toevoegen om de leden van uw standaardteam toewijzen aan een of meer subteams. U kunt alleen bestaande gebruikers toevoegen aan een subteams bepalen. Voor het toevoegen van nieuwe gebruikers die zich niet in het hulpprogramma voor beoordeling uitnodigen u ze met behulp van de knop 'Uitnodigen' op de pagina instellingen voor Team.

![Toewijzen van leden subteams bepalen](images/1-Teams-3.PNG)

## <a name="assign-reviews-to-your-subteams"></a>Beoordelingen toewijzen aan uw subteams ##

Eenmaal hebt u uw subteams gemaakt en de teamleden die zijn toegewezen, kunt u beginnen met de installatiekopie toe te wijzen en tekst naar deze subteams beoordeelt. Dit wordt gedaan vanuit het venster controleren.
Als u wilt een afzonderlijke installatiekopie toewijzen aan een subteams bepalen, klik op het weglatingsteken in de rechterbovenhoek van de afbeelding, selecteert u verplaatsen naar en selecteer een subteams bepalen.

![Afbeelding beoordeling subteam toewijzen](images/3-review-image-subteam-1.png)

## <a name="switch-between-subteams-to-review-assigned-content"></a>Schakelen tussen subteams toegewezen inhoud controleren ##

Als u lid zijn van een of meer subteams bent, kunt u schakelen tussen deze subteams vanuit het Dashboard van de hulpprogramma's voor controle. Selecteer om te zien alle van de huidige in afwachting van revisies die horen bij een subteams bepalen, kies subteams bepalen van het tabblad installatiekopie.

![Schakelen tussen subteams](images/3-review-image-subteam-2.png)
