---
title: 'Zelfstudie: Facebook-inhoud controleren - Azure Content Moderator'
titlesuffix: Azure Cognitive Services
description: In deze zelfstudie leert u hoe u met behulp van machine learning en Content Moderator berichten en opmerkingen voor Facebook kunt controleren.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: tutorial
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 085ddaf757f2a11b4db4aadb22ad16009d29231e
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54260433"
---
# <a name="tutorial-facebook-content-moderation-with-content-moderator"></a>Zelfstudie: Facebook-inhoud controleren met Azure Content Moderator

In deze zelfstudie leert u hoe u met behulp van machine learning en Content Moderator berichten en opmerkingen voor Facebook kunt controleren.

In deze zelfstudie gaat u de volgende stappen uitvoeren:

1. Een Content Moderator-team samenstellen.
2. Azure-functies maken die luisteren naar HTTP-gebeurtenissen van Content Moderator en Facebook.
3. Een Facebook-pagina en -app maken en deze verbinden met Content Moderator.

Als dit klaar is, wordt inhoud die door bezoekers op Facebook wordt geplaatst eerst naar Content Moderator verstuurd. Op basis van drempelwaarden voor overeenkomst zorgen de werkstromen van Content Moderator ervoor dat de inhoud wordt gepubliceerd of dat er beoordelingen worden gemaakt binnen het beoordelingsprogramma. 

In de volgende afbeelding ziet u de bouwstenen van de oplossing.

![Beheer van Facebook-berichten](images/tutorial-facebook-moderation.png)

## <a name="create-a-content-moderator-team"></a>Een Content Moderator-team samenstellen

Raadpleeg de quickstart [Content Moderator op internet proberen](quick-start.md) voor informatie over hoe u zich aanmeldt voor Content Moderator en een team samenstelt.

## <a name="configure-image-moderation-workflow-threshold"></a>Werkstroom voor controle van afbeeldingen configureren (drempelwaarde)

Raadpleeg de pagina [Werkstromen](review-tool-user-guide/workflows.md) voor informatie over het configureren van een aangepaste werkstroom voor afbeeldingen (drempelwaarde). Noteer de **naam** van de werkstroom.

## <a name="3-configure-text-moderation-workflow-threshold"></a>3. Werkstroom voor controle van tekst configureren (drempel)

Gebruik stappen vergelijkbaar zoals op de pagina [Werkstromen](review-tool-user-guide/workflows.md) voor het configureren van een aangepaste drempelwaarde en werkstroom voor het controleren van tekst. Noteer de **naam** van de werkstroom.

![Tekstwerkstroom configureren](images/text-workflow-configure.PNG)

Test uw werkstroom met behulp van de knop Execute Workflow.

