---
title: 'Functies: Actie en de context - Personalizer'
titleSuffix: Azure Cognitive Services
description: Personalizer gebruikt functies, informatie over acties en context, om betere ranking-suggesties maken. Functies kunnen worden zeer algemene of specifiek voor een item.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: edjez
ms.openlocfilehash: c317cbec02b82743c233bf36f743cea808c30c69
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68253585"
---
# <a name="features-are-information-about-actions-and-context"></a>Functies zijn informatie over acties en context

De service Personalizer werkt door te leren wat uw toepassing moet worden weergegeven voor gebruikers in een bepaalde context.

Maakt gebruik van personalizer **functies**, dit is informatie over de **huidige context** het beste kiezen **actie**. De functies vertegenwoordigen alle gegevens die nuttig kan zijn voor het bereiken van hogere beloningen aanpassen. Functies kunnen worden zeer algemene of specifiek voor een item. 

Bijvoorbeeld, u mogelijk een **functie** over:

* De _gebruiker_ , zoals een `UserID`. 
* De _inhoud_ bijvoorbeeld als een video is een `Documentary`, een `Movie`, of een `TV Series`, of of een handelsversie-item is beschikbaar in de store.
* De _huidige_ periode van de tijd, zoals welke dag van de week is.

Personalizer niet schrijven, beperken, of welke functies u voor acties en context verzenden kunt oplossen:

* Als u deze niet hebt, kunt u sommige functies voor sommige acties en niet voor andere verzenden. Tv-serie kan bijvoorbeeld kenmerken films geen hebt.
* Mogelijk hebt u enkele functies die beschikbaar zijn slechts enkele voorbeelden van momenten. Een mobiele toepassing kan bijvoorbeeld informatie dan een webpagina te bieden. 
* Na verloop van tijd kunt u toevoegen en verwijderen van functies over context en acties. Personalizer blijft Leer van de beschikbare informatie.
* Er moet ten minste één functie voor de context. Personalizer biedt geen ondersteuning voor de context van een leeg zijn. Als u alleen een vaste context telkens verzendt, kies Personalizer de actie voor classificaties alleen met betrekking tot de functies in de acties. 
* Personalizer wordt geprobeerd om te kiezen acties die het meest voor iedereen op elk gewenst moment geschikt.

## <a name="supported-feature-types"></a>Ondersteunde functie typen

Personalizer biedt ondersteuning voor functies van de reeks, numerieke en Booleaanse typen.

### <a name="how-choice-of-feature-type-affects-machine-learning-in-personalizer"></a>Hoe de keuze van het functietype is van invloed op Machine Learning in Personalizer

* **Tekenreeksen**: Voor tekenreekstypen maakt elke combinatie van sleutel en waarde nieuwe gewicht in de Personalizer machine learning-model. 
* **Numerieke**: Als het getal moet proportioneel invloed hebben op het resultaat van persoonlijke instellingen, moet u numerieke waarden gebruiken. Dit is zeer afhankelijk scenario. In het voorbeeld van een vereenvoudigde bijvoorbeeld wanneer een handelsversie personaliseren ondervindt, NumberOfPetsOwned kan een functie die numeriek is, kunt u mensen met 2 of 3 huisdieren invloed op het persoonlijke instellingen resultaat die twee of drie keer zoveel met 1 huisdier zijn. Functies die zijn gebaseerd op numerieke eenheden, maar waarbij de betekenis niet lineaire - zoals leeftijd, temperatuur of hoogte van de persoon - zijn beste gecodeerd als tekenreeksen en de kwaliteit van de functie kan doorgaans worden verbeterd door het gebruik van bereiken. Bijvoorbeeld, leeftijd kan worden gecodeerd als "Ouder": "0-5", "Leeftijd": "6-10", enzovoort.
* **Booleaanse** waarden die worden verzonden met de waarde 'false' act alsof ze op alle hadn't is verzonden.

Functies die niet aanwezig zijn, moeten worden weggelaten uit de aanvraag. Vermijd het verzenden van functies met een null-waarde, omdat deze wordt verwerkt als bestaande en met de waarde 'null' als het model te trainen.

## <a name="categorize-features-with-namespaces"></a>Functies met naamruimten categoriseren

Personalizer wordt in de functies die zijn ingedeeld in naamruimten. U hebt vastgesteld, in uw toepassing, als de naamruimten worden gebruikt en wat ze moeten worden. Naamruimten worden gebruikt voor het groeperen van functies over een vergelijkbaar onderwerp of functies die afkomstig zijn van een bepaalde bron.

Hier volgen enkele voorbeelden van functie naamruimten die worden gebruikt door toepassingen:

* User_Profile_from_CRM
* Time
* Mobile_Device_Info
* http_user_agent
* VideoResolution
* UserDeviceInfo
* Weer
* Product_Recommendation_Ratings
* current_time
* NewsArticle_TextAnalytics

