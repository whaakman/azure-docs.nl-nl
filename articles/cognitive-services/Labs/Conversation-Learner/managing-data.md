---
title: Gebruikers gegevens beheren met Conversation Learner-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het beheren van gebruikers gegevens met Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 857e899764d284e2d78f1172fa8eeac04c57d618
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705196"
---
# <a name="managing-user-data"></a>Gebruikers gegevens beheren

Op deze pagina wordt beschreven wat de Conversation Learner-Cloud service meldt bij het uitvoeren van dialoog vensters met eind gebruikers.  Ook wordt beschreven hoe u Conversation Learner-logboeken koppelt aan gebruikers-Id's, zodat u alle logboeken kunt ophalen of verwijderen die zijn gekoppeld aan een bepaalde gebruiker.

## <a name="overview-of-end-user-data-logging"></a>Overzicht van logboek registratie van gegevens van eind gebruikers

Standaard registreert de Conversation Learner Cloud service interacties tussen eind gebruikers en uw bot.  Deze logboeken zijn belang rijk voor het verbeteren van uw bot, zodat u de gevallen kunt identificeren waarin uw bot de onjuiste entiteit heeft geëxtraheerd of de verkeerde actie heeft geselecteerd.  Deze fouten kunnen vervolgens worden gecorrigeerd door naar de pagina dialoog vensters van de gebruikers interface te gaan, correcties aan te brengen en dit gecorrigeerd dialoog venster op te slaan als een nieuw trein venster. Zie voor meer informatie de zelf studie over het dialoog venster Logboeken.

## <a name="how-to-disable-logging"></a>Logboek registratie uitschakelen

U kunt bepalen of gesp rekken met eind gebruikers zich bevinden op de pagina instellingen voor uw Conversation Learner model.  Er is een selectie vakje voor conversaties voor logboek registratie.  Als u dit selectie vakje uitschakelt, worden conversaties met eind gebruikers niet geregistreerd.

## <a name="what-is-logged"></a>Wat is vastgelegd 

In het dialoog venster Logboeken Conversation Learner worden de gebruikers invoer, entiteits waarden, geselecteerde acties en tijds tempels voor elke beurt opgeslagen.  Deze logboeken worden gedurende een bepaalde tijd opgeslagen en vervolgens verwijderd (Zie de Help-pagina op ' standaard waarde en grenzen ' voor meer informatie).  

Conversation Learner maakt een unieke ID voor elk vastgelegd dialoog venster.  Conversation Learner slaat *geen* gebruikers-id op met geregistreerde dialoog vensters.  

## <a name="associating-logged-dialogs-with-a-user-id"></a>Geregistreerde dialoog vensters koppelen aan een gebruikers-ID

Het is vaak belang rijk dat u geregistreerde dialoog vensters kunt koppelen aan de ID van de gebruiker. zo kunt u bijvoorbeeld vastgelegde dialoog vensters van een bepaalde gebruiker ophalen of verwijderen.  Omdat Conversation Learner geen gebruikers-id opslaat, moet deze koppeling worden onderhouden door de code van de ontwikkelaar.  

Als u deze toewijzing wilt maken, moet u de id van het `EntityDetectionCallback`geregistreerde dialoog venster in opslaan. vervolgens slaat u de koppeling tussen de gebruikers-id en het geregistreerde dialoog venster op in de opslag ruimte van uw bot.  

```
cl.EntityDetectionCallback(async (text: string, memoryManager: ClientMemoryManager): Promise<void> => {

    // Get user and session info
    var sessionData = memoryManager.SessionInfo();

    // sessionData.sessionId is the ID of this logged dialog.
    // In your bot-specific data store, store an association
    // between your user identifier and this session ID.
    console.log(sessionData.logDialogId)

    // sessionData.userId and sessionData.userName are the 
    // user ID and user name as defined by the channel the user
    // is on (eg, Skype, Teams, Facebook Messenger, etc.).
    // For some channels, this will be undefined.
    // This information is NOT stored with the logged dialog.
    console.log(sessionData.userId);
    console.log(sessionData.userName);

})
```

## <a name="headers-for-http-calls"></a>Headers voor HTTP-aanroepen

Voeg in elk van de onderstaande HTTP-aanroepen de volgende header toe:

```
Ocp-Apim-Subscription-Key=<LUIS_AUTHORING_KEY>
```

waar `<LUIS_AUTHORING_KEY>` is de Luis-ontwerp sleutel die wordt gebruikt om toegang te krijgen tot uw Conversation learner-toepassingen.

## <a name="how-to-obtain-raw-data-for-a-logged-dialog"></a>Onbewerkte gegevens voor een vastgelegd dialoog venster ophalen

Als u de onbewerkte gegevens voor een logboek venster wilt ophalen, kunt u deze HTTP-oproep gebruiken:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Waar `<appId>` is de GUID voor dit Conversation learner model en `<logDialgoId>` is de id van het dialoog venster Logboek dat u wilt ophalen.  

> [!NOTE]
> Logboek dialoogvensters kunnen worden bewerkt door de ontwikkelaar en vervolgens worden opgeslagen als trein dialoogvensters.  Wanneer dit is gebeurd, slaat Conversation Learner de ID van het dialoog venster Bron logboek op met het dialoog venster trainen.  Daarnaast kan een trein dialoog venster worden ' vertakkingen ' in de gebruikers interface. Als er een dialoog venster voor een trein is gekoppeld aan de logboek-ID, worden vertakkingen uit het dialoog venster trainen met dezelfde logboek dialoogvenster-ID gemarkeerd.

Voer de volgende stappen uit om alle dialoog vensters van een logboek te verkrijgen die zijn afgeleid van een logbestand.

Haal eerst alle trein dialoogvensters op:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialogs
```

Waar `<appId>` de GUID voor dit Conversation learner model is.  

Hiermee worden alle dialoog vensters van de trein weer gegeven.  Zoek in deze lijst naar de `sourceLogDialogId`gekoppelde en noteer de bijbehorende `trainDialogId`. 

Naar één trein dialoog venster met ID:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Waar `<appId>` is de GUID voor dit Conversation learner model en `<trainDialogId>` is de id van het dialoog venster dat u wilt ophalen.  

## <a name="how-to-delete-a-logged-dialog"></a>Een vastgelegd dialoog venster verwijderen

Als u een dialoog venster met de ID van het logboek wilt verwijderen, kunt u deze HTTP-oproep gebruiken:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Waar `<appId>` is de GUID voor dit Conversation learner model en `<logDialogId>` is de id van het logboek dialoogvenster dat u wilt verwijderen. 

Als u een dialoog venster voor de trein wilt verwijderen, kunt u deze HTTP-oproep gebruiken:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Waar `<appId>` is de GUID voor dit Conversation learner model en `<trainDialogId>` is de id van het dialoog venster dat u wilt verwijderen. 
