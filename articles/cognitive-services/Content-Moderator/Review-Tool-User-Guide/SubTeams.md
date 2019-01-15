---
title: Teams en subteams in de Content Moderator-API - Content Moderator beheren
titlesuffix: Azure Cognitive Services
description: Informatie over het gebruik van teams en subteams in de Content Moderator-API voor Cognitive Services.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: e45e06bdcc69dc88d8c979bfbb5a0974f7284b22
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266226"
---
# <a name="manage-review-teams-and-subteams"></a>Controle-teams en subteams beheren

U moet een Team maken voordat u met Content Moderator. Wanneer u zich omhoog en de naam van uw team, is dit team wordt uw standaardteam. U kunt slechts één team in het controlehulpprogramma hebben. U kunt echter meerdere subteams maken. Subteams zijn handig voor het maken van escalatie teams of teams die zijn toegewezen aan het controleren van bepaalde categorieën van inhoud. Bijvoorbeeld, kunt u inhoud voor volwassenen verzenden naar een ander team voor meer controle.

In dit onderwerp wordt uitgelegd hoe u subteams maakt en beoordelingen op elk gewenst moment snel wilt toewijzen. U kunt echter [werkstromen](workflows.md) om toe te wijzen van beoordelingen op basis van specifieke criteria.

## <a name="go-to-the-teams-setting"></a>Ga naar de Teams instelling

Om te beginnen op het maken van een subteams bepalen, selecteert u de **Teams** optie onder instellingen.

![Instellingen voor team](images/0-teams-1.png)

## <a name="create-subteams"></a>Subteams maken

De standaard-team bevat alle mogelijke revisoren; subteams zijn subsets van de standaardteam. U toewijzen niet iemand aan een subteams bepalen als ze nog geen leden van de standaardteam, dus moet u nu een van de revisoren toevoegen aan de standaardteam. Klik op de knop uitnodigen op de pagina Team.

![Gebruikers uitnodigen](images/invite-users.png)

### <a name="create-a-subteam"></a>Maken van een subteams bepalen
Blader door de teampagina naar de sectie subteams bepalen. Klik op de knop toevoegen subteams bepalen. 

![Toevoegen van subteams bepalen](images/1-teams-1.png)

### <a name="name-your-subteam"></a>Naam van uw subteams bepalen
Geef de naam van uw subteams bepalen in het dialoogvenster en klik op opslaan.

![De naam van de subteams bepalen](images/1-Teams-2.PNG)

### <a name="assign-members-from-your-default-team"></a>Leden van uw standaardteam toewijzen.
Klik op de knop Add Member leden van uw standaardteam toewijzen aan een of meer subteams. U kunt alleen bestaande gebruikers toevoegen aan een subteams bepalen. Voor het toevoegen van nieuwe gebruikers die zich niet in het beoordelingsprogramma, nodigen u ze met behulp van de knop 'Uitnodigen' op de pagina Team-instellingen.

![Subteams bepalen leden toewijzen](images/1-Teams-3.PNG)

## <a name="assign-reviews-to-your-subteams"></a>Beoordelingen toewijzen aan uw subteams

Nadat u hebt uw subteams gemaakt en de teamleden die zijn toegewezen, kun u de installatiekopie toe te wijzen en tekst coderevisies tot deze subteams. Dit wordt gedaan via de beoordeling-venster.
Als u een installatiekopie van een afzonderlijke toewijzen aan een subteams bepalen wilt, klikt u op het beletselteken in de rechterbovenhoek van de afbeelding, selecteert u verplaatsen naar en selecteer een subteams bepalen.

![Afbeelding beoordeling subteam toewijzen](images/3-review-image-subteam-1.png)

## <a name="switch-between-subteams-to-review-assigned-content"></a>Schakelen tussen subteams om toegewezen inhoud te controleren

Als u een lid van een of meer subteams, kunt u schakelen tussen deze subteams vanuit het hulpprogramma voor beoordeling dashboard. Als u wilt zien welke van de huidige in afwachting van beoordelingen die behoren tot een subteams bepalen, selecteert u kiest subteams bepalen op het tabblad installatiekopie.

![Schakelen tussen subteams](images/3-review-image-subteam-2.png)
