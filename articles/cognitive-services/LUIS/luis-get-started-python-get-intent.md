---
title: 'Zelfstudie: een LUIS-app (Language Understanding) aanroepen met behulp van Python | Microsoft Docs'
description: In deze zelfstudie leert u hoe u een LUIS-app aanroept met behulp van Python.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 8671e81f6d8c18c17f34843d2c1b8460306daeb5
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264581"
---
# <a name="tutorial-call-a-luis-endpoint-using-python"></a>Zelfstudie: Een LUIS-eindpunt aanroepen met behulp van Python
Lees hoe u utterances doorgeeft aan een LUIS-eindpunt en intenties en entiteiten terugkrijgt.

<!-- green checkmark -->
> [!div class="checklist"]
> * Een LUIS-abonnement maken en de sleutelwaarde kopiëren voor later gebruik
> * LUIS eindpuntresultaten bekijken vanuit browser naar openbare IoT-voorbeeld-app
> * Visual Studio C#-console-app maken om HTTPS-aanroepen te versturen naar LUIS eindpunt

Voor dit artikel hebt u een gratis [LUIS][LUIS]-account nodig om de LUIS-toepassing te maken.

## <a name="create-luis-subscription-key"></a>LUIS-abonnementssleutel maken
U hebt een Cognitive Services API-sleutel nodig om aanroepen te doen naar de LUIS-voorbeeld-app die wordt gebruikt in dit scenario. 

Voer de volgende stappen uit om een API-sleutel op te halen: 

1. U moet eerst een [Cognitive Services API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) in Azure Portal maken. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

2. Meld u aan bij Azure Portal op https://portal.azure.com. 

3. Volg de stappen in [Abonnementssleutels maken met behulp van Azure](./luis-how-to-azure-subscription.md) om een sleutel op te halen.

4. Ga terug naar de [LUIS](luis-reference-regions.md)-website en meld u aan met uw Azure-account. 

    [![](media/luis-get-started-node-get-intent/app-list.png "Schermopname van de lijst met apps")](media/luis-get-started-node-get-intent/app-list.png)

## <a name="understand-what-luis-returns"></a>Inzicht in wat LUIS retourneert

Om inzicht te krijgen in wat een LUIS-app retourneert, kunt u de URL van een LUIS-voorbeeld-app in een browservenster plakken. De voorbeeld-app is een IoT-app die detecteert of de gebruiker lampen wil inschakelen of uitschakelen.

1. Het eindpunt van de voorbeeld-app heeft deze indeling: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_API_KEY>&verbose=false&q=turn%20on%20the%20bedroom%20light` Kopieer de URL en vervang de waarde van het veld `subscription-key` door uw abonnementssleutel.
2. Plak de URL in een browservenster en druk op Enter. In de browser wordt een JSON-resultaat weergegeven dat aangeeft dat LUIS de intent `HomeAutomation.TurnOn` en de entiteit `HomeAutomation.Room` met de waarde `bedroom` detecteert.

    ![JSON-resultaat detecteert de intent TurnOn](./media/luis-get-started-node-get-intent/turn-on-bedroom.png)
3. Wijzig de waarde van de parameter `q=` in de URL in `turn off the living room light` en druk op Enter. Het resultaat geeft nu aan dat LUIS de intent `HomeAutomation.TurnOff` en de entiteit `HomeAutomation.Room` met waarde `living room` heeft gedetecteerd. 

    ![JSON-resultaat detecteert de intent TurnOff](./media/luis-get-started-node-get-intent/turn-off-living-room.png)


## <a name="consume-a-luis-result-using-the-endpoint-api-with-python"></a>Een LUIS-resultaat gebruiken met behulp van de eindpunt-API met Python

U kunt Python gebruiken voor toegang tot de resultaten die u in het browservenster in de vorige stap hebt gezien.

1. Kopieer een van de volgende codefragmenten:

   [!code-python[Console app code that calls a LUIS endpoint for Python 2.7](~/samples-luis/documentation-samples/endpoint-api-samples/python/quickstart-call-endpoint-2-7.py)]

   [!code-python[Console app code that calls a LUIS endpoint for Python 3.6](~/samples-luis/documentation-samples/endpoint-api-samples/python/quickstart-call-endpoint-3-6.py)]


2. Vervang de waarde van het veld `Ocp-Apim-Subscription-Key`-door de sleutel van uw LUIS-abonnement.

3. Voer het script uit. De uitvoer bestaat uit de JSON die u eerder hebt gezien in het browservenster.
<!-- 
![Console window displays JSON result from LUIS](./media/luis-get-started-python-get-intent/console-turn-on.png)
-->

## <a name="clean-up-resources"></a>Resources opschonen
De twee resources die in deze zelfstudie zijn gemaakt, zijn de LUIS-abonnementssleutel en het JavaScript-project. Verwijder de LUIS-abonnementssleutel uit Azure Portal. Sluit het Visual Studio-project en verwijder de map uit het bestandssysteem. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Utterances toevoegen](luis-get-started-python-add-utterance.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website