U kunt de functie naamruimten volgen van uw eigen conventies, zolang ze geldige JSON-sleutels zijn naam. Naamruimten worden gebruikt om te organiseren van functies in verschillende sets en te onderscheiden van functies met vergelijkbare namen. U kunt zien van naamruimten als 'voorvoegsel' die is toegevoegd aan de onderdeelnamen. Naamruimten kan niet worden genest.


In de volgende JSON `user`, `state`, en `device` functie naamruimten. Opmerking van de openbare preview-versie: Op dit moment wordt ten zeerste aangeraden namen voor functie-naamruimten die UTF-8 op basis van zijn en beginnen met verschillende letters. Bijvoorbeeld, `user`, `state`, en `device` beginnen met `u`, `s`, en `d`. Op dit moment met naamruimten met dezelfde tekens als eerste kan leiden tot conflicten in indexen die wordt gebruikt voor machine learning.

JSON-objecten kunnen geneste JSON-objecten en eenvoudige eigenschapswaarden bevatten. Een matrix kan worden opgenomen alleen als de matrixitems getallen zijn. 

```JSON
{
    "contextFeatures": [
        { 
            "user": {
                "profileType":"AnonymousUser",
                "latlong": [47.6, -122.1]
            }
        },
        {
            "state": {
                "timeOfDay": "noon",
                "weather": "sunny"
            }
        },
        {
            "device": {
                "mobile":true,
                "Windows":true
            }
        }
    ]
}
```

## <a name="how-to-make-feature-sets-more-effective-for-personalizer"></a>Hoe u de functie stelt effectiever voor Personalizer

Een goede functieset helpt Personalizer meer informatie over het voorspellen van de actie die de basis van de hoogste prijs. 

Houd rekening met functies te verzenden naar de Personalizer positie-API die volgt u deze aanbevelingen:

* Er zijn onvoldoende functies tot persoonlijke instellingen voor station. Hoe meer precies de inhoud moet worden toegepast, meer functies nodig zijn.

* Er zijn onvoldoende kenmerken van diverse *dichtheden*. Een functie is *compacte* als veel items worden gegroepeerd in een paar buckets. Duizenden video's kunnen bijvoorbeeld worden geclassificeerd als "Long" (meer dan 5 minuten lang) en 'Korte' (onder 5 minuten lang). Dit is een *zeer compacte* functie. De dezelfde duizenden items hebben daarentegen, een kenmerk met de naam "Title", waarvoor bijna nooit dezelfde waarde van het ene item naar een andere. Dit is een zeer niet-dense of *sparse* functie.  

Met functies van high-densitysampling helpt bij het leren van het ene item naar een andere extrapoleren Personalizer. Maar als er slechts een paar functies en ze te compacte zijn, de Personalizer probeert te nauwkeurig doel inhoud met slechts een paar buckets om uit te kiezen.

### <a name="improve-feature-sets"></a>Functiesets verbeteren 

Analyseer het gedrag van gebruikers aan de hand van een Offline-evaluatie. Hiermee kunt u om te kijken naar oude gegevens om te zien welke functies intensief een bijdrage levert aan positieve voordelen ten opzichte van die minder bijdrage levert. U kunt zien welke functies, helpen en deze worden maximaal u en uw toepassing betere functies vinden om te verzenden naar Personalizer resultaten nog verder verbeteren.

Deze volgende secties bevatten algemene procedures voor het verbeteren van functies die worden verzonden naar Personalizer.

#### <a name="make-features-more-dense"></a>Functies meer compacte maken

Het is mogelijk voor het verbeteren van uw functiesets door te bewerken om deze grotere en meer of minder compacte maken.

Een tijdstempel omlaag naar de tweede is bijvoorbeeld een zeer sparse functie. Kan dat het meer compacte (effectief) worden gemaakt door te classificeren tijden in "ochtend", "twaalf uur 's middags", "middag", enzovoort.


#### <a name="expand-feature-sets-with-extrapolated-information"></a>Vouw de functiesets geëxtrapoleerde informatie

U kunt ook meer functies ophalen door na te denken van vakjes kenmerken die kunnen worden afgeleid van informatie die u al hebt. Bijvoorbeeld in een lijst met fictieve film personalisatie, is het mogelijk dat een weekend vs weekdag elicits verschillend gedrag van gebruikers? Tijd kan worden uitgebreid naar een kenmerk 'weekend' of 'dag'. Culturele feestdagen station aandacht wordt besteed aan bepaalde filmtypen? Een kenmerk 'Halloween' is bijvoorbeeld handig in de locaties waar deze relevant is. Is het mogelijk dat en weer Regenachtig aanzienlijke invloed op de keuze van een film voor veel mensen heeft? Met de datum en tijd, kan een weerservice bieden dat gegevens en u deze als een extra functie toevoegen kunt. 

