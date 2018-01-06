---
title: Azure Machine Learning-Afwijkingsdetectie API | Microsoft Docs
description: Afwijkingsdetectie Detection-API is een voorbeeld gebouwd met Microsoft Azure Machine Learning die afwijkingen gedetecteerd in de reeks tijdgegevens met numerieke waarden die gelijkmatig zijn verdeeld in de tijd.
services: machine-learning
documentationcenter: 
author: alokkirpal
manager: jhubbard
editor: cgronlun
ms.assetid: 52fafe1f-e93d-47df-a8ac-9a9a53b60824
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/05/2017
ms.author: alok;rotimpe
ms.openlocfilehash: e2adfffa00a726fe2c452c25dd777ef054319b04
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="machine-learning-anomaly-detection-api"></a>Machine Learning eigen Afwijkingsdetectie API
## <a name="overview"></a>Overzicht
[Afwijkingsdetectie detectie API](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2) is een voorbeeld gebouwd met Azure Machine Learning die afwijkingen gedetecteerd in de reeks tijdgegevens met numerieke waarden die gelijkmatig zijn verdeeld in de tijd.

Deze API kan de volgende soorten afwijkende patronen in de reeks tijdgegevens detecteren:

* **Positieve en negatieve trends**: bijvoorbeeld wanneer bewaking geheugengebruik in een opwaartse trend computing interessant kan zijn omdat dit mogelijk indicatief voor een geheugenlek
* **Wijzigingen in het dynamische waardenbereik**: bijvoorbeeld als u de uitzonderingen worden veroorzaakt door een cloudservice controleren, eventuele wijzigingen in het dynamisch bereik van waarden kunnen duiden op instabiliteit van de status van de service, en
* **Bereikt en daalt**: bijvoorbeeld bij de bewaking van het aantal mislukte aanmeldingen in een service of het nummer van de opdrachten om uitchecken in een e-commercesite pieken of dips kunnen duiden op abnormaal gedrag.

Dergelijke wijzigingen in waarden bijhouden gedurende een tijd- en de doorlopende wijzigingen in hun waarden op deze machine learning detectoren als afwijkingsdetectie scores. Zij vereisen geen ad-hoc drempelwaarde afstemmen en hun scores kunnen worden gebruikt voor het regelen false positief. De afwijkingsdetectie API handig in verschillende scenario's is zoals servicecontrole door bij te houden van KPI's gedurende een periode, bewaking van het gebruik door metrische gegevens zoals het aantal zoekopdrachten, aantallen klikken, door middel van items zoals geheugen, CPU, bewaking van toepassingsprestaties bestand leest, enzovoort. na verloop van tijd.

De aanbieding Afwijkingsdetectie wordt geleverd met nuttige hulpprogramma's om u op weg.

