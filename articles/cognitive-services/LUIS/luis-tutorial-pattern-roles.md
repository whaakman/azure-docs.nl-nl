---
title: Zelfstudie over het patroon rollen gebruiken voor het verbeteren van LUIS voorspellingen - Azure | Microsoft Docs
titleSuffix: Cognitive Services
description: In deze zelfstudie, gebruikt u patroon rollen voor contextueel gerelateerde entiteiten voor het verbeteren van LUIS voorspellingen.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 07/20/2018
ms.author: v-geberr;
ms.openlocfilehash: 938b868fadfa102174b270a222494710fc156442
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39174563"
---
# <a name="tutorial-improve-app-with-pattern-roles"></a>Zelfstudie: Een app met behulp van patroon verbeteren

In deze zelfstudie, gebruikt u een enkele entiteit met de rollen in combinatie met patronen te verhogen van de intentie en entiteit voorspelling.  Wanneer u patronen gebruikt, zijn minder voorbeeld uitingen nodig voor het doel.

> [!div class="checklist"]
* Patroon rollen begrijpen
* Gebruik eenvoudige entiteit met rollen 
* Patroon voor uitingen met behulp van eenvoudige entiteit met de rol maken
* Patroon voorspelling verbeteringen controleren

Voor dit artikel hebt u een gratis [LUIS](luis-reference-regions.md)-account nodig om uw LUIS-toepassing te creëren.

## <a name="before-you-begin"></a>Voordat u begint
Als u geen de Human Resources-app vanuit de [patroon](luis-tutorial-pattern.md) zelfstudie [importeren](luis-how-to-start-new-app.md#import-new-app) de JSON naar een nieuwe app in de [LUIS](luis-reference-regions.md#luis-website) website. De app voor het importeren is gevonden in de [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-patterns-HumanResources-v2.json) GitHub-opslagplaats.

Als u de oorspronkelijke Human Resources-app wilt gebruiken, kloont u de versie op de pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) en wijzigt u de naam in `roles`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. 

## <a name="the-purpose-of-roles"></a>Het doel van rollen
Het doel van de rollen is om op te halen van context-gerelateerde entiteiten in een utterance. In de utterance `Move new employee Robert Williams from Sacramento and San Francisco`, wordt de oorspronkelijke plaats en plaats van bestemming waarden aan elkaar zijn gerelateerd en algemene taal gebruiken om aan te duiden elke locatie. 

Wanneer u patronen, alle entiteiten in het patroon moeten worden gedetecteerd _voordat_ het patroon overeenkomt met de utterance. 

Wanneer u een patroon maakt, wordt de eerste stap is het selecteren van het doel van het patroon. U selecteert het doel, als het patroon overeenkomt, de juiste intentie altijd geretourneerd met een hoge score (meestal 99-100%). 

### <a name="compare-hierarchical-entity-to-simple-entity-with-roles"></a>Hiërarchische entiteit met eenvoudige entiteit met behulp van vergelijken

In de [hiërarchische zelfstudie](luis-quickstart-intent-and-hier-entity.md), wordt de **MoveEmployee** bedoeling gedetecteerd wanneer een bestaande werknemer van een gebouw en office verplaatsen naar een andere. De voorbeeld-uitingen bron en doel-locaties heeft maar rollen niet hebt gebruikt. In plaats daarvan zijn de bron en doel onderliggende items van de hiërarchische entiteit. 

In deze zelfstudie detecteert de Human Resources-app uitingen over het verplaatsen van nieuwe werknemers vanaf één plaats naar een andere. Deze twee soorten uitingen zijn vergelijkbaar, maar ze worden opgelost met verschillende LUIS-mogelijkheden.

|Zelfstudie|Voorbeeld utterance|Bron en doel-locaties|
|--|--|--|
|[Hiërarchisch (Er zijn geen rollen)](luis-quickstart-intent-and-hier-entity.md)|MV Jill Jones van **a-2349** naar **b-1298**|a-2349, b-1298|
|In deze zelfstudie (met functies)|Verplaatsen van Billy Patterson van **Yuma** naar **Denver**.|Yuma, Denver|

U kunt de hiërarchische entiteit in het patroon kan gebruiken, omdat alleen hiërarchische ouders worden gebruikt in de bovenliggende items. Als u wilt retourneren de benoemde locaties van de bron en doel, muse u gebruikt een patroon.

### <a name="simple-entity-for-new-employee-name"></a>Eenvoudige entiteit voor de naam van een nieuwe werknemer
De naam van de nieuwe werknemer, Billy Patterson, maakt geen deel uit van de entiteit lijst **werknemer** nog. De naam van de nieuwe werknemer wordt eerst opgehaald om de naam van de verzenden naar een extern systeem te maken van de bedrijfsreferenties. Nadat de bedrijfsreferenties zijn gemaakt, de referenties van de werknemer zijn toegevoegd aan de lijst met entiteit **werknemer**.

De **werknemer** lijst is gemaakt in de [lijst zelfstudie](luis-quickstart-intent-and-list-entity.md).

De **NewEmployee** entiteit is een eenvoudige entiteit met er zijn geen rollen. 

### <a name="simple-entity-with-roles-for-relocation-cities"></a>Eenvoudige entiteit met de rollen voor verplaatsing van steden
De nieuwe werknemer en familie moeten worden verplaatst van de huidige plaats naar een plaats waar het fictieve bedrijf gevestigd is. Omdat een nieuwe medewerker afkomstig van een stad zijn kan, moeten de locaties worden gedetecteerd. Een lijst met instellen, zoals een lijst met entiteit werkt niet omdat alleen de plaatsen in de lijst met zou worden geëxtraheerd.

