---
title: Sessie-retouraanroepen gebruiken met de toepassing van een conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van de sessie retouraanroepen aan een conversatie cursist-toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f8970620c1f0f87ccae13d031092a048144ffb19
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345286"
---
# <a name="how-to-use-session-callbacks-with-a-conversation-learner-application"></a>Het gebruik van retouraanroepen sessie met een conversatie cursist-toepassing

Deze zelfstudie laat zien hoe de callbacks onSessionStart en onSessionEnd.

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de bot 'tutorialSessionCallbacks.ts' wordt uitgevoerd.

    npm run tutorial-session-callbacks

## <a name="details"></a>Details
Deze zelfstudie bevat informatie over het concept van een sessie, hoe sessies standaard worden verwerkt en hoe u dit gedrag kunt negeren.

Een sessie is een conversatie met de bot. Hiermee schakelt u meerdere kan hebben, maar er zijn geen lange onderbrekingen in de conversatie (bijvoorbeeld: 30 minuten).  Zie de help-pagina van 'Limieten' voor de standaardduur voor de time-out van sessie.

Als er lange onderbrekingen, wordt de bot Ga naar de volgende sessie.  Een nieuwe sessie starten, worden de periodieke neurale netwerk wordt in de initiële status geplaatst.  Standaard deze ook worden gewist alle waarden van de entiteit, hoewel dit gedrag kan worden gewijzigd (geïllustreerde hieronder).

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst met Apps op zelfstudie-11-SessionCallbacks. 

### <a name="entities"></a>Entiteiten

We hebben vier entiteiten gedefinieerd in de toepassing.

![](../media/tutorial11_entities.PNG)

Hierbij moet u onthouden is dat BotName een programmatische entiteit.  Hierdoor wordt ingesteld door de bot tijdens het starten van een sessie.

### <a name="actions"></a>Acties

Wij hebben vier acties gemaakt. 

![](../media/tutorial11_actions.PNG)

Eerst wordt deze zelfstudie beschreven hoe waarmee waarden aan het begin van de sessie--entiteit bijvoorbeeld de entiteit BotName instellen voordat de gebruiker zegt.

Ten tweede in deze zelfstudie wordt beschreven hoe om vast te leggen van de waarden van een nieuwe sessie naar de volgende.  In deze zelfstudie gaan we ervan uit naam van de gebruiker en telefoonnummer blijven hetzelfde van een nieuwe sessie naar de volgende, maar dat de locatie mag wijzigen.  Daarom persistent we is de naam en het telefoonnummer voor sessies, maar de locatie van de gebruiker wissen.

### <a name="train-dialog"></a>Dialoogvenster Train

Hier volgt een voorbeeld van dit dialoogvenster. Dit is één sessie - dat wil zeggen, er zijn geen lange onderbrekingen in dit dialoogvenster.

![](../media/tutorial11_traindialog.PNG)

### <a name="code-for-the-callbacks"></a>Code voor de callbacks

De code voor de callback-methoden is in het bestand: c:\<installedpath > \src\demos\tutorialSessionCallbacks.ts.

![](../media/tutorial11_code.PNG)

Beide methoden zijn optioneel.

- OnSessionStartCallback: deze methode stelt de BotName-entiteit.
- OnSessionEndCallback: u kunt opgeven wat u wilt wissen. Hiermee worden alle entiteiten met uitzondering van gebruikersnaam en het telefoonnummer gebruiker gewist.

### <a name="try-the-bot"></a>Probeer de bot

Overschakelen naar de Webgebruikersinterface en klik op logboek-dialoogvensters.

1. Voer 'Hallo'.
2. Systeem: "Hallo, ik ben Botty. Wat is de naam van uw?' waarvan de naam Botty afkomstig van de OnSessionStartCallback heeft.
3. Voer 'jason'.
4. Systeem: ' High jason. Wat is uw telefoonnummer?'
5. Voer ' 555-555-5555'.
6. Systeem: 'kunt u zien Botty uw locatie en jason?'
7. Typ "Redmond".

Dit is één sessie. Voor het starten van een nieuwe sessie moet om deze sessie te beëindigen. 

1. Klik op de sessietime-out. Dit gaat u naar de volgende sessie.
    - De knop 'Sessietime-out' is opgegeven voor foutopsporing.  In een werkelijke sessie moet een lange pauze zou optreden van ongeveer 30 minuten.  Zie de help-pagina van 'Limieten' voor de standaardduur voor de time-out van sessie.
1. Voer 'Hallo'.
2. Systeem: 'kunt u zien Botty uw locatie en jason?'
    - Het systeem heeft de naam en het telefoonnummer aantal onthouden.
2. Voer een nieuwe locatie: 'Seattle'.
3. Systeem: 'dus jason u zich in Haarlem'.
4. Klik op gereed testen.

Laten we Ga terug naar het Log-dialoogvensters. U ziet dat de laatste conversatie is opgesplitst in twee omdat elk logboek dialoogvenster komt met één sessie overeen.  

![](../media/tutorial11_splitdialogs.PNG)

- In de eerste interactie Botty is ingesteld, maar zijn niet de naam en het telefoonnummer.
- De tweede interactie ziet u het nummer van de naam en het telefoonnummer.

U hebt nu gezien hoe sessies standaard worden verwerkt en hoe kunt u het standaardgedrag vervangen. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [API-aanroepen](./12-api-calls.md)
