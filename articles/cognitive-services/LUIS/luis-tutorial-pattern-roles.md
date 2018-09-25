---
title: 'Zelfstudie 4: Patroon rollen voor de context van de bijbehorende gegevens'
titleSuffix: Azure Cognitive Services
description: Een patroon gebruiken om gegevens te extraheren uit een juist opgemaakte sjabloon utterance. De sjabloon utterance maakt gebruik van een enkele entiteit en rollen om gerelateerde gegevens zoals de oorspronkelijke locatie- en doellocatie te extraheren.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.technology: language-understanding
ms.topic: article
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 07fc753f7f60b51fc6b27e43ffb8c181c570bb5a
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032509"
---
# <a name="tutorial-4-extract-contextually-related-patterns"></a>Zelfstudie: 4. Contextgerelateerde patronen herkennen

In deze zelfstudie moet u een patroon gebruiken om gegevens te extraheren uit een juist opgemaakte sjabloon utterance. De sjabloon utterance maakt gebruik van een enkele entiteit en rollen om gerelateerde gegevens zoals de oorspronkelijke locatie- en doellocatie te extraheren.  Wanneer u patronen gebruikt, zijn minder voorbeeld uitingen nodig voor het doel.

Het doel van de rollen is om op te halen van context-gerelateerde entiteiten in een utterance. In de utterance `Move new employee Robert Williams from Sacramento and San Francisco`, wordt de oorspronkelijke plaats en plaats van bestemming waarden aan elkaar zijn gerelateerd en algemene taal gebruiken om aan te duiden elke locatie. 


De naam van de nieuwe werknemer, Billy Patterson, maakt geen deel uit van de entiteit lijst **werknemer** nog. De naam van de nieuwe werknemer wordt eerst opgehaald om de naam van de verzenden naar een extern systeem te maken van de bedrijfsreferenties. Nadat de bedrijfsreferenties zijn gemaakt, de referenties van de werknemer zijn toegevoegd aan de lijst met entiteit **werknemer**.

De nieuwe werknemer en familie moeten worden verplaatst van de huidige plaats naar een plaats waar het fictieve bedrijf gevestigd is. Omdat een nieuwe medewerker afkomstig van een stad zijn kan, moeten de locaties worden gedetecteerd. Een lijst met instellen, zoals een lijst met entiteit werkt niet omdat alleen de plaatsen in de lijst met zou worden geëxtraheerd.

De rolnamen die is gekoppeld aan de bron en doel steden moeten uniek zijn in alle entiteiten. Er is een eenvoudige manier om te controleren of dat de rollen die uniek zijn om ze te koppelen aan de containerentiteit via een naamgeving. De **NewEmployeeRelocation** entiteit is een eenvoudige entiteit met twee rollen: **NewEmployeeReloOrigin** en **NewEmployeeReloDestination**. Relo is kort voor gegevensverplaatsing.

Omdat de voorbeeld-utterance `Move new employee Robert Williams from Sacramento and San Francisco` heeft alleen machine geleerde entiteiten, het is belangrijk om voldoende uitingen voorbeeld met de intent bieden, zodat de entiteiten worden gedetecteerd.  

**Terwijl patronen u minder voorbeeld uitingen kunnen, als de entiteiten niet worden gedetecteerd, wordt het patroon komt niet overeen.**

Als u problemen met detectie van eenvoudige entiteit hebt omdat deze een naam, bijvoorbeeld een stad, kunt u overwegen een woordgroepenlijst met vergelijkbare waarden. Hiermee wordt de detectie van de naam van de stad door middel van LUIS een extra signaal over dit type woord of woordgroep. Het patroon te woordgroep lijsten alleen helpen met detectie van de entiteit, die nodig voor het patroon is voor de overeenkomst. 

**In deze zelfstudie leert u hoe u:**

> [!div class="checklist"]
> * Gebruik bestaande zelfstudie-app
> * Nieuwe entiteiten maken
> * Nieuwe doel maken
> * Trainen
> * Publiceren
> * Intenties en entiteiten ophalen van eindpunt
> * Patroon met rollen maken
> * Woordgroepenlijst van steden maken
> * Intenties en entiteiten ophalen van eindpunt

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Gebruik bestaande app
Ga door met de app hebt gemaakt in de laatste zelfstudie, met de naam **Personeelszaken**. 