![Tekstwerkstroom testen](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Azure-functies maken

Meld u aan bij de [Azure-beheerportal](https://portal.azure.com/) om Azure-functies te maken. Volg deze stappen:

1. Maak een Azure-functie-app zoals wordt weergegeven op de pagina [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal).
2. Open de zojuist gemaakte functie-app.
3. Navigeer in de app naar **Platformfuncties -> Toepassingsinstellingen**.
4. Definieer het volgende [toepassingsinstellingen](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings):

> [!NOTE]
> De waarde voor **cm: Region** moet bestaan uit de naam van de regio (zonder spaties).
> Dus bijvoorbeeld **westeurope**, niet Europa - west, **westcentralus**, niet US - west-centraal, enzovoort.
>

| App-instelling | Beschrijving   | 
| -------------------- |-------------|
| cm:TeamId   | De id van het Content Moderator-team  | 
| cm:SubscriptionKey | Uw abonnementssleutel voor Content Moderator: zie [Referenties](review-tool-user-guide/credentials.md) | 
| cm:Region | De naam van uw Content Moderator-regio, zonder de spaties. Zie de vorige opmerking. |
| cm:ImageWorkflow | De naam van de werkstroom om uit te voeren voor afbeeldingen |
| cm:TextWorkflow | De naam van de werkstroom om uit te voeren voor tekst |
| cm:CallbackEndpoint | URL voor de CMListener-functie-app die u verderop in deze handleiding maakt |
| fb:VerificationToken | Het geheime token, wordt ook gebruikt om u te abonneren op de feed met Facebook-gebeurtenissen |
| fb:PageAccessToken | Het toegangstoken voor de Facebook Graph-API verloopt niet en maakt het mogelijk de functie voor het verbergen/verwijderen van berichten namens u uit te voeren. |

5. Maak een nieuwe **HttpTrigger-CSharp**-functie met de naam **FBListener**. Deze functie ontvangt gebeurtenissen van Facebook. Maak deze functie met de volgende stappen:

    1. Houd de pagina [Een functie-app maken vanuit de Azure-portal](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) open ter referentie.
    2. Klik op het plusteken **+** om een nieuwe functie te maken.
    3. Kies in plaats van een ingebouwde sjabloon de optie **Ga aan de slag met uw eigen/aangepaste functie**.
    4. Klik op de tegel met de naam **HttpTrigger-CSharp**.
    5. Voer de naam **FBListener** in. Stel **Autorisatieniveau** in op **Functie**.
    6. Klik op **Create**.
    7. Vervang de inhoud van **run.csx** door de inhoud van [**FbListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FbListener/run.csx).

6. Maak een nieuwe **HttpTrigger-CSharp**-functie met de naam **CMListener**. Deze functie ontvangt gebeurtenissen van Content Moderator. Volg deze stappen voor het maken van deze functie.

    1. Houd de pagina [Een functie-app maken vanuit de Azure-portal](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) open ter referentie.
    2. Klik op het plusteken **+** om een nieuwe functie te maken.
    3. Kies in plaats van een ingebouwde sjabloon de optie **Ga aan de slag met uw eigen/aangepaste functie**.
    4. Klik op de tegel met de naam **HttpTrigger-CSharp**.
    5. Voer de naam **CMListener** in. Stel **Autorisatieniveau** in op **Functie**.
    6. Klik op **Create**.
    7. Vervang de inhoud van **run.csx** door de inhoud van [**CMListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/CmListener/run.csx).

## <a name="configure-the-facebook-page-and-app"></a>De Facebook-pagina en -app configureren
1. Maak een Facebook-app.

    1. Navigeer naar de [site voor Facebook-ontwikkelaars](https://developers.facebook.com/)
    2. Klik op **My Apps**.
    3. Voeg een nieuw app toe.
    4. Selecteer **Webhooks -> Get Started**
    5. Selecteer **Page -> Subscribe to this topic**
    6. Geef **FBListener Url** op als de Callback URL en het **Verify Token** dat u hebt geconfigureerd onder **Function App Settings**
    7. Als u zich hebt geabonneerd, bladert u omlaag naar de feed en selecteert u **subscribe**.

2. Maak een Facebook-pagina.

    1. Navigeer naar [Facebook](https://www.facebook.com/bookmarks/pages) en maak een **nieuwe Facebook-pagina**.
    2. Volg deze stappen om toe te staan dat de Facebook-app toegang heeft tot deze pagina:
        1. Navigeer naar de [Graph API Explorer](https://developers.facebook.com/tools/explorer/).
        2. Selecteer **Application**.
        3. Selecteer **Page Access Token**, verstuur een **Get**-aanvraag.
        4. Klik op de **Page ID** in het antwoord.
        5. Voeg de **/subscribed_apps** toe aan de URL en verstuur een **Get**-aanvraag (leeg antwoord).
        6. Verstuur een **Post**-aanvraag. U krijgt het antwoord als **success: true**.

3. Maak een Graph API-toegangstoken dat niet verloopt.

    1. Navigeer naar de [Graph API Explorer](https://developers.facebook.com/tools/explorer/).
    2. Selecteer de optie **Application**.
    3. Selecteer de optie **Get User Access Token**.
    4. Selecteer onder **Select Permissions** de opties **manage_pages** en **publish_pages**.
    5. We gebruiken het **toegangstoken** (token met korte levensduur) in de volgende stap.

4. We gebruiken Postman voor de volgende stappen.

    1. Open **Postman** (of downloadt het programma [hier](https://www.getpostman.com/)).
    2. Importeer deze twee bestanden:
        1. [Postman Collection](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/Facebook%20Permanant%20Page%20Access%20Token.postman_collection.json)
        2. [Postman Environment](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FB%20Page%20Access%20Token%20Environment.postman_environment.json)       
    3. Werk deze omgevingsvariabelen bij:
    
    | Sleutel | Waarde   | 
    | -------------------- |-------------|
    | appId   | Voeg hier de id van uw Facebook-app in  | 
    | appSecret | Voeg hier het geheim van uw Facebook-app in | 
    | token_met_korte_levensduur | Voeg hier het toegangstoken met korte levensduur van de gebruiker in dat u in de vorige stap hebt gegenereerd |
    4. Voer nu de drie API's uit die in de verzameling worden vermeld: 
        1. Selecteer **Generate Long-Lived Access Token** en klik op **Send**.
        2. Selecteer **Get User ID** en klik op **Send**.
        3. Selecteer **Get Permanent Page Access Token** en klik op **Send**.
    5. Kopieer de waarde van **access_token** in het antwoord en wijs deze toe aan de app-instelling, **fb:PageAccessToken**.

Dat is alles.

De oplossing verzendt alle afbeeldingen en tekst die op uw Facebook-pagina worden geplaatst naar Content Moderator. De werkstromen die u eerder hebt geconfigureerd, worden aangeroepen. De inhoud die niet voldoet aan de criteria die zijn gedefinieerd in de werkstromen, worden ter beoordeling aangeboden binnen het beoordelingsprogramma. De rest van de inhoud wordt gepubliceerd.

## <a name="license"></a>Licentie

Alle SDK's en voorbeelden van Microsoft Cognitive Services worden gelicentieerd met de MIT-licentie. Zie de [LICENTIE](https://microsoft.mit-license.org/) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

1. [Bekijk een demo (video)](https://channel9.msdn.com/Events/Build/2017/T6033) van deze oplossing van Microsoft build 2017.
1. [Het Facebook-voorbeeld op GitHub](https://github.com/MicrosoftContentModerator/samples-fbPageModeration)
1. https://docs.microsoft.com/azure/azure-functions/functions-create-github-webhook-triggered-function
2. http://ukimiawz.github.io/facebook/2015/08/12/webhook-facebook-subscriptions/
3. http://stackoverflow.com/questions/17197970/facebook-permanent-page-access-token
