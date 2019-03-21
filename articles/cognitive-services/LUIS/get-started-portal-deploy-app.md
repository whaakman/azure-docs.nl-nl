---
title: 'Quickstart: Met LUIS Portal-app implementeren'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Zodra de app is gereed om terug te keren utterance voorspellingen naar een clienttoepassing, bijvoorbeeld een chatbot, moet u de app implementeren op het eindpunt van de voorspelling. In deze snelstartgids leert u een toepassing implementeren met het maken van een voorspelling endpoint-resource, de resource toe te wijzen aan de app, de app trainen en publiceren van de app.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: 0cf7464188bb502d77c673284cba226ce91a5725
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58259717"
---
# <a name="quickstart-deploy-an-app-in-the-luis-portal"></a>Quickstart: Een app implementeren in de portal LUIS

Zodra de app is gereed om terug te keren utterance voorspellingen naar een clienttoepassing, bijvoorbeeld een chatbot, moet u de app implementeren op het eindpunt van de voorspelling. 

In deze snelstartgids leert u een toepassing implementeren met het maken van een voorspelling endpoint-resource, de resource toe te wijzen aan de app, de app trainen en publiceren van de app. 

## <a name="prerequisites"></a>Vereisten

* [Azure-abonnement](https://azure.microsoft.com/free).
* Voltooi de [vorige portal snelstartgids](get-started-portal-build-app.md) of [downloaden en importeren van de app](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/in-portal/build-portal-app.json). 


## <a name="create-endpoint-resource"></a>Endpoint-resource maken

De voorspelling endpoint-resource wordt gemaakt in Azure portal. Deze resource moet alleen worden gebruikt voor eindpunt voorspelling query's. Gebruik deze resource niet voor schrijven wijzigingen in de app. 

1. Aanmelden bij de  **[Azure-portal](https://ms.portal.azure.com/)**. 

1. Selecteer het groene **+** zich in het bovenste linkerdeelvenster en zoek naar de `Cognitive Services` op marketplace, selecteer dit. 

1. Het abonnement met de volgende instellingen configureren:

    |Instelling|Value|Doel|
    |--|--|--|
    |Name|`my-cognitive-service-resource`|De naam van de Azure-resource. U moet deze naam wanneer u de resource aan de app in de LUIS-portal toewijzen.|
    |Abonnement|Uw abonnement|Selecteer een van de abonnementen die zijn gekoppeld aan uw account.|
    |Locatie|**US - west**|De azure-regio voor deze resource.|
    |Prijscategorie|**S0**|De standaardwaarde prijscategorie voor deze resource.|
    |Resourcegroep|`my-cognitive-service-resource-group`|Maak een nieuwe resourcegroep voor alle serviceresources in uw cognitive. Wanneer u klaar bent met de resources, kunt u de resourcegroep voor het opschonen van uw abonnement verwijderen. | 

    ![Azure API kiezen](./media/get-started-portal-deploy-app/create-cognitive-services-resource.png) 

1. Selecteer **maken** te maken van de Azure-resource. 

    In de volgende sectie leert u hoe u deze nieuwe resources verbinden met een LUIS-app in de LUIS-portal. 

## <a name="assign-the-resource-key-to-the-luis-app-in-the-luis-portal"></a>De bronsleutel toewijzen aan de LUIS-app in de portal LUIS

Telkens wanneer u een nieuwe resource van LUIS maakt, moet u de resource toewijzen aan de LUIS-app. Zodra deze is toegewezen, hoeft u deze stap opnieuw uitvoert, tenzij u een nieuwe resource maken. U kunt een nieuwe resource om uit te breiden, de regio's van uw app of voor de ondersteuning van een hoger aantal voorspelling query's maken. 

1. Aanmelden bij de [LUIS portal](https://www.luis.ai), kiest u de **myEnglishApp** -app uit de lijst met apps.

1. Selecteer **beheren** in het menu rechtsboven en selecteer **sleutels en eindpunten**.

1. Als u wilt toevoegen de LUIS, selecteert u **Resource toewijzen +**.

    [![Een resource toewijzen aan uw app](./media/get-started-portal-deploy-app/assign-resource-button.png)](./media/get-started-portal-deploy-app/assign-resource-button.png#lightbox)

1. Selecteer de naam van uw tenant, abonnement en resourcegroep en selecteer vervolgens **resource toewijzen**. 

    ![Een resource toewijzen aan uw app](./media/get-started-portal-deploy-app/assign-resource.png)

1. De nieuwe rij in de tabel zoeken en kopieer de eindpunt-URL. Het is goed samengesteld om te maken van een HTTP GET-aanvraag naar de LUIS-API-eindpunt voor een voorspelling. 

## <a name="train-and-publish-the-app"></a>De app trainen en publiceren 

U moet aanleren wanneer u klaar bent om te testen. Wanneer u de momenteel getrainde versie beschikbaar zijn voor clienttoepassingen uit de voorspelling eindpunt runtime wilt, moet u de app publiceren. 

1. Als de app ongetrainde is, selecteert u **Train** rechts van het bovenste menu.

1. Selecteer **publiceren** rechts van het bovenste menu. Accepteer de standaardinstellingen van de omgeving en selecteer **publiceren**.

1. Wanneer de meldingsbalk groen geslaagd wordt weergegeven aan de bovenkant van het browservenster wordt weergegeven, selecteert u de **verwijzen naar de lijst met eindpunten** koppeling. 

    ![De meldingsbalk app is gepubliceerd in browser](./media/get-started-portal-deploy-app/successfully-published-notification.png)

1. Hiermee gaat u naar de **sleutels en het eindpunt instellingen** pagina. De lijst met toegewezen resources en het bijbehorende eindpunt URL's is aan de onderkant van de pagina. 

1. Selecteer de eindpunt-URL die is gekoppeld aan de namen van uw nieuwe resourcegroep. Hiermee opent u een webbrowser met een correct opgemaakte URL waarmee een **ophalen** aanvraag voor de voorspelling endpoint-runtime. 

1. De `q=` aan het einde van de URL is te kort voor **query** en waar utterance van de gebruiker wordt toegevoegd aan de GET-aanvraag. Na de `q=`, voer de dezelfde gebruiker utterance aan het einde van de vorige snelstartgids gebruikt:

    ```Is there a form named hrf-234098```

    Het antwoord van de browser wordt dezelfde JSON wordt de clienttoepassing ontvangt:

    ```JSON
    {
    "query": "Is there a form named hrf-234098",
    "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9768753
    },
    "intents": [
        {
        "intent": "FindForm",
        "score": 0.9768753
        },
        {
        "intent": "None",
        "score": 0.0216071066
        }
    ],
    "entities": [
        {
        "entity": "hrf-234098",
        "type": "Human Resources Form Number",
        "startIndex": 22,
        "endIndex": 31
        }
      ]
    }
    ```

    Dit antwoord krijgt u meer informatie dan de standaardwaarde **Test** deelvenster in de vorige zelfstudie. Als u deze hetzelfde niveau van de gegevens in het deelvenster zien wilt, kan de app moet worden gepubliceerd. Selecteer in het testvenster **vergelijken met gepubliceerd**. Gebruik **weergeven JSON-weergave** in het deelvenster gepubliceerd om te zien van de dezelfde JSON als de vorige stap. Hiermee kunt u de huidige app die u bezig bent en de app die is gepubliceerd naar het eindpunt met elkaar vergelijken.

    [![Vergelijk ten opzichte van de gepubliceerde versie van de app wordt bewerkt](./media/get-started-portal-deploy-app/compare-test-pane.png)](./media/get-started-portal-deploy-app/compare-test-pane.png#lightbox)


## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u klaar bent met deze Quick Start, selecteert u **mijn apps** van het bovenste navigatiemenu. Klik links selectievakje van de app in de lijst en selecteer **verwijderen** via het context-werkbalk boven de lijst. 

[![App verwijderen uit de lijst met mijn apps](./media/get-started-portal-build-app/delete-app.png)](./media/get-started-portal-build-app/delete-app.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Algemene intenties en entiteiten identificeert](luis-tutorial-prebuilt-intents-entities.md)