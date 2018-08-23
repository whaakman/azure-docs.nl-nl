---
title: Facebook-inhoudstoezicht met Azure Content Moderator | Microsoft Docs
description: Gemiddeld Facebook-pagina's met machine learning op basis van de Content Moderator
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/18/2017
ms.author: sajagtap
ms.openlocfilehash: 4570b514bef33c8dfa6d220ee4cd88ad068ba4d0
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "41994383"
---
# <a name="facebook-content-moderation-with-content-moderator"></a>Facebook-inhoudstoezicht met Content Moderator

In deze zelfstudie, we informatie over het gebruik van machine learning-op basis van Content Moderator voor matige Facebook-berichten en opmerkingen.

De zelfstudie leidt u door de volgende stappen uit:

1. Maak een Content Moderator-team.
2. Maak Azure Functions die naar http-gebeurtenissen van de Content Moderator en Facebook luistert.
3. Een Facebook-Page en een App maken en verbinden met Content Moderator.

Als we klaar bent, wordt de inhoud geplaatst door de bezoekers van Content Moderator verzenden en Facebook. Op basis van drempelwaarden voor de overeenkomst, uw werkstromen Content Moderator publiceren de inhoud of beoordelingen binnen het beoordelingsprogramma maken. 

De volgende afbeelding ziet u de bouwstenen van de oplossing.

![Beheer van Facebook-berichten](images/tutorial-facebook-moderation.png)

## <a name="create-a-content-moderator-team"></a>Een Content Moderator-team maken

Raadpleeg de [snelstartgids](quick-start.md) pagina om te registreren voor Content Moderator en een team maken.

## <a name="configure-image-moderation-workflow-threshold"></a>Configureren van de installatiekopie van beheer van workflow (threshold)

Raadpleeg de [werkstromen](review-tool-user-guide/workflows.md) pagina voor het configureren van een aangepaste installatiekopie-werkstroom (threshold). Houd er rekening mee de werkstroom **naam**.

## <a name="3-configure-text-moderation-workflow-threshold"></a>3. Werkstroom van de toezicht op tekst (threshold) configureren

Ga als volgt aan de [werkstromen](review-tool-user-guide/workflows.md) pagina voor het configureren van een aangepaste tekst drempelwaarde en werkstroom. Houd er rekening mee de werkstroom **naam**.

![Tekst-werkstroom configureren](images/text-workflow-configure.PNG)

Test uw werkstroom met behulp van de knop 'Workflow uitvoeren'.