Als u de app Personeelszaken uit de vorige zelfstudie hebt, gebruikt u de volgende stappen uit:

1.  Download en sla [app JSON-bestand](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-patterns-HumanResources-v2.json).

2. De JSON importeren in een nieuwe app.

3. Uit de **beheren** sectie, op de **versies** tabblad en noem het klonen van de versie `roles`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. Omdat de versienaam van de wordt gebruikt als onderdeel van de URL-route, kan niet de naam van de tekens die niet toegestaan in een URL zijn bevatten.

## <a name="create-new-entities"></a>Nieuwe entiteiten maken

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecteer **entiteiten** in de linkernavigatiebalk. 

3. Selecteer **Create new intent**.

4. Voer in het pop-upvenster `NewEmployee` als een **eenvoudige** entiteit.

5. Selecteer **Create new intent**.

6. Voer in het pop-upvenster `NewEmployeeRelocation` als een **eenvoudige** entiteit.

7. Selecteer **NewEmployeeRelocation** uit de lijst met entiteiten. 

8. Voer de eerste rol als `NewEmployeeReloOrigin` en voert u selecteert.

9. Voer de tweede rol als `NewEmployeeReloDestination` en voert u selecteert.

## <a name="create-new-intent"></a>Nieuwe doel maken
Labels van de entiteiten in deze stappen is mogelijk beter als de vooraf gedefinieerde keyPhrase-entiteit wordt verwijderd voordat u begint vervolgens terug nadat u klaar bent met de stappen in deze sectie worden toegevoegd. 

1. Selecteer **Intents** in de linkernavigatiebalk.

2. Selecteer **Create new intent**. 

3. Voer `NewEmployeeRelocationProcess` als de naam van de intentie in het pop-updialoogvenster.

4. Voer het volgende voorbeeld-uitingen labels van de nieuwe entiteiten. De entiteits- en functie van waarden zijn vet weergegeven. Houd er rekening mee om te schakelen naar de **Tokens weergave** als u het label van de tekst gemakkelijker vinden. 

    U kunt de rol van de entiteit niet opgeven als labels in het doel. Doet u dat later bij het maken van het patroon. 

    |Utterance|NewEmployee|NewEmployeeRelocation|
    |--|--|--|
    |Verplaats **Bob Jones** van **Seattle** naar **Los Colinas**|Bob Jones|Seattle, Los Colinas|
    |Verplaats **Dave C. Cooper** van **Redmond** naar **New York City**|Dave C. Cooper|Redmond, New York City|
    |Verplaats **Jim Paul Smith** van **Toronto** naar **West Vancouver**|Jim Paul Smith|Amsterdam, West-Vancouver|
    |Verplaats **J. Benson** van **Boston** naar **Staines bij Thames**|"J". Benson|Boston, Staines bij Thames|
    |Verplaats **Travis "Trav" Hinton** van **Castelo Branco** naar **Orlando**|Travis "Trav" Hinton|Castelo Branco, Orlando|
    |Verplaats **Trevor Nottington III** van **Aranda de Duero** naar **Boise**|Trevor Nottington III|Aranda de Duero, Boise|
    |Verplaats **herstel na noodgevallen. Greg Williams** van **Orlando** naar **Ellicott stad**|Dr. Greg Williams|Orlando, Ellicott stad|
    |Verplaats **Robert "Berend" Gregson** van **Kansas City** naar **San Juan Capistrano**|Robert "Berend" Gregson|Kansas City, San Juan Capistrano|
    |Verplaats **Patti Owens** van **Bellevue** naar **Rockford**|Patti Owens|Bellevue, Rockford|
    |Verplaats **Janet Bartlet** van **Tuscan** naar **Rome**|Janet Bartlet|Tuscan, Rome|

    Naam van de werknemer heeft een aantal voorvoegsel, aantal woorden, syntaxis en achtervoegsel. Dit is belangrijk voor LUIS om te begrijpen van de variaties van de naam van een nieuwe werknemer. De namen van steden hebben ook verschillende woordentelling en syntaxis. Deze verschillende is het belangrijk om te leren van LUIS hoe deze entiteiten in utterance van een gebruiker kunnen worden weergegeven. 
    
    Als een van de entiteiten van de dezelfde woordentelling en geen andere variaties waren, zou u LUIS leren dat deze entiteit alleen die woordentelling en geen andere wijzigingen heeft aan te geven. LUIS zou niet mogelijk om goed te voorspellen een bredere set van variaties omdat dit een niet werd weergegeven. 

    Als u de entiteit keyPhrase verwijderd, opnieuw toevoegen aan de app nu.