#### <a name="expand-feature-sets-with-artificial-intelligence-and-cognitive-services"></a>Vouw de functiesets met kunstmatige intelligentie en cognitive services

Kunstmatige intelligentie en kant-en-klaar-Cognitive Services is een zeer krachtige toevoeging aan de Personalizer. 

U kunt informatie die relevant zijn voor persoonlijke instellingen zijn automatisch ophalen door voorverwerking van de items die met behulp van kunstmatige intelligentie-services.

Bijvoorbeeld:

* U kunt een filmbestand via uitvoeren [Video Indexer](https://azure.microsoft.com/services/media-services/video-indexer/) om op te halen scène-elementen, tekst, sentiment en veel andere kenmerken. Deze kenmerken kunnen vervolgens meer compacte in overeenstemming met kenmerken die de itemmetagegevens van de oorspronkelijke niet worden gemaakt. 
* Installatiekopieën kunnen worden uitgevoerd via de objectdetectie, gezichten via sentiment, enzovoort.
* Gegevens in de tekst kan worden uitgebreid door te extraheren van entiteiten, sentiment, het uitbreiden van entiteiten met Bing knowledge graph, enzovoort.

U kunt verschillende andere [Azure Cognitive Services](https://www.microsoft.com/cognitive-services), bijvoorbeeld

* [Entiteiten koppelen](../entitylinking/home.md)
* [Tekstanalyse](../text-analytics/overview.md)
* [Emoties](../emotion/home.md)
* [Computer Vision](../computer-vision/home.md)

## <a name="actions-represent-a-list-of-options"></a>Acties vertegenwoordigen een lijst met opties

Elke actie:

* Een id heeft.
* Bevat een lijst met functies.
* De lijst van kenmerken kan grote (honderdtallen) maar raden wij aan evaluatie van de effectiviteit van de functie als u wilt verwijderen van functies die niet zijn een bijdrage levert aan beloningen ophalen. 
* De functies in de **acties** mogelijk of beschikt niet over een correlatie met functies in de **context** door Personalizer gebruikt.
* Functies voor acties kunnen worden gebruikt in bepaalde acties en andere niet. 
* Functies voor een bepaalde actie-ID kunnen één dag beschikbaar zijn, maar later niet meer beschikbaar. 

Personalizer van machine learning-algoritmen functioneren beter wanneer er stabiel functiesets zijn, maar positie aanroepen niet mislukken als de functie wijzigingen na verloop van tijd ingesteld.

Zenden geen in meer dan 50 acties wanneer Trefwoordenrangschikking acties. Dit is mogelijk dezelfde 50 acties telkens of die kunnen worden gewijzigd. Bijvoorbeeld, als u een productcatalogus van 10.000 items voor een e-commerce-toepassing hebt, kunt u een aanbeveling of filtering engine om te bepalen van de bovenkant 40 een klant kan wilt en Personalizer gebruiken om het te vinden die de meeste derden (bijvoorbeeld genereren de gebruiker wordt toegevoegd aan het mandje) voor de huidige context.


### <a name="examples-of-actions"></a>Voorbeelden van acties

De acties die u naar de API positie verzendt afhankelijk van wat u probeert om aan te passen.

Hier volgen enkele voorbeelden:

|Doel|Action|
|--|--|
|Aan persoonlijke voorkeuren aanpassen waarop artikel op een nieuwswebsite is gemarkeerd.|Elke actie is een mogelijke nieuwsartikel.|
|Optimaliseer ad plaatsing op een website.|Elke actie is een lay-out- of regels voor het maken van een lay-out voor de advertenties (bijvoorbeeld: op de bovenkant op de juiste, kleine afbeeldingen, grote afbeeldingen).|
|Persoonlijke rangorde van de aanbevolen items op een winkelwagen website weergegeven.|Elke actie wordt een specifiek product.|
|Gebruikersinterface-elementen, zoals filters toe te passen op een specifieke foto voorstellen.|Elke actie mogelijk een ander filter.|
|Kies een chatbot antwoord om te verduidelijken bedoeling van gebruikers of een actie voorstellen.|Elke actie kan worden gebruikt bij het interpreteren van de reactie.|
|Kies wat u wilt weergeven boven aan een lijst met zoekresultaten|Elke actie is een van de hoogste aantal zoekresultaten.|


### <a name="examples-of-features-for-actions"></a>Voorbeelden van functies voor acties

De volgende zijn goede voorbeelden van functies voor acties. Deze afhankelijk van veel elke toepassing.

* Functies met kenmerken van de acties. Bijvoorbeeld, is het een film of een reeks tv-programma?
* Functies over hoe gebruikers zijn aan de slag met deze actie in het verleden kunt kunnen. Bijvoorbeeld, deze film voornamelijk is gezien door mensen in demografische gegevens A of B, meestal wordt afgespeeld niet meer dan één keer.
* Functies over de kenmerken van hoe u de gebruiker *ziet* de acties. Bijvoorbeeld, wordt de poster voor de film wordt weergegeven in de miniaturen opnemen gezichten, auto's of landschappen?

### <a name="load-actions-from-the-client-application"></a>Acties van de clienttoepassing laden

Functies van de acties kunnen meestal afkomstig zijn van content management systems, catalogi en recommender systemen. Uw toepassing is verantwoordelijk voor het laden van de informatie over de acties van de relevante databases en systemen die u hebt. Als uw acties niet te wijzigen of hun aandacht geladen telkens onnodige invloed op de prestaties heeft, kunt u de logica in uw toepassing om deze gegevens in cache toevoegen.

### <a name="prevent-actions-from-being-ranked"></a>Voorkomen dat de acties op basis van wordt gerangschikt

In sommige gevallen zijn er acties die u niet wilt weergeven voor gebruikers. De beste manier om te voorkomen dat een actie uit die worden beoordeeld als bovenste is niet wilt opnemen in de lijst met de API positie in de eerste plaats.

In sommige gevallen deze kan alleen worden bepaald later in uw bedrijfslogica als een resultaat _actie_ van een API positie aanroep moet worden weergegeven aan een gebruiker. Voor deze gevallen moet u _inactieve gebeurtenissen_.

## <a name="json-format-for-actions"></a>JSON-indeling voor acties

Bij het aanroepen van positie, ontvangt u meerdere acties kiezen uit:

JSON-objecten kunnen geneste JSON-objecten en eenvoudige eigenschapswaarden bevatten. Een matrix kan worden opgenomen alleen als de matrixitems getallen zijn. 

```json
{
    "actions": [
    {
      "id": "pasta",
      "features": [
        {
          "taste": "salty",
          "spiceLevel": "medium",
          "grams": [400,800]
        },
        {
          "nutritionLevel": 5,
          "cuisine": "italian"
        }
      ]
    },
    {
      "id": "ice cream",
      "features": [
        {
          "taste": "sweet",
          "spiceLevel": "none",
          "grams": [150, 300, 450]
        },
        {
          "nutritionalLevel": 2
        }
      ]
    },
    {
      "id": "juice",
      "features": [
        {
          "taste": "sweet",
          "spiceLevel": "none",
          "grams": [300, 600, 900]
        },
        {
          "nutritionLevel": 5
        },
        {
          "drink": true
        }
      ]
    },
    {
      "id": "salad",
      "features": [
        {
          "taste": "salty",
          "spiceLevel": "low",
          "grams": [300, 600]
        },
        {
          "nutritionLevel": 8
        }
      ]
    }
  ]
}
```

## <a name="examples-of-context-information"></a>Voorbeelden van contextinformatie

Informatie over de _context_ is afhankelijk van elk geval de toepassing en het gebruik, maar deze kan omvatten informatie zoals:

* Informatie over uw gebruiker demografische en profiel.
* De informatie opgehaald uit de HTTP-headers, zoals de gebruikersagent, of is afgeleid van HTTP-informatie, zoals omgekeerde geografische lookups op basis van IP-adressen.
* Informatie over de huidige tijd, zoals de dag van de week, weekend of niet, ochtend of bijkomen, feestdagen of niet, enzovoort.
* Informatie van mobiele toepassingen, zoals locatie, verplaatsing of accuniveau hebt uitgepakt.
* Historische statistische functies van het gedrag van gebruikers -, zoals wat de film genres die u gebruikt zijn deze gebruiker heeft de meest bekeken.

Uw toepassing is verantwoordelijk voor het laden van de informatie over de context van de relevante databases, sensoren en systemen die u hebt. Als uw contextinformatie niet wijzigen, kunt u bedrijfslogica toevoegen in uw toepassing om deze informatie, voordat deze worden verzonden naar de API van de positie in cache.

## <a name="json-format-for-context"></a>JSON-indeling voor context 

Context wordt uitgedrukt als een JSON-object dat wordt verzonden naar de API positie:

JSON-objecten kunnen geneste JSON-objecten en eenvoudige eigenschapswaarden bevatten. Een matrix kan worden opgenomen alleen als de matrixitems getallen zijn. 

```JSON
{
    "contextFeatures": [
        { 
            "user": {
                "name":"Doug"
            }
        },
        {
            "state": {
                "timeOfDay": "noon",
                "weather": "sunny"
            }
        },
        {
            "device": {
                "mobile":true,
                "Windows":true,
                "screensize": [1680,1050]
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Volgende stappen

[Bekrachtigingen](concepts-reinforcement-learning.md) 
