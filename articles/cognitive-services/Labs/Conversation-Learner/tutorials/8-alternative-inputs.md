---
title: Het gebruik van alternatieve invoer met de conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van alternatieve invoer met de conversatie cursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 2214436b193932e5b3b80c190f7754a0436b7ed8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345320"
---
# <a name="how-to-use-alternative-inputs"></a>Het gebruik van alternatieve invoer

Deze zelfstudie laat zien hoe het veld 'alternatieve invoer' gebruiken voor invoer van de gebruiker in de onderwijs-interface.

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
'Alternatieve invoer' is een alternatieve utterances die de gebruiker kan hebben op een bepaald punt in een dialoogvenster training. Alternatieve invoer kunnen u meer compactly variaties van wat een gebruiker kan bijvoorbeeld zonder alle variaties in een dialoogvenster twee afzonderlijke trainings opgeven.

## <a name="steps"></a>Stappen

### <a name="create-the-application"></a>De toepassing maken

1. Klik op nieuwe App in de Webgebruikersinterface
2. Voer in het vak Naam AlternativeInputs. Klik vervolgens op maken.

### <a name="create-an-entity"></a>Maak een entiteit

1. Klik op entiteiten en vervolgens nieuwe entiteit.
2. Voer plaats in de naam van de entiteit.
3. Klik op Maken.

### <a name="create-three-actions"></a>Drie acties maken

1. Klik op acties en vervolgens nieuwe actie
2. In het antwoord, typ 'welke stad wilt u?'.
3. Voer $city in tot uitsluiting entiteiten.
3. Klik op Maken

Maak vervolgens de tweede actie:

1. Klik op acties en vervolgens nieuwe actie.
3. In het antwoord, typt u 'het in $city mooi weer is waarschijnlijk'.
4. Vereiste entiteiten invoeren $city.
4. Klik op Maken.

De derde actie maken:

1. Klik op acties en vervolgens nieuwe actie.
3. Typ 'het verzoek tot het weer Try-antwoord.
    - Dit zou zijn in het antwoord op de vraag van de gebruiker, zoals 'hoe kan het systeem?'
4. Voer $city in tot uitsluiting entiteiten.
4. Klik op Maken

U hebt nu de drie acties.

### <a name="train-the-bot"></a>De bot trainen

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
2. Typ 'Wat is het weer'.
3. Score acties op en selecteer 'welke stad wilt u?'
2. Voer 'denver'.
3. Dubbelklik op 'denver, en selecteer plaats.
    - Dit gemarkeerd als een entiteit plaats.
5. Klik op Score-acties
    - Houd er rekening mee dat denver nu aanwezig in de entiteit plaats is. 
6. Selecteer 'het in $city mooi weer is waarschijnlijk'.
7. Klik op het onderwijs gereed.

Voeg een ander voorbeeld van dit dialoogvenster:

1. Klik op nieuwe actie en vervolgens nieuwe Train dialoogvenster.
2. Typ 'Wat kunt u doen?'.
3. Score acties op en selecteert u 'het verzoek tot het weer Try-
2. Voer 'Wat is het weer in Haarlem'.
3. Dubbelklik op "seattle" en selecteer plaats.
    - Dit gemarkeerd als een entiteit plaats.
5. Klik op Score-acties
    - Houd er rekening mee dat seattle nu aanwezig in de entiteit plaats is. 
6. Selecteer 'het in $city mooi weer is waarschijnlijk'.
7. Klik op het onderwijs gereed.

Wat gebeurt er als de gebruiker iets semantisch vergelijkbaar met de bovenstaande:

1. Klik op nieuwe actie en vervolgens nieuwe Train dialoogvenster.
2. Typ 'help'.
3. Klik op Score acties.
    - Houd er rekening mee dat de scores voor de twee mogelijke antwoorden zeer sluiten zijn. Dit vertellen dat over de grens tussen de twee acties is hetzelfde als het model.
6. Klik op afbreken onderwijs en te bevestigen.

![](../media/tutorial8_closescores.png)

In dit geval zou het handig om toe te voegen alternatieve invoer voor dialoogvensters. Als u het onderwijs doet, kunt u deze toevoegen. U kunt ook teruggaan en deze later toevoegen.

2. Klik op 'Wat kunt u doen?' in de trein dialoogvensters.
2. Klik in het dialoogvenster op 'Wat kunt u doen?' om deze te selecteren.
    1. Voer in de alternatieve invoer toevoegen in het rechterdeelvenster onder entiteit detectie, een aantal alternatieven:
    1. Voer 'Wat zijn mijn keuzes?'
    2. Voer 'Ik wil mijn keuzes'.
    3. Voer 'help'
    1. Klik op wijzigingen verzenden.


![](../media/tutorial8_helpalternates.png)

2. Klik nu op 'wat het weer in Haarlem is'.
    1. Voer 'Prognose voor seattle' in de alternatieve invoer toevoegen.
    2. Dubbelklik op "seattle" en selecteer plaats. Houd er rekening mee dat de entiteiten voor alternatieve invoer moeten aanwezig zijn en dezelfde set van entiteiten hebben. Het is goed als de inhoud van de diensten verschilt.
    3. In de alternatieve invoer toevoegen, voer 'wordt het regen vandaag in denver'.
    4. Klik op 'denver, en selecteer plaats.
    5. Klik op wijzigingen verzenden en uitgevoerd.


We voegen alternatieve invoer voor het eerste dialoogvenster:

1. Klik op de trein dialoogvensters.
2. Klik in het dialoogvenster beginnen met 'wat het weer is'.
2. Klik op 'Wat is het weer' in het linkerdeelvenster:
    1. Voer 'weer prognose' in de alternatieve toevoegen-invoer.
    2. Voer 'wordt het regen?'
    3. Klik op indienen wijzigingen.
4. Klik op 'denver' in het linkerdeelvenster:
    1. Voer 'voor denver' in de alternatieve toevoegen-invoer.
    2. Voer 'Prognose voor austin'.
        - Houd er rekening mee dat de volledige tekst is geselecteerd. Klik op de wachtwoordzin op te geven en vervolgens de rode x. Vervolgens austin selecteren en klik op de plaats.
        - Klikt u op wijzigingen verzenden
    1. Klik op het gereed waardoor het model opnieuw trainen.

![](../media/tutorial8_altcities.png)

We gaan de variaties:

1. Klik op nieuwe Train dialoogvenster.
2. Typ 'welke bent u mogelijkheden'.
3. Klik op Score acties.
    - Houd er rekening mee dat de scores zijn nu meer bepalende op de volgende actie waarmee wordt aangegeven van de zekerheid van het model.
2. Selecteer 'Probeer vragen weer'.
6. Klik op gereed onderwijs

Nu hebt u gezien hoe alternatieve invoer kunnen worden gebruikt om aan te geven van de gebruiker mogelijk hebben laten weten dat meer. Ze helpen u Vermijd het maken van veel dialoogvensters, die op vele manieren hetzelfde, zijn door ze in een dialoogvenster met één samenvouwen en wat de gebruiker kan zeggen inventariseren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Logboek dialoogvensters](./9-log-dialogs.md)