## <a name="train"></a>Trainen

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publiceren

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Doel en entiteiten ophalen van eindpunt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Ga naar het einde van de URL in het adres en voer `Move Wayne Berry from Miami to Mount Vernon` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. 

    ```JSON
    {
      "query": "Move Wayne Berry from Newark to Columbus",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.514479756
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.514479756
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.017118983
        },
        {
          "intent": "MoveEmployee",
          "score": 0.009982505
        },
        {
          "intent": "GetJobInformation",
          "score": 0.008637771
        },
        {
          "intent": "ApplyForJob",
          "score": 0.007115978
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.006120186
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00452428637
        },
        {
          "intent": "None",
          "score": 0.00400899537
        },
        {
          "intent": "OrgChart-Reports",
          "score": 0.00240071164
        },
        {
          "intent": "Utilities.Help",
          "score": 0.001770991
        },
        {
          "intent": "EmployeeFeedback",
          "score": 0.001697356
        },
        {
          "intent": "OrgChart-Manager",
          "score": 0.00168116146
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00163952739
        },
        {
          "intent": "FindForm",
          "score": 0.00112958835
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.629158735
        },
        {
          "entity": "newark",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 27,
          "score": 0.638941
        }
      ]
    }  
    ```

De score intentie voorspelling is alleen ongeveer 50%. Als uw clienttoepassing een hogere waarde vereist, wordt dit moet worden hersteld. De entiteiten zijn niet een voorspeld.

Een van de locaties is uitgepakt, maar is niet de andere locatie. 

Patronen helpt de voorspelling score, maar de entiteiten die moeten worden correct voorspeld voordat het patroon overeenkomt met de utterance. 

## <a name="pattern-with-roles"></a>Patroon met rollen

1. Selecteer **bouwen** in de bovenste navigatiebalk.

2. Selecteer **patronen** in het linkernavigatievenster.

3. Selecteer **NewEmployeeRelocationProcess** uit de **een doel selecteren** vervolgkeuzelijst. 

4. Voer in het volgende patroon: `move {NewEmployee} from {NewEmployeeRelocation:NewEmployeeReloOrigin} to {NewEmployeeRelocation:NewEmployeeReloDestination}[.]`

    Als u trainen, publiceren en het eindpunt van de query, kunt u mogelijk doen om te zien dat de entiteiten niet zijn gevonden, zodat het patroon is niet overeenkomen, dus de voorspelling niet verbeteren. Dit is een gevolg van onvoldoende uitingen voorbeeld met gelabelde entiteiten. In plaats van het toevoegen van meer voorbeelden, een woordgroepenlijst u lost dit probleem wilt toevoegen.

## <a name="cities-phrase-list"></a>Lijst met steden woordgroep
Steden, zoals de namen van personen zijn moeilijk in dat ze elke combinatie van woorden en leestekens worden kunnen. Het plaatsen van de regio en de hele wereld bekend zijn, dus LUIS moet een woordgroepenlijst plaatsen om te beginnen met leren. 

1. Selecteer **woordgroepenlijst** uit de **verbeterde app-prestaties** gedeelte van het menu links. 

2. Naam van de lijst met `Cities` en voeg de volgende `values` voor de lijst:

    |Waarden van de woordgroepenlijst met|
    |--|
    |Seattle|
    |San Diego|
    |New York City|
    |Los Angeles|
    |Portland|
    |Philadephia|
    |Miami|
    |Dallas|

    Voeg geen elke plaats in de hele wereld of zelfs elke plaats in de regio. LUIS moet kunnen om te generaliseren wat een plaats in de lijst is. Zorg ervoor dat u houden **deze waarden zijn verwisselbaar** geselecteerde. Deze instelling betekent de woorden in de lijst op behandeld als synoniemen. 