* De [webtoepassing](http://anomalydetection-aml.azurewebsites.net/) helpt u bij het evalueren en de resultaten van de afwijkingsdetectie API's voor uw gegevens te visualiseren.

> [!NOTE]
> Probeer **IT Afwijkingsdetectie Insights-oplossing** aangedreven door [deze API](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2)
> 
> Deze end-to-end-oplossing voor uw Azure-abonnement geïmplementeerd <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank"> **Begin hier >**</a>
> 
>

## <a name="api-deployment"></a>API-implementatie
Om de API gebruiken, moet u deze implementeren op uw Azure-abonnement waar deze wordt gehost als Azure Machine Learning-webservice.  U kunt dit doen vanuit de [AI-galerie van Azure](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2).  Dit implementeert twee AzureML-webservices (en hun verwante resources) naar uw Azure-abonnement - één voor afwijkingsdetectie seizoensgebonden opsporen en één zonder seizoensgebonden detectie.  Zodra de implementatie is voltooid, is het mogelijk voor het beheren van uw API's van de [AzureML-webservices](https://services.azureml.net/webservices/) pagina.  Op deze pagina kunt u zich kunt vinden van uw eindpuntlocaties, API-sleutels, evenals voorbeeldcode voor de API aanroept.  Meer gedetailleerde instructies vindt u [hier](https://docs.microsoft.com/azure/machine-learning/machine-learning-manage-new-webservice).

## <a name="scaling-the-api"></a>De API schalen
Uw implementatie hebben standaard een gratis Dev/Test-abonnement waaronder 1000 transacties/en 2 compute uren/maand.  U kunt upgraden naar een ander plan volgens uw behoeften.  Informatie over de prijzen van verschillende abonnementen beschikbaar zijn [hier](https://azure.microsoft.com/en-us/pricing/details/machine-learning/) onder 'Productie-Web-API prijzen'.

## <a name="managing-aml-plans"></a>Het beheren van AML plannen 
U kunt uw abonnement beheren [hier](https://services.azureml.net/plans/).  Naam van het abonnement wordt gebaseerd op de Resourcegroepnaam die u hebt gekozen bij het implementeren van de API, plus een tekenreeks die uniek is voor uw abonnement.  Instructies voor het upgraden van uw abonnement zijn beschikbaar [hier](https://docs.microsoft.com/azure/machine-learning/machine-learning-manage-new-webservice) onder de sectie 'Facturering abonnementen beheren'.

## <a name="api-definition"></a>API-definitie
De webservice biedt een op basis van REST-API die kan worden gebruikt op verschillende manieren een web- of mobiele toepassingen, R, Python, Excel, inclusief via HTTPS enzovoort.  U uw tijd reeksgegevens verzenden naar deze service via een REST-API-aanroep en het uitvoeren van een combinatie van de drie afwijkingsdetectie typen die hieronder worden beschreven.

## <a name="calling-the-api"></a>De API aanroept
U moet weten de eindpuntlocatie en de API-sleutel om de API aanroept.  Deze samen met de voorbeeldcode voor het aanroepen van de API zijn beschikbaar via de [AzureML-webservices](https://services.azureml.net/webservices/) pagina.  Navigeer naar de gewenste API en klik vervolgens op het tabblad 'Verbruiken' doen.  Houd er rekening mee dat u de API als een Swagger API kan aanroepen (dat wil zeggen met de URL-parameter `format=swagger`) of als een niet - Swagger API (d.w.z. zonder de `format` URL-parameter).  De voorbeeldcode maakt gebruik van de Swagger-indeling.  Hieronder volgt een voorbeeld van de aanvraag en antwoord in niet-Swagger-indeling.  Deze voorbeelden worden de seizoensgebonden-eindpunt.  Het eindpunt niet seizoensgebonden is vergelijkbaar.

### <a name="sample-request-body"></a>Voorbeeld aanvraagtekst
De aanvraag bevat twee objecten: `Inputs` en `GlobalParameters`.  In de onderstaande voorbeeldaanvraag sommige parameters worden verzonden, expliciet terwijl andere niet (Schuif omlaag voor een volledige lijst met parameters voor elk eindpunt).  Parameters die niet expliciet worden verzonden in de aanvraag gebruikt de standaardwaarden hieronder genoemd.

    {
                "Inputs": {
                        "input1": {
                                "ColumnNames": ["Time", "Data"],
                                "Values": [
                                        ["5/30/2010 18:07:00", "1"],
                                        ["5/30/2010 18:08:00", "1.4"],
                                        ["5/30/2010 18:09:00", "1.1"]
                                ]
                        }
                },
        "GlobalParameters": {
            "tspikedetector.sensitivity": "3",
            "zspikedetector.sensitivity": "3",
            "bileveldetector.sensitivity": "3.25",
            "detectors.spikesdips": "Both"
        }
    }

### <a name="sample-response"></a>Voorbeeldantwoord
Merk op dat, om te zien de `ColumnNames` veld, moet u opnemen `details=true` als een URL-parameter in uw aanvraag.  Zie de tabellen hieronder voor de zin achter elk van deze velden.

    {
        "Results": {
            "output1": {
                "type": "table",
                "value": {
                    "Values": [
                        ["5/30/2010 6:07:00 PM", "1", "1", "0", "0", "-0.687952590518378", "0", "-0.687952590518378", "0", "-0.687952590518378", "0"],
                        ["5/30/2010 6:08:00 PM", "1.4", "1.4", "0", "0", "-1.07030497733224", "0", "-0.884548154298423", "0", "-1.07030497733224", "0"],
                        ["5/30/2010 6:09:00 PM", "1.1", "1.1", "0", "0", "-1.30229513613974", "0", "-1.173800281031", "0", "-1.30229513613974", "0"]
                    ],
                    "ColumnNames": ["Time", "OriginalData", "ProcessedData", "TSpike", "ZSpike", "BiLevelChangeScore", "BiLevelChangeAlert", "PosTrendScore", "PosTrendAlert", "NegTrendScore", "NegTrendAlert"],
                    "ColumnTypes": ["DateTime", "Double", "Double", "Double", "Double", "Double", "Int32", "Double", "Int32", "Double", "Int32"]
                }
            }
        }
    }


## <a name="score-api"></a>Score-API
De Score-API wordt gebruikt voor het uitvoeren van de afwijkingsdetectie op niet-seizoensgebonden reeksgegevens. De API wordt een aantal afwijkingsdetectie detectoren uitgevoerd op de gegevens en hun scores afwijkingsdetectie retourneert. De afbeelding hieronder toont een voorbeeld van afwijkingen die kunnen worden gedetecteerd door de Score-API. Deze tijdreeks heeft 2 distinct verandert en 3 pieken. De rode puntjes tonen de tijd waarop de wijzigingen in het wordt gedetecteerd, terwijl de zwarte punten de gedetecteerde pieken weergeven.
![Score-API][1]

### <a name="detectors"></a>Detectoren
De afwijkingsdetectie API ondersteunt detectoren in 3 hoofdcategorieën. In de volgende tabel vindt meer informatie over specifieke invoerparameters en outputs voor elke detectie.

| Detectie-categorie | Detectie | Beschrijving | Invoerparameters | Uitvoer |
| --- | --- | --- | --- | --- |
| De detectoren piek |TSpike detectie |Pieken en dips op basis van veel van de waarden afkomstig zijn van de eerste en derde kwartielen detecteren |*tspikedetector.Sensitivity:* duurt geheel getal in het bereik 1-10-standaard: 3; Hogere waarden wordt onderschept meer extreme waarden, waardoor er minder gevoelig |TSpike: binaire waarden: '1' als een piek/dip wordt gedetecteerd, '0' anders |
| De detectoren piek | ZSpike detectie |Pieken en dips op basis van hoe ver de datapoints van hun gemiddelde zijn detecteren |*zspikedetector.Sensitivity:* nemen geheel getal in het bereik 1-10-standaard: 3; Hogere waarden wordt onderschept meer extreme waarden, zodat u minder gevoelig |ZSpike: binaire waarden: '1' als een piek/dip wordt gedetecteerd, '0' anders | |
| Detectie van trage Trend |Detectie van trage Trend |Detecteren van trage positieve trend aan de hand van de set gevoeligheid |*trenddetector.Sensitivity:* drempelwaarde bij detectie score (standaard: 3,25, 3,25 – 5 is een redelijke bereik te selecteren dat dit van; Hoe hoger minder gevoelig) |tscore: getal met drijvende voor afwijkingsdetectie score op trend |
| De detectoren niveau wijzigen | Detectie van bidirectionele niveau wijzigen |Omhoog en omlaag de wijzigingen aan de hand van de set gevoeligheid detecteren |*bileveldetector.Sensitivity:* drempelwaarde bij detectie score (standaard: 3,25, 3,25 – 5 is een redelijke bereik te selecteren dat dit van; Hoe hoger minder gevoelig) |rpscore: getal met drijvende voor afwijkingsdetectie score op omhoog en omlaag niveau wijzigen | |

### <a name="parameters"></a>Parameters
Meer gedetailleerde informatie over deze invoerparameters wordt in de onderstaande tabel vermeld:

| Invoerparameters | Beschrijving | Standaardinstelling | Type | Het geldige bereik | Voorgestelde bereik |
| --- | --- | --- | --- | --- | --- |
| detectors.historyWindow |Geschiedenis (in het aantal gegevenspunten) gebruikt voor afwijkingsdetectie score berekeningen |500 |geheel getal |10-2000 |Afhankelijk van de tijdreeks |
| detectors.spikesdips | Of moet worden gedetecteerd alleen bereikt, alleen dips of beide |Beide |opgesomd |Beide, Bronblokkades, Dips |Beide |
| bileveldetector.Sensitivity |Gevoeligheid voor bidirectionele niveau wijzigen detectie. |3.25 |dubbel |Geen |3,25-5 (minder waarden betekenen gevoeliger) |
| trenddetector.Sensitivity |Gevoeligheid voor positieve trend detectie. |3.25 |dubbel |None |3,25-5 (minder waarden betekenen gevoeliger) |
| tspikedetector.Sensitivity |Gevoeligheid voor TSpike detectie |3 |geheel getal |1-10 |3-5 (minder waarden betekenen gevoeliger) |
| zspikedetector.Sensitivity |Gevoeligheid voor ZSpike detectie |3 |geheel getal |1-10 |3-5 (minder waarden betekenen gevoeliger) |
| postprocess.tailRows |Nummer van de meest recente gegevenspunten worden bewaard in de uitvoer-resultaten |0 |geheel getal |0 (Houd alle gegevenspunten), of geef het aantal punten in resultaten |N/A |

### <a name="output"></a>Uitvoer
De API wordt alle detectoren uitgevoerd op de tijd reeksgegevens en retourneert afwijkingsdetectie scores en binaire piek indicatoren voor elk punt in tijd. De volgende tabel bevat de uitvoer van de API. 

| Uitvoer | Beschrijving |
| --- | --- |
| Time |Tijdstempels van onbewerkte gegevens, of geaggregeerde (en/of) impliciete gegevens als aggregatie (en/of) ontbreekt begrip van de gegevens wordt toegepast |
| Gegevens |Waarden van onbewerkte gegevens, of geaggregeerde (en/of) impliciete gegevens als aggregatie (en/of) ontbreekt begrip van de gegevens wordt toegepast |
| TSpike |Binaire indicator om aan te geven of een piek wordt gedetecteerd door TSpike detectie |
| ZSpike |Binaire indicator om aan te geven of een piek wordt gedetecteerd door ZSpike detectie |
| rpscore |Een zwevend nummer dat afwijkingsdetectie beoordelen op wijzigingen in twee richtingen |
| rpalert |1/0-waarde die aangeeft er is een niveau van bidirectionele wijzigen op basis van de invoer gevoeligheid afwijkingsdetectie |
| tscore |Een zwevend nummer dat afwijkingsdetectie beoordelen op positieve trend |
| talert |1/0-waarde die aangeeft er is een positieve trend afwijkingsdetectie op basis van de invoer gevoeligheid |

## <a name="scorewithseasonality-api"></a>ScoreWithSeasonality API
De API ScoreWithSeasonality wordt gebruikt voor het uitvoeren van de afwijkingsdetectie op timeseries waarvoor de seizoensgebonden patronen. Deze API is nuttig voor het detecteren van afwijkingen in de seizoensgebonden patronen.  
De volgende afbeelding toont een voorbeeld van afwijkingen gedetecteerd in een seizoen tijdreeks. De tijdreeks heeft een piek (1e zwarte stip), twee dips (de 2e zwarte punt en een aan het einde) en één niveau wijzigen (rode stip). Houd er rekening mee dat zowel het dip in het midden van de tijdreeks en de wijzigingen in het alleen discernable nadat seizoensgebonden onderdelen zijn verwijderd uit de reeks.
![Seizoensgebonden API][2]

### <a name="detectors"></a>Detectoren
De detectoren in de seizoensgebonden eindpunt zijn vergelijkbaar met die in het eindpunt niet seizoensgebonden, maar met verschillende parameternamen (Zie hieronder).

### <a name="parameters"></a>Parameters

Meer gedetailleerde informatie over deze invoerparameters wordt in de onderstaande tabel vermeld:

| Invoerparameters | Beschrijving | Standaardinstelling | Type | Het geldige bereik | Voorgestelde bereik |
| --- | --- | --- | --- | --- | --- |
| preprocess.aggregationInterval |Aggregatie-interval in seconden voor het verzamelen van de tijdreeks invoer |0 (geen aggregatie wordt uitgevoerd) |geheel getal |0: aggregatie > 0 anders overslaan |5 minuten tot 1 dag, afhankelijk van de tijdreeks |
| preprocess.aggregationFunc |Functie die wordt gebruikt voor het verzamelen van gegevens in de opgegeven AggregationInterval |gemiddelde |opgesomd |gemiddelde, sum, lengte |N/A |
| preprocess.replaceMissing |Waarden voor de ontbrekende gegevens rekenen |lkv (laatst bekende waarde) |opgesomd |nul, lkv, gemiddelde |N/A |
| detectors.historyWindow |Geschiedenis (in het aantal gegevenspunten) gebruikt voor afwijkingsdetectie score berekeningen |500 |geheel getal |10-2000 |Afhankelijk van de tijdreeks |
| detectors.spikesdips | Of moet worden gedetecteerd alleen bereikt, alleen dips of beide |Beide |opgesomd |Beide, Bronblokkades, Dips |Beide |
| bileveldetector.Sensitivity |Gevoeligheid voor bidirectionele niveau wijzigen detectie. |3.25 |dubbel |None |3,25-5 (minder waarden betekenen gevoeliger) |
| postrenddetector.Sensitivity |Gevoeligheid voor positieve trend detectie. |3.25 |dubbel |None |3,25-5 (minder waarden betekenen gevoeliger) |
| negtrenddetector.Sensitivity |Gevoeligheid voor negatieve trend detectie. |3.25 |dubbel |Geen |3,25-5 (minder waarden betekenen gevoeliger) |
| tspikedetector.Sensitivity |Gevoeligheid voor TSpike detectie |3 |geheel getal |1-10 |3-5 (minder waarden betekenen gevoeliger) |
| zspikedetector.Sensitivity |Gevoeligheid voor ZSpike detectie |3 |geheel getal |1-10 |3-5 (minder waarden betekenen gevoeliger) |
| seasonality.Enable |Hiermee wordt aangegeven of seizoensgebonden analyse wordt uitgevoerd |waar |booleaans |True, false |Afhankelijk van de tijdreeks |
| seasonality.numSeasonality |Maximum aantal periodieke cycli worden gedetecteerd |1 |geheel getal |1, 2 |1-2 |
| seasonality.transform |Of seizoensgebonden (en) trend onderdelen moeten worden verwijderd voordat u afwijkingsdetectie |deseason |opgesomd |None, deseason, deseasontrend |N/A |
| postprocess.tailRows |Nummer van de meest recente gegevenspunten worden bewaard in de uitvoer-resultaten |0 |geheel getal |0 (Houd alle gegevenspunten), of geef het aantal punten in resultaten |N/A |

### <a name="output"></a>Uitvoer
De API wordt alle detectoren uitgevoerd op de tijd reeksgegevens en retourneert afwijkingsdetectie scores en binaire piek indicatoren voor elk punt in tijd. De volgende tabel bevat de uitvoer van de API. 

| Uitvoer | Beschrijving |
| --- | --- |
| Time |Tijdstempels van onbewerkte gegevens, of geaggregeerde (en/of) impliciete gegevens als aggregatie (en/of) ontbreekt begrip van de gegevens wordt toegepast |
| OriginalData |Waarden van onbewerkte gegevens, of geaggregeerde (en/of) impliciete gegevens als aggregatie (en/of) ontbreekt begrip van de gegevens wordt toegepast |
| ProcessedData |Een van de volgende: <ul><li>Afhankelijk van het seizoen tijdreeks aangepast als aanzienlijke seizoensgebonden is gedetecteerd en deseason optie geselecteerd.</li><li>afhankelijk van het seizoen aangepast en detrended tijdreeks of aanzienlijke seizoensgebonden is gedetecteerd en deseasontrend optie is geselecteerd</li><li>anders is dit hetzelfde is als OriginalData</li> |
| TSpike |Binaire indicator om aan te geven of een piek wordt gedetecteerd door TSpike detectie |
| ZSpike |Binaire indicator om aan te geven of een piek wordt gedetecteerd door ZSpike detectie |
| BiLevelChangeScore |Een zwevend nummer dat afwijkingsdetectie beoordelen op niveau wijzigen |
| BiLevelChangeAlert |1/0-waarde die aangeeft er is een niveau wijzigen afwijkingsdetectie op basis van de invoer gevoeligheid |
| PosTrendScore |Een zwevend nummer dat afwijkingsdetectie beoordelen op positieve trend |
| PosTrendAlert |1/0-waarde die aangeeft er is een positieve trend afwijkingsdetectie op basis van de invoer gevoeligheid |
| NegTrendScore |Een zwevend nummer dat afwijkingsdetectie beoordelen op negatieve trend |
| NegTrendAlert |1/0-waarde die aangeeft er is een negatieve trend afwijkingsdetectie op basis van de invoer gevoeligheid |

[1]: ./media/apps-anomaly-detection-api/anomaly-detection-score.png
[2]: ./media/apps-anomaly-detection-api/anomaly-detection-seasonal.png

