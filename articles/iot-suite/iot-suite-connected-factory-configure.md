---
title: Configureren van de topologie verbonden factory | Microsoft Docs
description: Het configureren van de topologie van een verbonden factory vooraf geconfigureerde oplossing.
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: 19e0f48ab817428a1f953c80296b2e23effe5a8a
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="configure-the-connected-factory-preconfigured-solution"></a>De verbonden factory vooraf geconfigureerde oplossing configureren

De verbonden factory vooraf geconfigureerde oplossing ziet u een gesimuleerde dashboard voor een fictief bedrijf Contoso. Dit bedrijf heeft fabrieken in talloze globale locaties wereldwijd.

In dit artikel maakt gebruik van Contoso als voorbeeld voor het beschrijven van het configureren van de topologie van een verbonden factory-oplossing.

## <a name="simulated-factories-configuration"></a>Gesimuleerde fabrieken configuratie

Elke Contoso-factory heeft een productie-regels die bestaan uit drie stations. Elk station is een echte OPC UA-server met een specifieke rol:

* Assembly-station
* Station testen
* Verpakking station

Deze servers OPC UA OPC UA knooppunten hebben en [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) verzendt de waarden van deze knooppunten naar verbonden factory. Dit omvat:

* Huidige operationele status zoals huidige energieverbruik.
* Productie-informatie zoals het aantal producten die wordt geproduceerd.

Het dashboard kunt u inzoomen op de Contoso factory-topologie van een globale weergave omlaag naar de weergave van een station-niveau. Maakt gebruik van het dashboard verbonden factory:

* De visualisatie van afbeeldingen OEE en KPI's voor elke laag in de topologie.
* De visualisatie van huidige waarden van OPC UA-knooppunten in de stations.
* De samenvoeging van de cijfers OEE en KPI's van het niveau van het station op globaal niveau.
* De visualisatie van waarschuwingen en acties uit te voeren als de waarden voor specifieke reach-drempelwaarden.

## <a name="connected-factory-topology"></a>Verbonden factory-topologie

De topologie van de fabrieken, de regels voor productie en de stations is hiërarchische:

* Het globale niveau heeft factory knooppunten als onderliggende elementen.
* De fabrieken hebben productie regel knooppunten als onderliggende elementen.
* De regels van de productie hebben station knooppunten als onderliggende elementen.
* De stations (OPC UA-servers) hebben OPC UA knooppunten als onderliggende elementen.

Elk knooppunt in de topologie bevat een gemeenschappelijke set eigenschappen die bepalen:

* Een unieke id voor de topologie-knooppunt.
* Een naam.
* Een beschrijving.
* Een afbeelding.
* De onderliggende elementen van het knooppunt topologie.
* Minimum, doel- en maximumwaarden voor afbeeldingen van OEE en KPI's en de waarschuwing acties uit te voeren.

## <a name="topology-configuration-file"></a>Topologie-configuratiebestand

