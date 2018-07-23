---
title: Demo startprogramma Conversatiecursist - model, virtuele realiteit voor Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het maken van een demo Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 896ec007c03e30e5c20a5344430be040271bc00b
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171142"
---
# <a name="demo-virtual-reality-app-launcher"></a>Demo: Startprogramma voor Virtual reality

Deze demonstratie ziet u een virtual reality van model startprogramma voor apps, die ondersteuning biedt voor opdrachten, zoals "start Skype en in op het prikbord plaatsen." Een gebruiker moet bijvoorbeeld een app-naam en locatie om de app starten. Starten van het model wordt verwerkt door een API-aanroep. Wanneer een app-naam van de gebruiker wordt herkend, wordt de entityDetectionCallback gecontroleerd of de aangevraagde app, overeenkomt met een of meer apps in de lijst met geïnstalleerde apps. De aanvraag verwerkt wanneer de aangevraagde app niet is geïnstalleerd en de appnaam van de is niet eenduidig (komt overeen met meer dan één geïnstalleerde app).

## <a name="video"></a>Video

[![Demo VR App-overzicht](http://aka.ms/cl-demo-vrapp-preview)](http://aka.ms/blis-demo-vrapp)

## <a name="requirements"></a>Vereisten

In deze zelfstudie is vereist dat de bot VRAppLauncher wordt uitgevoerd:

    npm run demo-vrapp
    
### <a name="open-the-demo"></a>Open de demo

Klik in de lijst Model van de web-UI op VRAppLauncher. 

## <a name="entities"></a>Entiteiten

We hebt vier entiteiten gemaakt:

- Toepassingsnaam: bijvoorbeeld Skype
- PlacementLocation: voor de voorbeeld-prikbord
- UnknownAppName: een programmatische entiteit die het systeem wordt ingesteld wanneer deze niet wordt herkend door de naam van een entiteit de gebruiker aangeeft dat deze, bijvoorbeeld omdat deze nog niet is geïnstalleerd.
- DisAmbigAppNames: een matrix met twee of meer geïnstalleerde app-namen die overeenkomen met wat de gebruiker al zei. 

![](../media/tutorial_vrapplauncher_entities.PNG)

### <a name="actions"></a>Acties

We hebben een reeks acties met een API LaunchApp, die de aanroep van de functie om een app te starten wordt gestart met de naam gemaakt.

![](../media/tutorial_vrapplauncher_actions.PNG)

### <a name="training-dialogs"></a>Dialoogvensters voor training
We hebben een aantal training dialoogvensters gedefinieerd.

![](../media/tutorial_vrapplauncher_dialogs.PNG)

Bijvoorbeeld: we proberen een sessie voor onderwijs.

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
1. Voer 'Hallo'.
2. Klik op Score actie.
3. Klik op selecteren 'welke app u wilt starten?'
4. Voer 'outlook'.
    - LUIS herkent deze als een entiteit.
5. Klik op Score acties.
3. Klik om te selecteren, waar wilt u deze geplaatst?'
4. Voer 'op het prikbord'.
    - LUIS herkent deze als een PlacementLocation.
2. Voer Score acties.
6. Selecteer 'LaunchApp'
7. Systeem: 'begin outlook op het prikbord'.
    - Dit veroorzaakt een API-aanroep. De code voor deze aanroep is C:\<\installedpath > \src\demos\demoVRAppLauncher.ts. Echter, het daadwerkelijk bevat de code voor het starten van Outlook voor deze demo.
    - Hiermee schakelt u de entiteiten AppName en PlacementLocation. De geeft als resultaat de bovenstaande tekenreeks als antwoord.
4. Klik op het onderwijs gereed.

![](../media/tutorial_vrapplauncher_callbackcode.PNG)

Laten we beginnen een andere trainingssessie voor het verwerken van onbekende en niet-eenduidige entiteiten.

1. Klik op nieuwe dialoogvenster van de trein.
1. Voer 'start OneNote'. 
    - Het model herkent OneNote als een app-naam. De `EntityDetectionCallback` functie gedefinieerd in de code wordt de naam die is ingevoerd door de gebruiker naar een app-naam door deze aan de lijst met Apps die zijn gedefinieerd in de code die overeenkomt met omgezet. Deze retourneert vervolgens de set met alle overeenkomende apps. 
    - Als de lijst komt overeen met nul is, betekent dit dat de app niet is geïnstalleerd. Alles is ondergebracht in unknownAppName.
    - Als er meer dan één app, kopieert deze naar de `DisambigAppNames` en wist u de entiteit AppName.
2. Klik op Score actie.
3. Klik om te selecteren 'Sorry, ik weet niet de app $UknownAppName'.
4. Voer 'start Amazon'. We proberen de ander pad.
5. Klik op Score acties.
    - Amazon-Video's en muziek van Amazon worden nu `DisambigAppNames` geheugen en OneNote is gewist.
3. Klik om te selecteren, er zijn aantal apps dat net als geluid...'
    - De score is niet erg hoog omdat we een paar training dialoogvensters tot nu alleen hebt gedefinieerd. Meer training dialoogvensters definiëren zou het model meer doorslaggevend maken.
2. Voer Score acties.
4. Klik op het onderwijs gereed.

U hebt nu gezien hoe u entiteit resolutie doet. De demo ook API callbacks bevatten en in een sjabloon voor het verzamelen van gegevens en de juiste actie dienovereenkomstig te controleren op aanwezigheid en dubbelzinnigheid hebt u geleerd.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Implementatie van een bot Conversatiecursist](../deploy-to-bf.md)
