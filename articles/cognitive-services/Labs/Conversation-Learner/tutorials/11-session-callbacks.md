---
title: Sessie callbacks gebruiken met een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van de callbacks sessie met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 73fb2a3f10476550bbe6bd20c387a1cc2a7727d8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51246477"
---
# <a name="how-to-use-session-callbacks-with-a-conversation-learner-model"></a>Sessie callbacks gebruiken met een model Conversatiecursist

Deze zelfstudie laat zien hoe de callbacks onSessionStart en onSessionEnd.

## <a name="video"></a>Video

[![Zelfstudie-11-Preview](https://aka.ms/cl-tutorial-11-preview)](https://aka.ms/blis-tutorial-11)

## <a name="requirements"></a>Vereisten
In deze zelfstudie vereist dat de `tutorialSessionCallbacks` bot wordt uitgevoerd.

    npm run tutorial-session-callbacks

## <a name="details"></a>Details
In deze zelfstudie bevat informatie over het concept van een sessie, hoe sessies standaard worden verwerkt en hoe u dit gedrag kunt negeren.

Een sessie is een gesprek met de bot. Hiermee schakelt u meerdere kan hebben, maar er zijn geen lange onderbrekingen in de uitwisseling (bijvoorbeeld 30 minuten).  Zie de help-pagina op 'Limieten' voor de standaardduur voor de time-out van sessie.

Als er lange onderbrekingen, klikt u vervolgens de bot wordt omgeleid naar de volgende sessie.  Een nieuwe sessie starten, worden de terugkerende neural network opgenomen in de oorspronkelijke staat.  Standaard ook wist de App alle entiteitswaarden, hoewel dit gedrag kan worden gewijzigd (geïllustreerde hieronder).

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst Model op zelfstudie-11-SessionCallbacks. 

### <a name="entities"></a>Entiteiten

Vier entiteiten zijn gedefinieerd in het model.

![](../media/tutorial11_entities.PNG)

Eén ding te weten is dat BotName een programmatische entiteit.  Deze entiteit wordt door de bot tijdens het starten van een sessie worden ingesteld.

### <a name="actions"></a>Acties

Vier acties zijn gedefinieerd in het model.

![](../media/tutorial11_actions.PNG)

Eerst wordt in deze zelfstudie beschreven hoe voor het beheren van entiteitswaarden aan het begin van de sessie, bijvoorbeeld de entiteit BotName instellen voordat de gebruiker iets zegt.

Ten tweede worden in deze zelfstudie laten zien hoe om vast te leggen van de waarden van een nieuwe sessie naar de volgende.  In deze zelfstudie gaan we ervan uit naam van de gebruiker en telefoonnummer blijven hetzelfde uit een sessie naar de volgende, maar dat kan worden gewijzigd door hun locatie.  We daarom behouden de naam en het telefoonnummer tussen sessies, maar de locatie van de gebruiker wissen.

### <a name="train-dialog"></a>Dialoogvenster van de trein

Hier volgt een voorbeeld van dit dialoogvenster. Dit is een sessie - dat wil zeggen, er zijn geen lange pauzes in dit dialoogvenster.

![](../media/tutorial11_traindialog.PNG)

### <a name="code-for-the-callbacks"></a>Code voor de callbacks

De code voor de callback-methoden is in het bestand: c:\<installedpath > \src\demos\tutorialSessionCallbacks.ts.

![](../media/tutorial11_code.PNG)

Beide methoden zijn optioneel.

- OnSessionStartCallback: deze methode stelt de BotName-entiteit.
- OnSessionEndCallback: u kunt opgeven wat u wilt behouden. Hiermee worden alle entiteiten met uitzondering van de gebruikersnaam en het telefoonnummer van de gebruiker gewist.

### <a name="try-the-bot"></a>Probeer de bot

Schakel over naar de Web-UI en klik op het logboek-dialoogvensters.

1. Voer 'Hallo'.
2. Systeem: "Hallo, ik ben Botty. Wat is jouw naam?' de naam Botty afkomstig van de OnSessionStartCallback heeft.
3. Voer 'jason'.
4. Systeem: ' Hi jason. Wat is uw telefoonnummer?'
5. Voer ' 555-555-5555'.
6. Systeem: 'kunt u zien Botty uw locatie, jason?'
7. Typ "Redmond".

Dit is één sessie. Om een nieuwe sessie starten, moeten we deze sessie beëindigen. 

1. Klik op de sessietime-out. Dit gaat u naar de volgende sessie.
    - De knop 'Sessietime-out' is opgegeven voor foutopsporing.  In een werkelijke-sessie moet een lange pauze plaatsvinden, ongeveer 30 minuten duren.  Zie de help-pagina op 'Limieten' voor de standaardduur voor de time-out van sessie.
1. Voer 'Hallo'.
2. Systeem: 'kunt u zien Botty uw locatie, jason?'
    - Het systeem heeft het nummer van de naam en het telefoonnummer onthouden.
2. Voer een nieuwe locatie: 'Seattle'.
3. Systeem: 'dus jason u bent in Seattle'.
4. Klik op gereed testen.

Laten we gaat u terug naar het Log-dialoogvensters. U ziet dat de laatste conversatie is opgesplitst in twee omdat elke log-dialoogvenster komt met één sessie overeen.  

![](../media/tutorial11_splitdialogs.PNG)

- In de eerste interactie Botty is ingesteld, maar zijn niet de naam en het telefoonnummer.
- De tweede interactie bevat het nummer van de naam en het telefoonnummer.

U hebt nu gezien hoe sessies standaard worden verwerkt, en hoe kunt u het standaardgedrag negeren. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [API-aanroepen](./12-api-calls.md)
