---
title: 'Quickstart: Een app implementeert met de portal LUIS'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Leer hoe u uw LUIS-app implementeren naar het eindpunt voorspelling nadat de app is gereed om terug te keren utterance voorspellingen naar een clienttoepassing, bijvoorbeeld een chatbot. In deze snelstartgids leert u hoe u een toepassing implementeren met het maken van een voorspelling endpoint-resource, de resource toe te wijzen aan de app, de app trainen en publiceren van de app.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: db2b543fa8e5429cc8d50d7789b03239173f563d
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65154538"
---
# <a name="quickstart-deploy-an-app-in-the-luis-portal"></a>Quickstart: Een app implementeren in de portal LUIS

Wanneer uw LUIS-app is gereed om terug te keren utterance voorspellingen naar een clienttoepassing (bijvoorbeeld een chatbot), moet u de app implementeren op het eindpunt van de voorspelling.

In deze snelstartgids leert u een toepassing te implementeren. U maakt een voorspelling endpoint-resource, de resource toewijzen aan de app de app trainen en publiceer de app.

## <a name="prerequisites"></a>Vereisten

* Krijgen een [Azure-abonnement](https://azure.microsoft.com/free).
* Voltooi de [vorige portal snelstartgids](get-started-portal-build-app.md) of [downloaden en importeren van de app](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/in-portal/build-portal-app.json).

## <a name="create-the-endpoint-resource"></a>De endpoint-resource maken

U kunt de voorspelling endpoint-resource maken in Azure portal. Deze resource moet alleen worden gebruikt voor eindpunt voorspelling query's. Gebruik deze resource niet voor schrijven wijzigingen in de app.

1. Meld u aan bij [Azure Portal](https://ms.portal.azure.com/).

1. Selecteer het groene **+** zich in het deelvenster van de linkerbovenhoek. Zoeken naar `Cognitive Services` in de marketplace en selecteer deze.

1. Het abonnement met de volgende instellingen configureren:

   |Instelling|Value|Doel|
   |--|--|--|
   |Name|`my-cognitive-service-resource`|De naam van de Azure-resource. U moet deze naam wanneer u de resource aan de app in de LUIS-portal toewijzen.|
   |Abonnement|Uw abonnement|Selecteer een van de abonnementen die zijn gekoppeld aan uw account.|
   |Locatie|**US - west**|De Azure-regio voor deze resource.|
   |Prijscategorie|**S0**|De standaardwaarde prijscategorie voor deze resource.|
   |Resourcegroep|`my-cognitive-service-resource-group`|Maak een nieuwe resourcegroep voor alle serviceresources in uw cognitive. Wanneer u klaar bent met de resources, kunt u de resourcegroep voor het opschonen van uw abonnement verwijderen. |
   | | | |

   ![Azure API kiezen](./media/get-started-portal-deploy-app/create-cognitive-services-resource.png)

1. Selecteer **maken** te maken van de Azure-resource.

   In de volgende sectie leert u hoe u deze nieuwe resources verbinden met een LUIS-app in de LUIS-portal.

## <a name="assign-the-resource-key-to-the-luis-app-in-the-luis-portal"></a>De bronsleutel toewijzen aan de LUIS-app in de portal LUIS

Telkens wanneer u een nieuwe resource van LUIS maakt, moet u de resource toewijzen aan de LUIS-app. Nadat deze toegewezen, hoeft u deze stap opnieuw uitvoert, tenzij u een nieuwe resource maken. U kunt een nieuwe resource om uit te breiden, de regio's van uw app of voor de ondersteuning van een hoger aantal voorspelling query's maken.

1. Aanmelden bij de [LUIS portal](https://www.luis.ai) en kies de **myEnglishApp** -app uit de lijst met apps.

1. Selecteer **beheren** in het menu rechtsboven en selecteer vervolgens **sleutels en eindpunten**.

1. Als u wilt toevoegen de LUIS, selecteer **Resource toewijzen +**.

   [![Een resource toewijzen aan uw app](./media/get-started-portal-deploy-app/assign-resource-button.png)](./media/get-started-portal-deploy-app/assign-resource-button.png#lightbox)

1. Selecteer de naam van uw tenant, abonnement en resourcegroep. Selecteer **resource toewijzen**.

   ![Een resource toewijzen aan uw app](./media/get-started-portal-deploy-app/assign-resource.png)

1. De nieuwe rij in de tabel zoeken en kopieer de eindpunt-URL. Deze correct wordt samengesteld om een `HTTP GET` aanvraag naar de LUIS-API-eindpunt voor een voorspelling.

## <a name="train-and-publish-the-app"></a>De app trainen en publiceren

De app trainen wanneer u bent klaar om dit te testen. Publiceer de app als u wilt dat de momenteel getrainde versie beschikbaar zijn voor clienttoepassingen van de query voorspelling endpoint-runtime.

1. Als de app ongetrainde is, selecteert u **Train** in het menu in de rechterbovenhoek.

1. Selecteer **publiceren** in het menu bovenaan. Accepteer de standaardinstellingen van de omgeving en selecteer **publiceren**.

1. Wanneer de meldingsbalk groen geslaagd wordt weergegeven aan de bovenkant van het browservenster wordt weergegeven, selecteert u **verwijzen naar de lijst met eindpunten**.

   ![De meldingsbalk app is gepubliceerd in browser](./media/get-started-portal-deploy-app/successfully-published-notification.png)

1. Op de **sleutels en het eindpunt instellingen** pagina, de lijst met toegewezen resources en bijbehorende eindpunt-URL's onder vinden.

1. Selecteer de eindpunt-URL die is gekoppeld aan de namen van uw nieuwe resourcegroep. Deze actie wordt een webbrowser geopend met een correct opgemaakte URL waarmee een `GET` aanvraag voor de voorspelling endpoint-runtime.

1. De `q=` aan het einde van de URL is te kort voor **query** en waar utterance van de gebruiker wordt toegevoegd aan de GET-aanvraag. Na de `q=`, voer de dezelfde gebruiker utterance aan het einde van de vorige snelstartgids gebruikt:

    ```Is there a form named hrf-234098```

    De browser ziet u het antwoord, dit is dezelfde JSON wordt de clienttoepassing ontvangt:

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

    Dit antwoord krijgt u meer informatie dan het testvenster standaard in de vorige zelfstudie. Als u wilt deze hetzelfde niveau van de gegevens in het deelvenster wordt weergegeven, moet u de app publiceren. Nadat de app wordt gepubliceerd, selecteert u **vergelijken met gepubliceerd** in het deelvenster. Gebruik **weergeven JSON-weergave** in het deelvenster gepubliceerde test om te zien van de dezelfde JSON als de vorige stap. Op deze manier kunt u de huidige app waaraan u werkt met een app die is gepubliceerd naar het eindpunt vergelijken.

    [![Vergelijk ten opzichte van de gepubliceerde versie van de app wordt bewerkt](./media/get-started-portal-deploy-app/compare-test-pane.png)](./media/get-started-portal-deploy-app/compare-test-pane.png#lightbox)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met deze Quick Start, selecteert u **mijn apps** van het bovenste navigatiemenu. Selecteer het selectievakje van de app in de lijst en selecteer vervolgens **verwijderen** via het context-werkbalk boven de lijst.

[![App verwijderen uit de lijst met mijn apps](./media/get-started-portal-build-app/delete-app.png)](./media/get-started-portal-build-app/delete-app.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Algemene intenties en entiteiten identificeert](luis-tutorial-prebuilt-intents-entities.md)
