---
title: Bekijk de tekst in het Azure Content Moderator - Content Moderator
description: Meer informatie over het bekijken van tekst in de Content Moderator om te zien van de score en tags gedetecteerd. Gebruik de informatie die u wilt weten of inhoud geschikt zijn moet.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 005e6ed853cbc6013d74e7ff479097dcbb0a1043
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54259560"
---
# <a name="let-human-reviewers-review-text"></a>Menselijke revisoren Bekijk tekst

U kunt Azure Content Moderator gebruiken om te controleren van tekst met behulp van scores en tags gedetecteerd. Gebruik de informatie om te bepalen of inhoud geschikt is. 

## <a name="select-or-enter-the-text-to-review"></a>Selecteer of Voer de tekst in om te controleren

Selecteer in de Content Moderator, de **probeer** tabblad. Selecteer vervolgens de **tekst** optie om naar het startscherm voor beheer van tekst te gaan. Voer een tekst of dien de standaardvoorbeeldtekst voor beheer van geautomatiseerde tekst. U kunt een maximum van 1024 tekens bestaan.

## <a name="get-ready-to-review-results"></a>Aan de slag met de resultaten bekijken

Het beoordelingsprogramma roept eerst de tekst toezicht-API. Er wordt vervolgens beoordelingen van teksten gegenereerd met behulp van de gedetecteerde tags. Het beoordelingsprogramma komt overeen met de scoreresultaten voor de aandacht van uw team.

## <a name="review-text-results"></a>Bekijk de resultaten van tekst

Gedetailleerde resultaten worden weergegeven in de windows. De resultaten zijn gedetecteerde tags en voorwaarden die zijn geretourneerd door de tekst toezicht-API. Als u wilt in-/ uitschakelen van een label selecteren de status, selecteert u de tag. Ook kunt u werken met een aangepaste labels die u hebt gemaakt.

![Schermafbeelding van de beoordeling hulpprogramma weergegeven tekst in een Chrome-browservenster gemarkeerd](../images/reviewresults_text.png)