3. Trainen en publiceren van de app.

## <a name="get-intent-and-entities-from-endpoint"></a>Doel en entiteiten ophalen van eindpunt

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Ga naar het einde van de URL in het adres en voer `Move wayne berry from miami to mount vernon` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. 

    ```JSON
    {
      "query": "Move Wayne Berry from Miami to Mount Vernon",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.9999999
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.9999999
        },
        {
          "intent": "Utilities.Confirm",
          "score": 1.49678385E-06
        },
        {
          "intent": "MoveEmployee",
          "score": 8.240291E-07
        },
        {
          "intent": "GetJobInformation",
          "score": 6.3131273E-07
        },
        {
          "intent": "None",
          "score": 4.25E-09
        },
        {
          "intent": "OrgChart-Manager",
          "score": 2.8E-09
        },
        {
          "intent": "OrgChart-Reports",
          "score": 2.8E-09
        },
        {
          "intent": "EmployeeFeedback",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.StartOver",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.Help",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Stop",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Cancel",
          "score": 1.35E-09
        },
        {
          "intent": "FindForm",
          "score": 1.23846156E-09
        },
        {
          "intent": "ApplyForJob",
          "score": 5.692308E-10
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "builtin.keyPhrase",
          "startIndex": 5,
          "endIndex": 15
        },
        {
          "entity": "miami",
          "type": "builtin.keyPhrase",
          "startIndex": 22,
          "endIndex": 26
        },
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.9410646,
          "role": ""
        },
        {
          "entity": "miami",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 26,
          "score": 0.9853915,
          "role": "NewEmployeeReloOrigin"
        },
        {
          "entity": "mount vernon",
          "type": "NewEmployeeRelocation",
          "startIndex": 31,
          "endIndex": 42,
          "score": 0.986044347,
          "role": "NewEmployeeReloDestination"
        }
      ],
      "sentimentAnalysis": {
        "label": "neutral",
        "score": 0.5
      }
}
    ```

De intentie score is nu veel hoger en de rolnamen van de maken deel uit van het antwoord van de entiteit.

## <a name="hierarchical-entities-versus-roles"></a>Hiërarchische entiteiten ten opzichte van rollen

In de [hiërarchische zelfstudie](luis-quickstart-intent-and-hier-entity.md), wordt de **MoveEmployee** bedoeling gedetecteerd wanneer een bestaande werknemer van een gebouw en office verplaatsen naar een andere. De voorbeeld-uitingen bron en doel-locaties heeft maar rollen niet hebt gebruikt. In plaats daarvan zijn de bron en doel onderliggende items van de hiërarchische entiteit. 

In deze zelfstudie detecteert de Human Resources-app uitingen over het verplaatsen van nieuwe werknemers vanaf één plaats naar een andere. Deze twee soorten uitingen zijn hetzelfde, maar ze worden opgelost met verschillende LUIS-mogelijkheden.

|Zelfstudie|Voorbeeld utterance|Bron en doel-locaties|
|--|--|--|
|[Hiërarchisch (Er zijn geen rollen)](luis-quickstart-intent-and-hier-entity.md)|MV Jill Jones van **a-2349** naar **b-1298**|a-2349, b-1298|
|In deze zelfstudie (met functies)|Verplaatsen van Billy Patterson van **Yuma** naar **Denver**.|Yuma, Denver|

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie een entiteit met de rollen en een doel met voorbeeld uitingen toegevoegd. Het eerste eindpunt voorspellen met behulp van de entiteit juist voorspeld de bedoeling, maar met een lage betrouwbaarheidsscore. Slechts één van de twee entiteiten is gedetecteerd. De zelfstudie toegevoegd vervolgens een patroon dat de entiteit-rollen en een woordgroepenlijst met gebruikt om de waarde van de namen van steden in de uitingen te verbeteren. De tweede eindpunt voorspelling geretourneerd van de score van een hoge betrouwbaarheid en beide rollen entiteit gevonden. 

> [!div class="nextstepaction"]
> [Informatie over aanbevolen procedures voor LUIS-apps](luis-concept-best-practices.md)
