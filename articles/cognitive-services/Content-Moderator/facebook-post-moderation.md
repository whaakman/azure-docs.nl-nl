---
title: Inhoud toezicht Facebook met Azure inhoud beheerder | Microsoft Docs
description: Gemiddeld Facebook-pagina's met machine learning op basis van inhoud beheerder
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/18/2017
ms.author: sajagtap
ms.openlocfilehash: 316c7212c30e9fe1151cdf5ceabf875439ad4c65
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344569"
---
# <a name="facebook-content-moderation-with-content-moderator"></a>Inhoud toezicht Facebook met de beheerder van de inhoud

In deze zelfstudie we informatie over het gebruik van machine learning-gebaseerde inhoud beheerder om te helpen gemiddeld Facebook-advertenties en -opmerkingen.

De zelfstudie leidt u door de volgende stappen uit:

1. Een beheerder inhoud team maken.
2. Azure-functies die naar HTTP-gebeurtenissen van inhoud beheerder en Facebook luisteren maken.
3. Een Facebook-Page en een App maken en deze verbinding maken met de beheerder van inhoud.

Nadat we klaar bent, wordt de inhoud die wordt gepost door de beheerder van de inhoud van de bezoekers verzenden in Facebook. Op basis van de drempelwaarden overeen, uw werkstromen inhoud beheerder publiceren de inhoud of beoordelingen vanuit het hulpprogramma revisie te maken. 

De volgende afbeelding ziet de bouwstenen van de oplossing.

![Beheer van Facebook-berichten](images/tutorial-facebook-moderation.png)

## <a name="create-a-content-moderator-team"></a>Een beheerder inhoud team maken

Raadpleeg de [Quick Start](quick-start.md) pagina aanmelden voor inhoud beheerder en het maken van een team.

## <a name="configure-image-moderation-workflow-threshold"></a>Afbeelding toezicht werkstroom (drempel) configureren

Raadpleeg de [werkstromen](review-tool-user-guide/workflows.md) kunt u een aangepaste installatiekopie-werkstroom (drempel) configureren. Houd er rekening mee de werkstroom **naam**.

## <a name="3-configure-text-moderation-workflow-threshold"></a>3. Tekst toezicht werkstroom (drempel) configureren

Ga als volgt naar de [werkstromen](review-tool-user-guide/workflows.md) pagina voor het configureren van een aangepaste tekst drempel en de werkstroom. Houd er rekening mee de werkstroom **naam**.

![Tekst-werkstroom configureren](images/text-workflow-configure.PNG)

De werkstroom testen met behulp van de knop 'Workflow uitvoeren'.

