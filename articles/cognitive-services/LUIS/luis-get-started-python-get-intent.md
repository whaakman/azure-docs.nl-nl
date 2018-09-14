---
title: 'Snelstart: een LUIS-app (Language Understanding) aanroepen met behulp van Python | Microsoft Docs'
description: In deze snelstart leert u hoe u een LUIS-app aanroept met behulp van Python.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 06/27/2018
ms.author: diberry
ms.openlocfilehash: bc7ae912d762a98c34b9a1b2d6a82d5630c4794b
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "43770003"
---
# <a name="quickstart-call-a-luis-endpoint-using-python"></a>Snelstart: Een LUIS-eindpunt aanroepen met behulp van Python
In deze snelstart leest u hoe u utterances doorgeeft aan een LUIS-eindpunt en intenties en entiteiten terugkrijgt.

<!-- green checkmark -->
<!--
> [!div class="checklist"]
> * Create LUIS subscription and copy key value for later use
> * View LUIS endpoint results from browser to public sample IoT app
> * Create Visual Studio C# console app to make HTTPS call to LUIS endpoint
-->

Voor dit artikel hebt u een gratis [LUIS](luis-reference-regions.md#luis-website)-account nodig om uw LUIS-toepassing te creëren.

<a name="create-luis-subscription-key"></a>
## <a name="create-luis-endpoint-key"></a>LUIS-eindpuntsleutel maken
U hebt een Cognitive Services API-sleutel nodig om aanroepen te doen naar de LUIS-voorbeeld-app die wordt gebruikt in dit scenario. 

Voer de volgende stappen uit om een API-sleutel op te halen: 

1. U moet eerst een [Cognitive Services API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) in Azure Portal maken. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

2. Meld u aan bij Azure Portal op https://portal.azure.com. 

3. Volg de stappen in [Eindpuntsleutels maken met behulp van Azure](./luis-how-to-azure-subscription.md) om een sleutel op te halen.

4. Ga terug naar de [LUIS](luis-reference-regions.md)-website en meld u aan met uw Azure-account. 

    [![](media/luis-get-started-node-get-intent/app-list.png "Schermopname van de lijst met apps")](media/luis-get-started-node-get-intent/app-list.png)

## <a name="understand-what-luis-returns"></a>Inzicht in wat LUIS retourneert

Om inzicht te krijgen in wat een LUIS-app retourneert, kunt u de URL van een LUIS-voorbeeld-app in een browservenster plakken. De voorbeeld-app is een IoT-app die detecteert of de gebruiker lampen wil inschakelen of uitschakelen.

1. Het eindpunt van de voorbeeld-app heeft deze indeling: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_API_KEY>&verbose=false&q=turn%20on%20the%20bedroom%20light` Kopieer de URL en vervang de waarde van het veld `subscription-key` door uw eindpuntsleutel.
2. Plak de URL in een browservenster en druk op Enter. In de browser wordt een JSON-resultaat weergegeven dat aangeeft dat LUIS de intent `HomeAutomation.TurnOn` en de entiteit `HomeAutomation.Room` met de waarde `bedroom` detecteert.

    ![JSON-resultaat detecteert de intent TurnOn](./media/luis-get-started-node-get-intent/turn-on-bedroom.png)
3. Wijzig de waarde van de parameter `q=` in de URL in `turn off the living room light` en druk op Enter. Het resultaat geeft nu aan dat LUIS de intent `HomeAutomation.TurnOff` en de entiteit `HomeAutomation.Room` met waarde `living room` heeft gedetecteerd. 

    ![JSON-resultaat detecteert de intent TurnOff](./media/luis-get-started-node-get-intent/turn-off-living-room.png)


## <a name="consume-a-luis-result-using-the-endpoint-api-with-python"></a>Een LUIS-resultaat gebruiken met behulp van de eindpunt-API met Python

U kunt Python gebruiken voor toegang tot de resultaten die u in het browservenster in de vorige stap hebt gezien.

1. Kopieer een van de volgende codefragmenten naar een bestand met de naam `quickstart-call-endpoint.py`:

   [!code-python[Console app code that calls a LUIS endpoint for Python 2.7](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/2.x/quickstart-call-endpoint-2-7.py)]

   [!code-python[Console app code that calls a LUIS endpoint for Python 3.6](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/3.x/quickstart-call-endpoint-3-6.py)]

2. Vervang de waarde van het veld `Ocp-Apim-Subscription-Key` door de sleutel van uw LUIS-eindpunt.

3. Installeer afhankelijkheden met `pip install requests`.

4. Voer het script uit met behulp van `python ./quickstart-call-endpoint.py`. De uitvoer bestaat uit de JSON die u eerder hebt gezien in het browservenster.
<!-- 
![Console window displays JSON result from LUIS](./media/luis-get-started-python-get-intent/console-turn-on.png)
-->

## <a name="clean-up-resources"></a>Resources opschonen
De twee resources die in deze zelfstudie zijn gemaakt, zijn de LUIS-eindpuntsleutel en het C#-project. Verwijder de LUIS-eindpuntsleutel uit Azure Portal. Sluit het Visual Studio-project en verwijder de map uit het bestandssysteem. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Utterances toevoegen](luis-get-started-python-add-utterance.md)