---
title: Het beheren van gebruikersgegevens met de conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het beheren van gegevens van de gebruiker met de conversatie cursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8d42f903559a1e07b42ded33972be4b552f21b5e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345272"
---
# <a name="managing-user-data"></a>Gebruikersgegevens beheren

Deze pagina wordt beschreven wat de conversatie cursist cloud-service registreert bij het uitvoeren van dialoogvensters met eindgebruikers.  Ook wordt beschreven hoe u logboeken van de conversatie cursist koppelen aan gebruikersnamen, zodat u kunt ophalen en verwijderen van alle logboeken die zijn gekoppeld aan een bepaalde gebruiker.

## <a name="overview-of-end-user-data-logging"></a>Overzicht van logboekregistratie van gegevens door eindgebruikers

Standaard registreert de conversatie cursist cloudservice interactie tussen gebruikers en uw bot.  Deze logboeken zijn belangrijk voor het verbeteren van uw bot, zodat u kunt het identificeren van gevallen waarin uw bot opgehaald van de entiteit onjuist of de onjuiste actie geselecteerd.  Deze fouten kunnen dan worden gecorrigeerd door te gaan naar de pagina 'Log-dialoogvensters' van de gebruikersinterface, correcties aanbrengen en deze gecorrigeerde dialoogvenster Opslaan als een nieuwe train-dialoogvenster. Voor meer informatie, Zie de zelfstudie op 'Logboek dialoogvensters'.

## <a name="how-to-disable-logging"></a>Logboekregistratie uitschakelen

U kunt bepalen of gesprekken met eindgebruikers op de pagina 'Instellingen' voor uw toepassing conversatie cursist zijn.  Er is een selectievakje voor "Logboek conversaties."  Als u dit selectievakje uitschakelt, zijn de gesprekken met eindgebruikers worden niet geregistreerd.

## <a name="what-is-logged"></a>Wat is geregistreerd 

In het logboek dialoogvensters slaat conversatie cursist de invoer van gebruiker, de waarden entiteit, de geselecteerde acties en de tijdstempels voor elke inschakelen.  Deze logboeken worden opgeslagen voor een bepaalde periode, en vervolgens verwijderd (Zie de help-pagina op 'standaardwaarde en grenzen' voor meer informatie).  

Conversatie cursist maakt een unieke ID voor elke aangemelde dialoogvenster.  Conversatie cursist biedt *niet* een gebruikers-id met aangemelde dialoogvensters opslaan.  

## <a name="associating-logged-dialogs-with-a-user-id"></a>Koppelen van dialoogvensters geregistreerd met een gebruikers-ID

Vaak is het belangrijk om te kunnen aangemelde dialoogvensters bijvoorbeeld met de ID van de gebruiker--koppelen, te kunnen ophalen of aangemelde dialoogvensters verwijderen uit een bepaalde gebruiker.  Omdat de conversatie cursist een gebruikers-id niet opslaat, moet deze koppeling worden beheerd door de code van de ontwikkelaar.  

Voor het maken van deze toewijzing verkrijgen van de ID van het logboek dialoogvenster in `EntityDetectionCallback`; slaat u vervolgens de koppeling tussen de gebruikers-ID en dit logboek dialoogvenster in uw bot-opslag.  

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

In elk van de onderstaande HTTP-aanroepen, de volgende koptekst kunt toevoegen:

```
Ocp-Apim-Subscription-Key=<LUIS_AUTHORING_KEY>
```

waar `<LUIS_AUTHORING_KEY>` is de sleutel die wordt gebruikt voor toegang tot de conversatie cursist toepassingen ontwerpen LUIS.

## <a name="how-to-obtain-raw-data-for-a-logged-dialog"></a>Het ophalen van onbewerkte gegevens voor een geregistreerde dialoogvenster

Als u de onbewerkte gegevens voor een dialoogvenster logboek, kunt u deze aanroep HTTP:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Waar `<appId>` is de GUID voor deze toepassing conversatie cursist en `<logDialgoId>` is de ID van het logboek dialoogvenster dat u wilt ophalen.  

Houd er rekening mee dat logboek dialoogvensters kunnen worden bewerkt door de ontwikkelaar, en vervolgens als trainen dialoogvensters opgeslagen.  Wanneer dit wordt gedaan, slaat conversatie cursist de ID van het dialoogvenster voor het logboek van 'bron' met het dialoogvenster train.  Bovendien wordt kunt een dialoogvenster train worden 'vertakte' in de gebruikersinterface; Als een dialoogvenster train een bijbehorende bron logboek dialoogvenster ID heeft, worden klikt u vervolgens vertakkingen van dialoogvenster train gemarkeerd met dezelfde logboek dialoogvenster ID.

Volg deze stappen voor alle train-dialoogvensters die zijn afgeleid van een dialoogvenster logboek.

Eerst alle train dialoogvensters ophalen:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialogs
```

Waar `<appId>` is de GUID voor deze conversatie cursist-toepassing.  

Hiermee wordt alle train-dialoogvensters.  Zoeken in deze lijst voor de bijbehorende `sourceLogDialogId`, en noteer de bijbehorende `trainDialogId`. 

Aan een enkele trainen dialoogvenster door-ID:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Waar `<appId>` is de GUID voor deze toepassing conversatie cursist en `<trainDialogId>` is de ID van de trein dialoogvenster dat u wilt ophalen.  

## <a name="how-to-delete-a-logged-dialog"></a>Het verwijderen van een geregistreerde dialoogvenster

Als u verwijderen van een logboek dialoogvenster gegeven van de bijbehorende ID wilt, kunt u deze HTTP-aanroep kunt gebruiken:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Waar `<appId>` is de GUID voor deze toepassing conversatie cursist en `<logDialogId>` is de ID van het logboek dialoogvenster dat u wilt verwijderen. 

Als u verwijderen van een train dialoogvenster gegeven van de bijbehorende ID wilt, kunt u deze HTTP-aanroep kunt gebruiken:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Waar `<appId>` is de GUID voor deze toepassing conversatie cursist en `<trainDialogId>` is de ID van de trein dialoogvenster dat u wilt verwijderen. 