![Tekst werkstroom testen](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Azure Functions maken

Aanmelden bij de [Azure Management Portal](https://portal.azure.com/) om uw Azure-functies te maken. Volg deze stappen:

1. Maken van een Azure-functie-App, zoals wordt weergegeven op de [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) pagina.
2. Open de nieuwe functie-App.
3. Navigeer in de App naar **Platform-onderdelen -> Toepassingsinstellingen**
4. Definieer het volgende [toepassingsinstellingen](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings):

> [!NOTE]
> De **cm: regio** moet de naam van de regio (zonder spaties).
> Bijvoorbeeld: **westeurope**, niet West-Europa, **westcentralus**, niet West-Centraal VS, enzovoort.
>

| App-instelling | Beschrijving   | 
| -------------------- |-------------|
| cm:TeamId   | Uw team inhoud beheerder-id  | 
| cm:SubscriptionKey | Uw inhoud beheerder abonnementssleutel - Zie [referenties](/review-tool-user-guide/credentials.md) | 
| cm:Region | Uw inhoud beheerder regionaam, zonder de spaties. Zie de voorgaande opmerking. |
| cm:ImageWorkflow | Naam van de werkstroom uit te voeren op afbeeldingen |
| cm:TextWorkflow | Naam van de werkstroom uit te voeren op tekst |
| cm:CallbackEndpoint | URL voor de CMListener functie-App die u verderop in deze handleiding maakt |
| FB:VerificationToken | Het geheime token, ook gebruikt om u te abonneren op de Facebook-gebeurtenissen kanaal |
| FB:PageAccessToken | Het toegangstoken Facebook graph api niet verloopt en biedt de mogelijkheid de functie verbergen/Delete posts namens jou. |

5. Maak een nieuwe **HttpTrigger CSharp** functie met de naam **FBListener**. Deze functie ontvangt gebeurtenissen van Facebook. Deze functie hebt gemaakt met de volgende stappen:

    1. Houd de [maken van Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) pagina open ter referentie.
    2. Klik op de **+** toevoegen om nieuwe functie te maken.
    3. In plaats van de ingebouwde sjablonen, kies de **aan de slag op uw eigen aangepaste/functie** optie.
    4. Klik op de tegel waarin staat dat **HttpTrigger CSharp**.
    5. Voer de naam **FBListener**. De **autorisatieniveau** veld moet worden ingesteld op **functie**.
    6. Klik op **Create**.
    7. Vervang de inhoud van de **run.csx** met de inhoud van [ **FbListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FbListener/run.csx).

6. Maak een nieuwe **HttpTrigger CSharp** functie met de naam **CMListener**. Deze functie ontvangt gebeurtenissen van Facebook. Volg deze stappen om deze functie te maken.

    1. Houd de [maken van Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) pagina open ter referentie.
    2. Klik op de **+** toevoegen om nieuwe functie te maken.
    3. In plaats van de ingebouwde sjablonen, kies de **aan de slag op uw eigen aangepaste/functie** optie.
    4. Klik op de tegel waarin staat dat **HttpTrigger CSharp**
    5. Voer de naam **CMListener**. De **autorisatieniveau** veld moet worden ingesteld op **functie**.
    6. Klik op **Create**.
    7. Vervang de inhoud van de **run.csx** met de inhoud van [ **CMListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/CmListener/run.csx).

## <a name="configure-the-facebook-page-and-app"></a>De Facebook-pagina en de App configureren
1. Een Facebook-App maken.

    1. Navigeer naar de [Facebook developer-site](https://developers.facebook.com/)
    2. Klik op **mijn Apps**.
    3. Voeg een nieuwe App.
    4. Selecteer **Webhooks Get -> gestart**
    5. Selecteer **pagina-abonneren > naar dit onderwerp**
    6. Geef de **FBListener Url** als de URL van de Callback en de **Token verifiëren** u hebt geconfigureerd onder de **functie App-instellingen**
    7. Zodra u zich hebt geabonneerd, bladert u omlaag naar de feed en selecteer **abonneren**.

2. Maak een Facebook-pagina.

    1. Navigeer naar [Facebook](https://www.facebook.com/bookmarks/pages) en maak een **nieuwe Facebook-Page**.
    2. Toestaan dat de Facebook-App voor toegang tot deze pagina met de volgende stappen:
        1. Navigeer naar de [Graph API Explorer](https://developers.facebook.com/tools/explorer/).
        2. Selecteer **toepassing**.
        3. Selecteer **pagina Access Token**, verzendt een **ophalen** aanvraag.
        4. Klik op de **pagina-ID** in het antwoord.
        5. Nu toevoegen de **/subscribed_apps** verzenden naar de URL een **ophalen** aanvraag (leeg antwoord).
        6. Verzenden van een **Post** aanvraag. Ophalen van het antwoord als **geslaagd: true**.

3. Maak een niet-verlopende Graph API-toegangstoken.

    1. Navigeer naar de [Graph API Explorer](https://developers.facebook.com/tools/explorer/).
    2. Selecteer de **toepassing** optie.
    3. Selecteer de **krijgen toegang gebruikerstoken** optie.
    4. Onder de **Selecteer machtigingen**, selecteer **manage_pages** en **publish_pages** opties.
    5. We gebruiken de **toegangstoken** (korte gehouden Token) in de volgende stap.

4. We Postman gebruiken voor de volgende stappen.

    1. Open **Postman** (of deze [hier](https://www.getpostman.com/)).
    2. Importeren van deze twee bestanden:
        1. [Postman verzameling](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/Facebook%20Permanant%20Page%20Access%20Token.postman_collection.json)
        2. [Postman omgeving](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FB%20Page%20Access%20Token%20Environment.postman_environment.json)       
    3. Deze omgevingsvariabelen bijwerken:
    
    | Sleutel | Waarde   | 
    | -------------------- |-------------|
    | appId   | Uw Facebook-App Identifier hier invoegen  | 
    | appSecret | Voeg hier uw Facebook-App-geheim | 
    | short_lived_token | U hebt gegenereerd in de vorige stap token voor de korte levensduur gebruikerstoegang invoegen |
    4. Voer nu de 3 API's die worden vermeld in de verzameling: 
        1. Selecteer **Long-Lived Access Token genereren** en klik op **verzenden**.
        2. Selecteer **gebruikers-ID ophalen** en klik op **verzenden**.
        3. Selecteer **Token voor permanente pagina op het toegang krijgen** en klik op **verzenden**.
    5. Kopieer de **access_token** waarde uit het antwoord en wijs het toe aan de App-instelling **fb:PageAccessToken**.

Dat is alles.

De oplossing verzendt alle afbeeldingen en tekst gepost op uw Facebook-pagina-inhoud beheerder. De werkstromen die u eerder hebt geconfigureerd, worden geactiveerd. De inhoud die voldoet niet aan de gedefinieerde criteria in de resultaten van de werkstromen in revisies in het hulpprogramma voor beoordeling. De rest van de inhoud wordt gepubliceerd.

## <a name="license"></a>Licentie

Alle Microsoft cognitieve Services SDK's en voorbeelden worden in licentie gegeven aan de MIT-licentie. Zie voor meer informatie [licentie](https://microsoft.mit-license.org/).

## <a name="developer-code-of-conduct"></a>Gedragscode voor ontwikkelaars

Ontwikkelaars die cognitieve Services, waaronder deze clientbibliotheek & voorbeeld verwacht voert u de 'Developer Code van uitvoeren voor Microsoft cognitieve Services', gevonden op http://go.microsoft.com/fwlink/?LinkId=698895.

## <a name="next-steps"></a>Volgende stappen

1. [Bekijk een demo (video)](https://channel9.msdn.com/Events/Build/2017/T6033) van deze oplossing van Microsoft Build 2017.
1. [De Facebook-voorbeeld op Github](https://github.com/MicrosoftContentModerator/samples-fbPageModeration)
1. https://docs.microsoft.com/azure/azure-functions/functions-create-github-webhook-triggered-function
2. http://ukimiawz.github.io/facebook/2015/08/12/webhook-facebook-subscriptions/
3. http://stackoverflow.com/questions/17197970/facebook-permanent-page-access-token