De rolnamen die is gekoppeld aan de bron en doel steden moeten uniek zijn in alle entiteiten. Er is een eenvoudige manier om te controleren of dat de rollen die uniek zijn om ze te koppelen aan de containerentiteit via een naamgeving. De **NewEmployeeRelocation** entiteit is een eenvoudige entiteit met twee rollen: **NewEmployeeReloOrigin** en **NewEmployeeReloDestination**.

### <a name="simple-entities-need-enough-examples-to-be-detected"></a>Eenvoudige entiteiten moeten voldoende voorbeelden om te worden gedetecteerd
Omdat de voorbeeld-utterance `Move new employee Robert Williams from Sacramento and San Francisco` heeft alleen machine geleerde entiteiten, het is belangrijk om voldoende uitingen voorbeeld met de intent bieden, zodat de entiteiten worden gedetecteerd.  

**Terwijl patronen u minder voorbeeld uitingen kunnen, als de entiteiten niet worden gedetecteerd, wordt het patroon komt niet overeen.**

Als u problemen met detectie van eenvoudige entiteit hebt omdat deze een naam, bijvoorbeeld een stad, kunt u overwegen een woordgroepenlijst met vergelijkbare waarden. Hiermee wordt de detectie van de naam van de stad door middel van LUIS een extra signaal over dit type woord of woordgroep. Het patroon te woordgroep lijsten alleen helpen met detectie van de entiteit, die nodig voor het patroon is voor de overeenkomst. 

## <a name="create-new-entities"></a>Nieuwe entiteiten maken
1. Selecteer **bouwen** in het bovenste menu.

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

## <a name="train-the-luis-app"></a>LUIS-app trainen
De nieuwe intentie en uitingen vereisen training. 

1. Selecteer rechtsboven op de website van LUIS de knop **Train**.

2. Het trainen is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het trainen is gelukt.

## <a name="publish-the-app-to-get-the-endpoint-url"></a>App publiceren om eindpunt-URL op te vragen
Om LUIS een voorspelling te laten geven in een chatbot of een andere toepassing, moet u de app publiceren. 

1. Selecteer rechtsboven op de website van LUIS de knop **Publish**. 

2. Selecteer de slot Production en vervolgens de knop **Publish**.

3. Het publiceren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het publiceren is gelukt.

## <a name="query-the-endpoint-without-pattern"></a>Query uitvoeren op het eindpunt zonder patroon
1. Selecteer onder aan de pagina **Publish** de koppeling **endpoint**. Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. 

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

Patronen helpt de voorspelling score, maar de entiteiten die moeten worden correct voorspeld voordat het patroon overeenkomt met de utterance. 

## <a name="add-a-pattern-that-uses-roles"></a>Een patroon dat gebruikmaakt van de rollen toevoegen
1. Selecteer **bouwen** in de bovenste navigatiebalk.

2. Selecteer **patronen** in het linkernavigatievenster.

3. Selecteer **NewEmployeeRelocationProcess** uit de **een doel selecteren** vervolgkeuzelijst. 

4. Voer in het volgende patroon: `move {NewEmployee} from {NewEmployeeRelocation:NewEmployeeReloOrigin} to {NewEmployeeRelocation:NewEmployeeReloDestination}[.]`

    Als u trainen, publiceren en het eindpunt van de query, kunt u mogelijk doen om te zien dat de entiteiten niet zijn gevonden, zodat het patroon is niet overeenkomen, dus de voorspelling niet verbeteren. Dit is een gevolg van onvoldoende uitingen voorbeeld met gelabelde entiteiten. In plaats van het toevoegen van meer voorbeelden, een woordgroepenlijst u lost dit probleem wilt toevoegen.

## <a name="create-a-phrase-list-for-cities"></a>Een woordgroepenlijst met voor steden maken
Steden, zoals de namen van personen zijn moeilijk in dat ze elke combinatie van woorden en leestekens worden kunnen. Maar het plaatsen van de regio en de hele wereld bekend zijn, dus LUIS moet een woordgroepenlijst plaatsen om te beginnen met leren. 

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

    Voeg geen elke plaats in de hele wereld of zelfs elke plaats in de regio. LUIS moet kunnen om te generaliseren wat een plaats in de lijst is. 

    Zorg ervoor dat u houden **deze waarden zijn verwisselbaar** geselecteerde. Deze instelling betekent de woorden in de lijst op behandeld als synoniemen. Dit is precies hoe ze moeten worden behandeld in het patroon.

    Houd er rekening mee [voor het laatst](luis-quickstart-primary-and-secondary-data.md) de reeks een woordgroepenlijst gemaakt is ook om de detectie van de entiteit van een enkele entiteit te verbeteren.  

3. Trainen en publiceren van de app.

## <a name="query-endpoint-for-pattern"></a>Eindpunt van de query voor het patroon
1. Selecteer onder aan de pagina **Publish** de koppeling **endpoint**. Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. 

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

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Om dit te doen, selecteer het weglatingsteken (***...*** ) aan de rechterkant van de naam van de app in de lijst met Apps, selecteer **verwijderen**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over aanbevolen procedures voor LUIS-apps](luis-concept-best-practices.md)