Als u wilt de eigenschappen die worden vermeld in de vorige sectie hebt geconfigureerd de verbonden factory-oplossing maakt gebruik van een configuratiebestand aangeroepen [ContosoTopologyDescription.json](https://github.com/Azure/azure-iot-connected-factory/blob/master/WebApp/Contoso/Topology/ContosoTopologyDescription.json).

U kunt dit bestand vinden in de broncode van de oplossing in de `WebApp/Contoso/Topology` map.

Het volgende fragment toont een overzicht van de `ContosoTopologyDescription.json` configuratiebestand:

```json
{
  <global_configuration>,
  "Factories": [
    <factory_configuration>,
    "ProductionLines": [
      <production_line_configuration>,
      "Stations": [
        <station_configuration>,
        <more station_configurations>
      ],
      <more production_line_configurations>
    ]
    <more factory_configurations>
  ]
}
```

De algemene eigenschappen van `<global_configuration>`, `<factory_configuration>`, `<production_line_configuration>`, en `<station_configuration>` zijn:

* **Naam** (Typ tekenreeks)

  Hiermee definieert een beschrijvende naam die moet worden slechts één voor de topologie-knooppunt in het dashboard worden weergegeven.

* **Beschrijving** (Typ tekenreeks)

  Hierin wordt beschreven in het knooppunt topologie in meer detail.

* **Afbeelding** (Typ tekenreeks)

  Het pad naar een installatiekopie in de WebApp-oplossing om weer te geven wanneer informatie over het knooppunt topologie wordt weergegeven in het dashboard.

* **OeeOverall**, **OeePerformance**, **OeeAvailability**, **OeeQuality**, **Kpi1**, **Kpi2** (type `<performance_definition>`)

  Deze eigenschappen definiëren minimum, doel en maximale waarden van de operationele afbeelding gebruikt om waarschuwingen te genereren. Deze eigenschappen worden ook de acties worden uitgevoerd als er een waarschuwing wordt gedetecteerd.

De `<factory_configuration>` en `<production_line_configuration>` items hebben een eigenschap:

* **GUID** (Typ tekenreeks)

  Een unieke identificatie van het knooppunt topologie.

`<factory_configuration>`heeft een eigenschap:

* **Locatie** (type `<location_definition>`)

  Hiermee geeft u op waar de factory zich bevindt.

`<station_configuration>`bevat de eigenschappen:

* **OpcUri** (Typ tekenreeks)

  Deze eigenschap moet worden ingesteld op de OPC UA aanvraag-URI van de OPC UA-server.
  Omdat het moet wereldwijd uniek door OPC UA-specificatie, wordt deze eigenschap wordt gebruikt om de topologie station knooppunt te identificeren.

* **OpcNodes**, die een matrix van OPC UA knooppunten zijn (type `<opc_node_description>`)

`<location_definition>`bevat de eigenschappen:

* **Plaats** (Typ tekenreeks)

  Naam van plaats die het dichtst bij de locatie

* **Land** (Typ tekenreeks)

  Land van de locatie

* **Breedtegraad** (double-type)

  Breedte van de locatie

* **Lengtegraad** (double-type)

  Lengtegraad van de locatie

`<performance_definition>`bevat de eigenschappen:

* **Minimale** (double-type)

  Laagste drempel de waarde kan bereiken. Als de huidige waarde lager dan deze drempelwaarde is, wordt een waarschuwing gegenereerd.

* **Doel** (double-type)

  De doelwaarde is ideaal.

* **Maximale** (double-type)

  Hoogste drempel de waarde kan bereiken. Als de huidige waarde hoger dan deze drempelwaarde is, wordt een waarschuwing gegenereerd.

* **MinimumAlertActions** (type `<alert_action>`)

  Definieert de set acties die kunnen worden genomen als reactie op een minimale waarschuwing.

* **MaximumAlertActions** (type `<alert_action>`)

  Definieert de set acties die kunnen worden genomen als reactie op een maximale waarschuwing.

`<alert_action`> heeft eigenschappen:

* **Type** (Typ tekenreeks)

  Type van de actie waarschuwing. De volgende typen zijn bekende:

  * **AcknowledgeAlert**: de status van de waarschuwing moet worden gewijzigd tot bevestigd.
  * **CloseAlert**: alle oudere waarschuwingen van hetzelfde type moeten niet meer worden weergegeven in het dashboard.
  * **CallOpcMethod**: een OPC UA-methode moet worden aangeroepen.
  * **OpenWebPage**: een browservenster moet geopend zijn aanvullende contextuele informatie wordt weergegeven.

* **Beschrijving** (Typ tekenreeks)

  Beschrijving van de actie die wordt weergegeven in het dashboard.

* **Parameter** (Typ tekenreeks)

  Parameters voor het uitvoeren van de actie vereist. De waarde is afhankelijk van het actietype.

  * **AcknowledgeAlert**: Er is geen parameter vereist.
  * **CloseAlert**: Er is geen parameter vereist.
  * **CallOpcMethod**: het knooppunt informatie en de parameters van de OPC UA-methode aan te roepen in de notatie 'NodeId van bovenliggend knooppunt, NodeId van methode aan te roepen, URI van de OPC UA-server.'
  * **OpenWebPage**: de URL in het browservenster worden weergegeven.

`<opc_node_description>`bevat informatie over OPC UA-knooppunten in een station (OPC UA-server). Knooppunten die geen bestaande OPC UA-knooppunten vertegenwoordigen, maar worden gebruikt als opslag in de berekening logica van verbonden factory zijn ook geldig. Deze heeft de volgende eigenschappen:

* **NodeId** (Typ tekenreeks)

  Adres van de OPC UA adresruimte knooppunt in het station (OPC UA van server). Syntaxis moet zoals opgegeven in de OPC UA-specificatie voor een NodeId.

* **Symbolische naam** (Typ tekenreeks)

  De naam moet worden weergegeven in het dashboard wanneer de waarde van deze OPC UA-knooppunt wordt weergegeven.

* **Relevantie** (matrix van het type tekenreeks)

  Hiermee wordt aangegeven waarvoor de berekening van OEE of KPI de knooppuntwaarde OPC UA relevant is. Elk matrixelement kan een van de volgende waarden zijn:

  * **OeeAvailability_Running**: de waarde is relevant voor de berekening van de beschikbaarheid van OEE.
  * **OeeAvailability_Fault**: de waarde is relevant voor de berekening van de beschikbaarheid van OEE.
  * **OeePerformance_Ideal**: de waarde is relevant voor de berekening van de prestaties van OEE en is meestal een constante waarde.
  * **OeePerformance_Actual**: de waarde is relevant voor de berekening van de prestaties van OEE.
  * **OeeQuality_Good**: de waarde is relevant voor de berekening van de kwaliteit van OEE.
  * **OeeQuality_Bad**: de waarde is relevant voor de berekening van de kwaliteit van OEE.
  * **Kpi1**: de waarde voor de berekening van KPI1 relevant is.
  * **Kpi2**: de waarde voor de berekening van KPI2 relevant is.

* **OpCode** (Typ tekenreeks)

  Hiermee wordt aangegeven hoe de waarde van het OPC UA-knooppunt wordt verwerkt in query's Time Series inzicht en OEE/KPI berekeningen. Elke query Time Series inzicht is bedoeld voor een specifieke timespan, die een parameter van de query en een resultaat te bezorgen. De OpCode bepaalt hoe het resultaat wordt berekend en kan een van de volgende waarden:

  * **Diff**: verschil tussen het laatste en de eerste waarde in de timespan.
  * **Gem.**: het gemiddelde van alle waarden in de timespan.
  * **Som**: de som van alle waarden in de timespan.
  * **Laatste**: momenteel niet gebruikt.
  * **Aantal**: het aantal waarden in de timespan.
  * **Maximale**: de maximale waarde in de timespan.
  * **Min**: de minimale waarde in de timespan.
  * **Const**: het resultaat is de waarde gespecificeerd door de eigenschap ConstValue.
  * **SubMaxMin**: het verschil tussen het maximale en de minimale waarde.
  * **TimeSpan**: de timespan.

* **Eenheden** (Typ tekenreeks)

  Definieert een eenheid van de waarde voor weergave in het dashboard.

* **Zichtbaar** (Typ Booleaanse waarde)

  Bepaalt of de waarde moet worden weergegeven in het dashboard.

* **ConstValue** (double-type)

  Als de **OpCode** is **Const**, dan is deze eigenschap de waarde van het knooppunt.

* **Minimale** (double-type)

  Als de huidige waarde onder deze waarde valt, wordt een minimale waarschuwing gegenereerd.

* **Maximale** (double-type)

  Als de huidige waarde wordt gegeven boven deze waarde, wordt een maximale waarschuwing gegenereerd.

* **MinimumAlertActions** (type `<alert_action>`)

  Definieert de set acties die kunnen worden genomen als reactie op een minimale waarschuwing.

* **MaximumAlertActions** (type `<alert_action>`)

  Definieert de set acties die kunnen worden genomen als reactie op een maximale waarschuwing.

Op het niveau van het station, ziet u ook **simulatie** objecten. Deze objecten worden alleen gebruikt voor het configureren van de simulatie verbonden factory en mag niet worden gebruikt voor het configureren van een echte topologie.

## <a name="how-the-configuration-data-is-used-at-runtime"></a>Hoe de configuratiegegevens wordt gebruikt tijdens runtime

Alle eigenschappen die in het configuratiebestand gebruikt kunnen worden gegroepeerd in verschillende categorieën, afhankelijk van hoe ze worden gebruikt. Deze categorieën zijn:

### <a name="visual-appearance"></a>Visuele weergave

In deze categorie definiëren het uiterlijk van het dashboard verbonden factory. Voorbeelden zijn:

* Naam
* Beschrijving
* Installatiekopie
* Locatie
* Eenheden
* Zichtbaar

### <a name="internal-topology-tree-addressing"></a>Interne topologie structuur adressering

De WebApp onderhoudt een interne gegevenswoordenboek met gegevens van alle knooppunten van de topologie. De eigenschappen **Guid** en **OpcUri** worden als sleutels gebruikt voor toegang tot deze woordenlijst en moet uniek zijn.

### <a name="oeekpi-computation"></a>OEE/KPI berekening

De cijfers OEE/KPI voor de simulatie verbonden factory zijn parameters gebruikt:

* De OPC UA knooppuntwaarden moeten worden opgenomen in de berekening.
* Hoe de afbeelding wordt berekend op basis van de telemetrie-waarden.

De formules OEE verbonden factory gebruikt door de http://oeeindustrystandard.oeefoundation.org gepubliceerd.

OPC UA knooppuntobjecten in stations inschakelen voor gebruik in de berekening OEE/KPI-tagging. De **relevantie** eigenschap wordt aangegeven voor welke afbeelding OEE/KPI de knooppuntwaarde OPC UA moet worden gebruikt. De **OpCode** eigenschap wordt gedefinieerd hoe de waarde in de berekening is opgenomen.

### <a name="alert-handling"></a>Afhandeling van waarschuwing

Verbonden factory ondersteunt een mechanisme voor eenvoudige minimum/maximum voor op basis van een drempelwaarde voor het genereren van waarschuwingen. Er zijn een aantal vooraf gedefinieerde acties die u kunt in reactie op waarschuwingen configureren. De volgende eigenschappen beheren dit mechanisme:

* Maximum
* Minimum
* MaximumAlertActions
* MinimumAlertActions

## <a name="correlating-to-telemetry-data"></a>Correleren van telemetriegegevens

Voor bepaalde bewerkingen, zoals de laatste waarde te visualiseren of Time Series inzicht query's, maken de WebApp moet een adresschema gebruiken voor de telemetriegegevens opgenomen. De telemetrie die is verzonden naar verbonden factory moet ook worden opgeslagen in interne gegevensstructuur. De twee eigenschappen voor het inschakelen van deze bewerkingen zijn op station (OPC UA-server) en OPC UA knooppuntniveau:

* **OpcUri**

  Identificeert vandaan (globaal unieke) de OPC UA-server de telemetrie. Deze eigenschap wordt in de opgenomen berichten verzonden als **ApplicationUri**.

* **NodeId**

  Geeft de waarde van een knooppunt in de OPC UA-server. De indeling van de eigenschap moet zoals opgegeven in de OPC UA-specificatie. Deze eigenschap wordt in de opgenomen berichten verzonden als **NodeId**.

Controleer [dit](https://github.com/Azure/iot-edge-opc-publisher) GitHub-pagina voor meer informatie over hoe de telemetriegegevens naar verbonden factory met de uitgever OPC wordt ingenomen.

## <a name="example-how-kpi1-is-calculated"></a>Voorbeeld: Hoe KPI1 wordt berekend

De configuratie in de `ContosoTopologyDescription.json` bestand bepaalt hoe OEE/KPI cijfers worden berekend. Het volgende voorbeeld ziet hoe de eigenschappen in dit bestand de berekening van de KPI1 beheren.

Verbonden geproduceerd factory die kpi1 wordt gebruikt voor het succes meten van het aantal producten in het afgelopen uur. Elk station (OPC UA-server) in de simulatie verbonden factory biedt een OPC UA-knooppunt (`NodeId: "ns=2;i=385"`), waarmee u de telemetrie deze KPI berekenen.

De configuratie voor dit knooppunt OPC UA eruit ziet het volgende fragment:

```json
{
  "NodeId": "ns=2;i=385",
  "SymbolicName": "NumberOfManufacturedProducts",
  "Relevance": [ "Kpi1", "OeeQuality_Good" ],
  "OpCode": "SubMaxMin"
},
```

Deze configuratie kan opvragen van de waarden van de telemetrie van dit knooppunt met Time Series Insights. De Time Series Insights query worden opgehaald:

* Het aantal waarden.
* De minimale waarde.
* De maximale waarde.
* Het gemiddelde van alle waarden.
* De som van alle waarden voor alle unieke **OpcUri** (**ApplicationUri**), **NodeId** -paren in een bepaalde TimeSpan-waarde.

Een kenmerk van de **NumberOfManufactureredProducts** knooppuntwaarde is dat alleen de bijbehorende waarde stijgt. Het aantal producten in de timespan berekenen verbonden factory gebruikt de **OpCode** **SubMaxMin**. De berekening haalt de minimale waarde aan het begin van de timespan en de maximumwaarde aan het einde van de timespan.

De **OpCode** configureert u de berekening logica voor het berekenen van het resultaat van het verschil tussen de maximale en minimale waarde in de configuratie. De resultaten worden vervolgens onder naar het hoofdniveau (global) verzameld en weergegeven in het dashboard.

## <a name="next-steps"></a>Volgende stappen

Een voorstel voor de volgende stap is te leren hoe u [Implementeer een gateway op Windows of Linux verbonden factory vooraf geconfigureerde oplossing voor](iot-suite-connected-factory-gateway-deployment.md).