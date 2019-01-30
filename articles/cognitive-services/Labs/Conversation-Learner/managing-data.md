---
title: Beheren van gebruikersgegevens met Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het beheren van gegevens van de gebruiker met Conversatiecursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 1d3687585a663cf937374e76dafe834c641ad3a2
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55215279"
---
# <a name="managing-user-data"></a>Gebruikersgegevens beheren

Deze pagina wordt beschreven wat de cloudservice Conversatiecursist zich aanmeldt bij het uitvoeren van dialoogvensters met eindgebruikers.  Het wordt ook beschreven hoe u Conversatiecursist logboeken koppelen aan gebruikers-id's, zodat u kunt ophalen of verwijderen van alle logboeken die zijn gekoppeld aan een bepaalde gebruiker.

## <a name="overview-of-end-user-data-logging"></a>Overzicht van logboekregistratie van gegevens van eindgebruikers

De cloudservice Conversatiecursist registreert standaard, interactie tussen gebruikers en uw bot.  Deze logboeken zijn belangrijk voor het verbeteren van uw bot, zodat u kunt voor het identificeren van gevallen waarin uw bot opgehaald van de entiteit onjuist of de onjuiste actie geselecteerd.  Deze fouten kunnen vervolgens worden gecorrigeerd door te gaan naar de pagina 'Log-dialoogvensters' van de gebruikersinterface, correcties maken en dit gecorrigeerde dialoogvenster Opslaan als een nieuw dialoogvenster van de trein. Zie voor meer informatie de zelfstudie op "Log-dialoogvensters."

## <a name="how-to-disable-logging"></a>Logboekregistratie uitschakelen

U kunt bepalen of gesprekken met eindgebruikers op de pagina 'Instellingen' voor het model van uw Conversatiecursist zijn.  Er is een selectievakje voor "Log gesprekken."  Als u dit selectievakje uitschakelt, worden gesprekken met eindgebruikers kunnen niet geregistreerd.

## <a name="what-is-logged"></a>Wat is aangemeld 

In logboek dialoogvensters slaat Conversatiecursist de invoer van de gebruiker, entiteitswaarden, geselecteerde acties en een tijdstempel voor elke inschakelen.  Deze logboeken worden opgeslagen voor een bepaalde periode en vervolgens verwijderd (Zie de help-pagina op 'standaardwaarde en grenzen' voor meer informatie).  

Conversatiecursist Hiermee maakt u een unieke ID voor elke geregistreerde dialoogvenster.  Conversatiecursist heeft *niet* een gebruikers-id met geregistreerde dialoogvensters opslaan.  

## <a name="associating-logged-dialogs-with-a-user-id"></a>Dialoogvensters voor koppelen worden geregistreerd met een gebruikers-ID

Vaak is het belangrijk dat u aangemeld dialoogvensters bijvoorbeeld met de ID van de gebruiker--koppelen, te kunnen ophalen of aangemelde dialoogvensters verwijderen uit een bepaalde gebruiker.  Omdat Conversatiecursist niet met een gebruikers-id opgeslagen worden, moet deze koppeling worden beheerd door de code van de ontwikkelaar.  

Voor het maken van deze toewijzing, verkrijgen de ID van het dialoogvenster geregistreerd in `EntityDetectionCallback`; slaat u vervolgens in de opslag van uw bot, de koppeling tussen de gebruikers-ID en dit dialoogvenster geregistreerd.  

```
cl.EntityDetectionCallback(async (text: string, memoryManager: ClientMemoryManager): Promise<void> => {

    // Get user and session info
    var sessionData = memoryManager.SessionInfo();

    // sessionData.sessionId is the ID of this logged dialog.
    // In your bot-specific datastore, store an association
    // bewteen your user identifier and this session ID.
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

In elk van de volgende HTTP-aanroepen, toevoegen de volgende header:

```
Ocp-Apim-Subscription-Key=<LUIS_AUTHORING_KEY>
```

waar `<LUIS_AUTHORING_KEY>` is de sleutel die wordt gebruikt voor toegang tot de conversatie Learner toepassingen ontwerpen LUIS.

## <a name="how-to-obtain-raw-data-for-a-logged-dialog"></a>Het ophalen van onbewerkte gegevens voor een geregistreerde dialoogvenster

Als u de onbewerkte gegevens voor een dialoogvenster logboek wilt, kunt u deze HTTP-aanroep:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Waar `<appId>` is de GUID voor dit model Conversatiecursist en `<logDialgoId>` is de ID van het logboek-dialoogvenster dat u wilt ophalen.  

> [!NOTE]
> Logboek dialoogvensters kunnen worden bewerkt door de ontwikkelaar en vervolgens opgeslagen als dialoogvensters trainen.  Wanneer dit wordt gedaan, slaat Conversatiecursist de ID van het dialoogvenster 'bron' logboek met het dialoogvenster van de trein.  Bovendien wordt kan een dialoogvenster van de trein worden "vertakt' in de gebruikersinterface; Als een dialoogvenster van de trein een bijbehorende bron log dialoog-ID heeft, worden klikt u vervolgens vertakkingen van de trein dialoogvenster gemarkeerd met dezelfde log dialoogvenster-ID.

Volg deze stappen voor alle dialoogvensters van de trein die zijn afgeleid van een dialoogvenster logboek.

Eerst alle dialoogvensters van de trein ophalen:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialogs
```

Waar `<appId>` is de GUID voor dit model Conversatiecursist.  

Hiermee wordt alle dialoogvensters van de trein geretourneerd.  Zoeken in deze lijst voor de bijbehorende `sourceLogDialogId`, en noteer de bijbehorende `trainDialogId`. 

Naar een enkele trainen dialoogvenster door-ID:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Waar `<appId>` is de GUID voor dit model Conversatiecursist en `<trainDialogId>` is de ID van de trein dialoogvenster dat u wilt ophalen.  

## <a name="how-to-delete-a-logged-dialog"></a>Het verwijderen van een geregistreerde dialoogvenster

Als u verwijderen van een logboek dialoogvenster gegeven van de ID wilt, kunt u deze HTTP-aanroep:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Waar `<appId>` is de GUID voor dit model Conversatiecursist en `<logDialogId>` is de ID van het logboek-dialoogvenster dat u wilt verwijderen. 

Als u verwijderen van een train dialoogvenster gegeven van de ID wilt, kunt u deze HTTP-aanroep:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Waar `<appId>` is de GUID voor dit model Conversatiecursist en `<trainDialogId>` is de ID van de trein dialoogvenster dat u wilt verwijderen. 
