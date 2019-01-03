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
ms.openlocfilehash: b6ea1ee4eb8b55d2da4069ef19a268ec68f49cb4
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796746"
---
# <a name="how-to-use-alternative-inputs"></a>Het gebruik van andere invoer

In deze zelfstudie laat zien hoe het veld Alternatieve invoer gebruiken voor uitingen van de gebruiker in de interface voor onderwijs.

## <a name="video"></a>Video

[![Zelfstudie alternatieve invoer-Preview](https://aka.ms/cl_Tutorial_v3_AlternativeInputs_Preview)](https://aka.ms/cl_Tutorial_v3_AlternativeInputs)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Alternatieve invoer zijn alternatieve, dezelfde gebruiker uitingen die de gebruiker kan hebben op een bepaald punt in een dialoogvenster training. Deze alternatieve invoer kunnen u meer compactly variaties van uitingen opgeven zonder voor elke variatie in afzonderlijke trainings-dialoogvensters.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het Model maken

1. In de Web-gebruikersinterface, klikt u op "Nieuwe Model."
2. Typ in het veld 'Name', 'AlternativeInputs' en druk op enter.
3. Klik op de knop 'Maken'.

### <a name="entity-creation"></a>Entiteit maken

1. In het linkerdeelvenster klikt u op "Entiteiten" en vervolgens de knop 'Nieuwe entiteit'.
2. Selecteer 'Aangepaste getraind' voor het "entiteitstype'.
3. Typ 'plaats' voor "Naam van de entiteit."
4. Klik op de knop 'Maken'.

Nu gaan we drie acties te maken.

### <a name="create-the-first-action"></a>De eerste actie maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u "Welke plaats?"
3. Typ in het veld 'Verwachte entiteit in antwoord op gebruiker...', 'plaats'.
4. Typ in het veld 'Diskwalificeren voorziet', 'plaats'.
5. Klik op de knop 'Maken'.

### <a name="create-the-second-action"></a>De tweede actie maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u "het weer in $city is waarschijnlijk zonnige."
3. Klik op de knop 'Maken'.

### <a name="create-the-third-action"></a>De derde actie maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u "Probeert het weer wordt gevraagd."
3. Typ in het veld 'Diskwalificeren voorziet', 'plaats'.
4. Klik op de knop 'Maken'.

U hebt nu de drie acties.

### <a name="train-the-model"></a>Het Model te trainen

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'Wat is het weer?'
3. Klik op de knop 'Acties Score'.
4. Selecteer het antwoord, "Welke plaats?"
5. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Denver"
6. Klik op de knop 'Acties Score'.
7. Selecteer het antwoord, "het weer in Denver is waarschijnlijk zonnige."
8. Klik op de knop 'Opslaan'.

Laten we trainen het model door het maken van een ander dialoogvenster van de trein meer.

### <a name="second-model-train-dialog"></a>Dialoogvenster van de trein tweede Model

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Kunt u doen?"
3. Klik op de knop 'Acties Score'.
4. Selecteer het antwoord, "Probeer weer wordt gevraagd."
5. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'Wat is het weer in Seattle?'
6. Klik op "Seattle" en klik vervolgens op 'plaats' uit de lijst met entiteiten.
7. Klik op de knop 'Acties Score'.
8. Selecteer het antwoord, "het weer in Seattle is waarschijnlijk zonnige."
9. Klik op de knop 'Opslaan'.

### <a name="third-model-train-dialog-using-alternative-input"></a>Derde Model trainen dialoogvenster met behulp van alternatieve invoer

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "help"
3. Klik op de knop 'Acties Score'.
    - Het Model is niet bekend met de beste optie, zodat deze de hoogste percentiel standaard kiest.
4. Klik op de knop 'Onderwijs afbreken' en klik vervolgens op "Confirm" knop.

![](../media/tutorial8_closescores.png)

Laten we het systeem met behulp van alternatieve invoergegevens beter afstemmen. U kunt alternatieve invoer toevoegen tijdens het onderwijs of hoger.

5. In het linkerdeelvenster klikt u op 'Train-dialoogvensters' en selecteer vervolgens "Kunt u doen?" in de lijst van de trein-dialoogvensters.
6. Klik op de 'kunt u doen?" utterance in het deelvenster chat.
7. In de ' alternatieve invoer toevoegen...' enter veld, type 'help' en druk op.
8. Klik op de knop 'Wijzigingen opslaan'.

![](../media/tutorial8_helpalternates.png)

Laten we een andere alternatieve invoer voor het afhandelen van Houston toevoegen.

9. Klik op de 'Wat is het weer in Seattle?' utterance in het deelvenster chat.
10. In het veld 'Toevoegen... alternatieve invoer' type 'Prognose voor Houston' en druk op enter.
    - De fout bericht hoogtepunten de feitelijke alternatieve invoer moeten semantisch gelijk en bevatten de dezelfde entiteiten als de oorspronkelijke utterance; niet alleen de dezelfde waarden van entiteiten. De aanwezigheid van dezelfde entiteiten is vereist.
11. Klik op 'Houston' en 'city' selecteert in de lijst met entiteiten.
12. In het veld 'Toevoegen... alternatieve invoer' type 'Prognose voor Seattle"en druk op enter.
13. Klik op "Seattle" en selecteer 'plaats' in de lijst met entiteiten.
14. Klik op de knop 'Wijzigingen opslaan'.
15. Klik op de knop 'Opslaan bewerken'.

### <a name="testing-the-model"></a>Het Model testen

1. In het linkerdeelvenster, klik op "Log-dialoogvensters" en vervolgens de "Log dialoogvenster Nieuwe."
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u in 'help mij'
3. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'forecast voor Denver'

![](../media/tutorial8_altcities.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Logboek-dialoogvensters](./11-log-dialogs.md)
