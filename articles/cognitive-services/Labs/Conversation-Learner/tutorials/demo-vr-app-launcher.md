---
title: Demo conversatie cursist realiteit van toepassing, virtuele app linksboven - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het maken van een demo conversatie cursist toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 3e41125bf7da9ee64d666d22cb275af01af54012
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345304"
---
# <a name="demo-virtual-reality-app-launcher"></a>Demo: Virtuele realiteit app starten

Deze demo ziet u een virtuele realiteit startprogramma voor toepassingen, die opdrachten ondersteunen, zoals 'Skype starten en in op de achtergrond plaatsen.' Er moet een gebruiker in te spreken van een app-naam en locatie om de app te starten. Starten van de App wordt verwerkt door een API-aanroep. Wanneer een app-naam van de gebruiker wordt herkend, wordt de entityDetectionCallback controleert of de aangevraagde app overeenkomt met een of meer apps in de lijst met geïnstalleerde apps. De aanvraag worden verwerkt wanneer de aangevraagde app is niet geïnstalleerd en de appnaam van de is niet eenduidig (komt overeen met meer dan één geïnstalleerde app).

## <a name="requirements"></a>Vereisten

Deze zelfstudie vereist dat de bot VRAppLauncher wordt uitgevoerd

    npm run demo-vrapp

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst met Apps van de webgebruikersinterface, op VRAppLauncher. 

## <a name="entities"></a>Entiteiten

Er vier entiteiten gemaakt:

- AppName: bijvoorbeeld Skype
- PlacementLocation: voor voorbeeld wanden
- UnknownAppName: een programmatische entiteit die het systeem wordt ingesteld wanneer deze niet wordt herkend door de naam van een entiteit de gebruiker aangeeft, bijvoorbeeld omdat het is niet geïnstalleerd.
- DisAmbigAppNames: Er is een matrix van twee of meer geïnstalleerde app-namen die overeenkomen met wat de gebruiker genoemd. 

![](../media/tutorial_vrapplauncher_entities.PNG)

### <a name="actions"></a>Acties

Wij hebben een reeks acties met een API aangeroepen LaunchApp die de functie-aanroep om een app te starten wordt gestart.

![](../media/tutorial_vrapplauncher_actions.PNG)

### <a name="training-dialogs"></a>Training dialoogvensters
We hebben een aantal training dialoogvensters gedefinieerd.

![](../media/tutorial_vrapplauncher_dialogs.PNG)

Als u bijvoorbeeld een sessie onderwijs eens te proberen.

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
1. Voer 'Hallo'.
2. Klik op de actie Score.
3. Klik op selecteren 'welke app u wilt starten?'
4. Voer 'outlook'.
    - Houd er rekening mee dat LUIS als een entiteit herkent.
5. Klik op Score acties.
3. Schakel 'waar wilt u deze geplaatst?'
4. Voer 'op de achtergrond'.
    - Houd er rekening mee dat LUIS als een PlacementLocation herkent.
2. Voer Score acties.
6. Selecteer 'LaunchApp'
7. Systeem: 'outlook starten op de achtergrond'.
    - Houd er rekening mee dat dit een API-aanroep veroorzaakt. De code voor deze aanroep is op C:\<\installedpath > \src\demos\demoVRAppLauncher.ts. Het daadwerkelijk geen de logica outlook voor deze demo echter starten.
    - Hiermee wist u de toepassingsnaam en PlacementLocation entiteiten. De retourneert de bovenstaande tekenreeks als antwoord.
4. Klik op het onderwijs gereed.

![](../media/tutorial_vrapplauncher_callbackcode.PNG)

Een andere trainingssessie voor het verwerken van onbekende en niet-eenduidige entiteiten begint.

1. Klik op nieuwe Train dialoogvenster.
1. Voer 'OneNote starten'. 
    - Deze herkend als een appnaam omdat het EntityDetectionCallback gedefinieerd in de code wordt de naam en de gebruiker heeft ingevoerd, wordt omgezet in een appnaam door vergelijken met de lijst met Apps die zijn gedefinieerd in de code. Vervolgens wordt de set met alle overeenkomende apps. 
    - Als de lijst met komt overeen met nul is, betekent dit dat de app is niet geïnstalleerd. Wordt deze in unknownAppName.
    - Als er meer dan één app hebt gevonden, wordt de kopiëren naar de DisambigAppNames en schakelt u de entiteit AppName.
2. Klik op de actie Score.
3. Schakel 'Helaas, ik weet niet de app $UknownAppName'.
4. Voer 'amazon starten'. We proberen het andere pad.
5. Klik op Score acties.
    - Opmerking Amazon-Video's en muziek in Amazon zijn nu DisambigAppNames geheugen. En OneNote is gewist.
3. Klik om te selecteren, er zijn enkele apps die geluid die...'
    - Houd er rekening mee dat de score is niet zeer hoge omdat er slechts enkele training dialoogvensters naar dit punt. Lijkt erop dat we moet een aantal meer hier het model meer bepalende.
2. Voer Score acties.
4. Klik op het onderwijs gereed.

U hebt nu gezien hoe u de omzetting van de entiteit. De demo ook API retouraanroepen geïllustreerd en een sjabloon voor het verzamelen van informatie en de juiste actie op basis van die controleren op aanwezigheid en dubbelzinnigheid bleek.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een conversatie cursist bot implementeren](../deploy-to-bf.md)
