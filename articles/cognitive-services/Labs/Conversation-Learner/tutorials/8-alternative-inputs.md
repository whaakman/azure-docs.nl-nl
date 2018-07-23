---
title: Alternatieve invoer gebruiken met Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van alternatieve invoer met Conversatiecursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8d3b3f419ceacbb9a6fe2b19cf68ea6873de536f
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171014"
---
# <a name="how-to-use-alternative-inputs"></a>Het gebruik van andere invoer

In deze zelfstudie laat zien hoe het veld "alternatieve invoer" gebruiken voor de invoer van de gebruiker in de interface voor onderwijs.

## <a name="video"></a>Video

[![Zelfstudie 8-Preview](http://aka.ms/cl-tutorial-08-preview)](http://aka.ms/blis-tutorial-08)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
'Invoer van de alternatieve' is een alternatieve uitingen die de gebruiker kan hebben op een bepaald punt in een dialoogvenster training. Alternatieve invoer kunnen u meer compactly opgeven variaties van wat een gebruiker zeggen kan, zonder dat om elke variatie in een afzonderlijke trainings-dialoogvenster weer te geven.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het model maken

1. Klik op Nieuw Model in de Web-UI
2. Voer in het vak Naam AlternativeInputs. Klik vervolgens op maken.

### <a name="create-an-entity"></a>Een entiteit maken

1. Klik op entiteiten, en vervolgens nieuwe entiteit.
2. Voer in de naam van de entiteit, plaats.
3. Klik op Maken.

### <a name="create-three-actions"></a>Drie acties maken

1. Klik op acties en vervolgens nieuwe actie
2. Typ 'welke stad wilt u dat?-antwoord.
3. Voer $city in diskwalificeren van entiteiten.
3. Klik op Maken

Maak vervolgens de tweede actie:

1. Klik op acties en vervolgens nieuwe actie.
3. Typ 'het weer in $city is waarschijnlijk zonnige' in het antwoord.
4. Vereiste entiteiten, voer $city.
4. Klik op Maken.

De derde actie maken:

1. Klik op acties en vervolgens nieuwe actie.
3. Typ 'Try waarin wordt gevraagd voor het weer-antwoord.
    - Dit zou zijn in reactie op vraag van de gebruiker, zoals 'wat kan het systeem doen?'
4. Voer $city in diskwalificeren van entiteiten.
4. Klik op Maken

U hebt nu de drie acties.

### <a name="train-the-bot"></a>De bot te trainen

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
2. Typ 'Wat is het weer'.
3. Klik op Score acties en selecteer 'welke stad wilt u dat?'
2. Voer 'denver'.
3. Dubbelklik op 'denver' en selecteer plaats.
    - Hiermee markeert als een entiteit plaats.
5. Klik op Score acties
    - 'denver' is nu aanwezig zijn in de entiteit plaats. 
6. Selecteer 'het weer in $city is waarschijnlijk zonnige'.
7. Klik op het onderwijs gereed.

Voeg een ander voorbeeld van dit dialoogvenster:

1. Klik op nieuwe actie en vervolgens nieuwe dialoogvenster van de trein.
2. Typ 'kunt u doen?'.
3. Klik op Score acties en selecteer 'Misschien dat voor het weer'
2. Voer 'Wat is het weer in seattle'.
3. Dubbelklik op "seattle" en selecteer plaats.
    - Hiermee markeert als een entiteit plaats.
5. Klik op Score acties
    - 'seattle' is nu aanwezig zijn in de entiteit plaats. 
6. Selecteer 'het weer in $city is waarschijnlijk zonnige'.
7. Klik op het onderwijs gereed.

We gaan nu kijken wat er gebeurt als de gebruiker iets semantisch vergelijkbaar met het bovenstaande positiefs zegt:

1. Klik op nieuwe actie en vervolgens nieuwe dialoogvenster van de trein.
2. Typ 'help'.
3. Klik op Score acties.
    - De scores voor de twee mogelijke antwoorden zijn zeer sluiten. Dit kan worden achterhaald dat het model wordt verward over de grens tussen de twee acties.
6. Klik op dit Onderwijzen en bevestigen.

![](../media/tutorial8_closescores.png)

In dit geval zou het helpen om toe te voegen alternatieve invoer voor dialoogvensters. Als u het onderwijs uitvoert, kunt u deze toevoegen. U kunt ook teruggaan en deze later toevoegen.

2. Klik op 'Kunt u doen?' in de dialoogvensters van de trein.
2. In het dialoogvenster, klikt u op 'kunt u doen?' om deze te selecteren.
    1. Voer in het rechterdeelvenster onder de detectie van de entiteit, in de alternatieve invoer toevoegen een aantal alternatieven:
    1. Voer 'Wat zijn mijn opties?'
    2. Voer 'Ik wil mijn keuzen'.
    3. Voer 'help'
    1. Klik op wijzigingen verzenden.


![](../media/tutorial8_helpalternates.png)

2. Klik nu op 'wat het weer in seattle is'.
    1. In de alternatieve invoer toevoegen, voer 'Prognose voor seattle'.
    2. Dubbelklik op "seattle" en selecteer plaats. De entiteiten voor alternatieve invoer moeten aanwezig zijn en hebben dezelfde set entiteiten. Het is prima als de inhoud van de entiteiten verschilt.
    3. Voer 'wordt deze regen vandaag nog in denver' in de alternatieve invoer toevoegen.
    4. Klik op 'denver' en selecteer plaats.
    5. Klik op wijzigingen verzenden en uitgevoerd.


Laten we andere invoer naar het eerste dialoogvenster toevoegen:

1. Klik op de trein dialoogvensters.
2. Klik in het dialoogvenster beginnen met 'wat het weer is'.
2. Klik op 'Wat is het weer' in het linkerdeelvenster:
    1. In andere invoer toevoegen, voert u 'weersvoorspelling'.
    2. Voer 'wordt deze regen?'
    3. Klik op Submit wijzigingen.
4. Klik op 'denver' in het linkerdeelvenster:
    1. Voer in de alternatieve invoer toevoegen, 'voor denver'.
    2. Voer 'Prognose voor austin'.
        - De volledige woordgroep wordt gemarkeerd. Klik op de woordgroep en vervolgens de rode x. Vervolgens selecteert u austin, en klik op plaats.
        - Klik op Submit wijzigingen
    1. Klikt u op Gereed, waardoor het model trainen.

![](../media/tutorial8_altcities.png)

We proberen de variaties:

1. Klik op nieuwe dialoogvenster van de trein.
2. Typ 'Wat zijn u mogelijkheden'.
3. Klik op Score acties.
    - De scores zijn nu meer doorslaggevend op de volgende actie waarmee de zekerheid van het model wordt aangegeven.
2. Selecteer 'Misschien voor weerberichten'.
6. Klik op gereed onderwijs

U hebt nu gezien hoe alternatieve invoer kunnen worden gebruikt om aan te geven van de dingen die de gebruiker mogelijk heeft gezegd. Ze helpen u te voorkomen dat het maken van veel dialoogvensters, die op vele manieren hetzelfde, zijn door ze in een enkel dialoogvenster samenvouwen en inventariseren van wat de gebruiker kan zeggen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Logboek-dialoogvensters](./9-log-dialogs.md)