![Tekst werkstroom testen](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Azure Functions maken

Aanmelden bij de [Azure Management Portal](https://portal.azure.com/) om uw Azure-functies te maken. Volg deze stappen:

1. Een Azure-functie-App maken zoals wordt weergegeven op de [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) pagina.
2. Open de zojuist gemaakte functie-App.
3. In de App navigeren naar **Platform-onderdelen -> Toepassingsinstellingen**
4. Definieer het volgende [toepassingsinstellingen](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings):

> [!NOTE]
> De **cm: regio** moet de naam van de regio (zonder spaties).
> Bijvoorbeeld, **westeurope**, niet West-Europa, **westcentralus**, niet West-Centraal VS, enzovoort.
>

| App-instelling | Beschrijving   | 
| -------------------- |-------------|
| cm:TeamId   | Uw TeamId Content Moderator  | 
| cm:SubscriptionKey | Uw abonnementssleutel Content Moderator - Zie [referenties](/review-tool-user-guide/credentials.md) | 
| cm:Region | Uw Content Moderator regionaam, zonder de spaties. Zie de vorige opmerking. |
| cm:ImageWorkflow | Naam van de werkstroom om uit te voeren in afbeeldingen |
| cm:TextWorkflow | Naam van de werkstroom om uit te voeren op tekst |
| cm:CallbackEndpoint | URL voor de CMListener functie-App die u verderop in deze handleiding maakt |
| FB:VerificationToken | Het token voor geheim, ook gebruikt om u te abonneren op de Facebook-gebeurtenissen feed |
| FB:PageAccessToken | De Facebook graph api-toegangstoken verloopt niet en kan de functie berichten verbergen/verwijderen uit uw naam. |

5. Maak een nieuwe **HttpTrigger-CSharp** functie met de naam **FBListener**. Deze functie ontvangt gebeurtenissen van Facebook. Deze functie maken door de volgende stappen:

    1. Houd de [Azure Functions maken](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) pagina open ter referentie.
    2. Klik op de **+** toevoegen om nieuwe functie te maken.
    3. Kies in plaats van de ingebouwde sjablonen, de **aan de slag met uw eigen/aangepaste functie** optie.
    4. Klik op de tegel met de melding dat **HttpTrigger-CSharp**.
    5. Voer de naam **FBListener**. De **autorisatieniveau** veld moet worden ingesteld op **functie**.
    6. Klik op **Create**.
    7. Vervang de inhoud van de **run.csx** met de inhoud van [ **FbListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FbListener/run.csx).

6. Maak een nieuwe **HttpTrigger-CSharp** functie met de naam **CMListener**. Deze functie ontvangt gebeurtenissen van Content Moderator. Volg deze stappen om deze functie te maken.

    1. Houd de [Azure Functions maken](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) pagina open ter referentie.
    2. Klik op de **+** toevoegen om nieuwe functie te maken.
    3. Kies in plaats van de ingebouwde sjablonen, de **aan de slag met uw eigen/aangepaste functie** optie.
    4. Klik op de tegel met de melding dat **HttpTrigger-CSharp**
    5. Voer de naam **CMListener**. De **autorisatieniveau** veld moet worden ingesteld op **functie**.
    6. Klik op **Create**.
    7. Vervang de inhoud van de **run.csx** met de inhoud van [ **CMListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/CmListener/run.csx).

## <a name="configure-the-facebook-page-and-app"></a>De Facebook-pagina en de App configureren
1. Maak een Facebook-App.

    1. Navigeer naar de [Facebook developer-site](https://developers.facebook.com/)
    2. Klik op **mijn Apps**.
    3. Een nieuwe App toevoegen.
    4. Selecteer **Webhooks Get -> aan de slag**
    5. Selecteer **pagina abonneren -> in dit onderwerp**
    6. Geef de **FBListener Url** als de Callback-URL en de **Token verifiëren** u hebt geconfigureerd onder de **functie App-instellingen**
    7. Zodra u zich hebt geabonneerd, bladert u omlaag naar de feed en selecteer **abonneren**.

2. Maak een Facebook-pagina.

    1. Navigeer naar [Facebook](https://www.facebook.com/bookmarks/pages) en maak een **nieuwe Facebook-Page**.
    2. Toestaan dat de Facebook-App voor toegang tot deze pagina door de volgende stappen:
        1. Navigeer naar de [Graph-API-Verkenner](https://developers.facebook.com/tools/explorer/).
        2. Selecteer **toepassing**.
        3. Selecteer **pagina toegangstoken**, verzendt een **ophalen** aanvraag.
        4. Klik op de **pagina-ID** in het antwoord.
        5. Nu toevoegen de **/subscribed_apps** toe aan de URL en verzend een **ophalen** (leeg antwoord)-aanvraag.
        6. Dien een **Post** aanvraag. U krijgt het antwoord als **geslaagd: true**.

3. Een niet-verlopen Graph API-toegangstoken hebt gemaakt.

    1. Navigeer naar de [Graph-API-Verkenner](https://developers.facebook.com/tools/explorer/).
    2. Selecteer de **toepassing** optie.
    3. Selecteer de **gebruiker Access Token ophalen** optie.
    4. Onder de **Selecteer machtigingen**, selecteer **manage_pages** en **publish_pages** opties.
    5. We gebruiken de **toegangstoken** (korte heb gewoond Token) in de volgende stap.

4. We gebruiken Postman om de volgende stappen.

    1. Open **Postman** (of deze [hier](https://www.getpostman.com/)).
    2. Importeer deze twee bestanden:
        1. [Postman-verzameling](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/Facebook%20Permanant%20Page%20Access%20Token.postman_collection.json)
        2. [Postman-omgeving](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FB%20Page%20Access%20Token%20Environment.postman_environment.json)       
    3. Deze omgevingsvariabelen bijwerken:
    
    | Sleutel | Waarde   | 
    | -------------------- |-------------|
    | toepassings-id   | Uw Facebook-App Identifier hier invoegen  | 
    | appSecret | Voeg hier uw Facebook-App-geheim | 
    | short_lived_token | Het toegangstoken van de korte levensduur hebben gebruiker die u in de vorige stap invoegen |
    4. Voer nu de 3 API's die worden vermeld in de verzameling: 
        1. Selecteer **Long-Lived Access Token genereren** en klikt u op **verzenden**.
        2. Selecteer **gebruiker-ID ophalen** en klikt u op **verzenden**.
        3. Selecteer **ophalen permanente pagina toegangstoken** en klikt u op **verzenden**.
    5. Kopieer de **access_token** waarde uit het antwoord en wijs deze toe aan de App-instelling **fb:PageAccessToken**.

Dat is alles.

De oplossing verzendt alle afbeeldingen en tekst op uw Facebook-pagina voor Content Moderator geplaatst. De werkstromen die u eerder hebt geconfigureerd, worden aangeroepen. De inhoud die niet met uw criteria die zijn gedefinieerd in de resultaten van de werkstromen in beoordelingen binnen het beoordelingsprogramma. De rest van de inhoud wordt gepubliceerd.

## <a name="license"></a>Licentie

Alle Microsoft Cognitive Services-SDK's en voorbeelden hebben een licentie voor de MIT-licentie. Zie voor meer informatie, [licentie](https://microsoft.mit-license.org/).

## <a name="developer-code-of-conduct"></a>Gedragscode voor ontwikkelaars

Ontwikkelaars die gebruikmaken van Cognitive Services, inclusief deze client-bibliotheek en voorbeeld gevonden op om te volgen van de 'ontwikkelaars Code van uitvoeren voor Microsoft Cognitive Services' worden verwacht http://go.microsoft.com/fwlink/?LinkId=698895.

## <a name="next-steps"></a>Volgende stappen

1. [Bekijk een demo (video)](https://channel9.msdn.com/Events/Build/2017/T6033) van deze oplossing van Microsoft Build 2017.
1. [De Facebook-voorbeeld op Github](https://github.com/MicrosoftContentModerator/samples-fbPageModeration)
1. https://docs.microsoft.com/azure/azure-functions/functions-create-github-webhook-triggered-function
2. http://ukimiawz.github.io/facebook/2015/08/12/webhook-facebook-subscriptions/
3. http://stackoverflow.com/questions/17197970/facebook-permanent-page-access-token
