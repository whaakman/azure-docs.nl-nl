---
title: Configureren van de verbonden Factory-topologie - Azure | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van de topologie van een oplossingsverbetering voor verbonden Factory.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: 3fd160fbccfb5298cefed6a731797ca6962b997c
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53602270"
---
# <a name="configure-the-connected-factory-solution-accelerator"></a>De oplossingsversneller voor verbonden Factory configureren

De oplossingsversneller voor verbonden Factory ziet u een gesimuleerde dashboard voor het fictieve bedrijf Contoso. Dit bedrijf heeft fabrieken op talloze globale locaties wereldwijd.

In dit artikel maakt gebruik van Contoso als voorbeeld voor het beschrijven van de topologie van een oplossing voor verbonden Factory configureren.

## <a name="simulated-factories-configuration"></a>Configuratie van de gesimuleerde factory 's

Elke Contoso-fabriek heeft productielijnen die bestaan uit drie stations. Elk station is een echte OPC UA-server met een specifieke functie:

* Assembly-station
* Testen van station
* Verpakkingsstation

Deze OPC UA-servers hebt OPC UA-knooppunten en [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) verzendt de waarden van deze knooppunten naar verbonden Factory. Dit omvat:

* De huidige operationele status, zoals de huidige energieverbruik.
* Productie-informatie, zoals het aantal producten die worden geproduceerd.

U kunt het dashboard gebruiken om te zoomen op de topologie van de factory Contoso uit een algemeen overzicht omlaag naar een weergave van de op stationsniveau. Het dashboard van Connected Factory kunt:

* De visualisatie van de OEE en KPI's cijfers voor elke laag in de topologie.
* De visualisatie van de huidige waarden van OPC UA-knooppunten in de stations.
* De aggregatie van de OEE en KPI's cijfers van het niveau van het station op globaal niveau.
* De visualisatie van waarschuwingen en acties uit te voeren als de waarden voor specifieke drempelwaarden bereiken.

## <a name="connected-factory-topology"></a>Verbonden Factory-topologie

De topologie van de fabrieken productielijnen en stations zijn hiërarchisch:

* Het globale niveau bevat factory knooppunten als onderliggende items.
* De fabrieken hebben productielijn knooppunten als onderliggende items.
* De productielijnen hebben station knooppunten als onderliggende items.
* De stations (OPC UA-servers) hebben OPC UA-knooppunten als onderliggende items.

Elk knooppunt in de topologie bevat een gemeenschappelijke set eigenschappen die bepalen:

* Een unieke id voor de topologie-knooppunt.
* Een naam.
* Een beschrijving.
* Een afbeelding.
* De onderliggende objecten van het knooppunt topologie.
* Minimum-, doel- en maximumwaarden voor OEE en KPI's cijfers en de waarschuwing acties uit te voeren.

## <a name="topology-configuration-file"></a>Topologie-configuratiebestand

Als u wilt de eigenschappen die worden vermeld in de vorige sectie hebt geconfigureerd de verbonden Factory-oplossing maakt gebruik van een configuratiebestand genaamd [ContosoTopologyDescription.json](https://github.com/Azure/azure-iot-connected-factory/blob/master/WebApp/Contoso/Topology/ContosoTopologyDescription.json).

U kunt dit bestand vinden in de broncode van de oplossing in de `WebApp/Contoso/Topology` map.

Het volgende codefragment toont een overzicht van de `ContosoTopologyDescription.json` configuratiebestand:

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

De algemene eigenschappen van `<global_configuration>`, `<factory_configuration>`, `<production_line_configuration>`, en `<station_configuration>` zijn:

* **Naam** (type string)

  Definieert een beschrijvende naam, die moet worden slechts één voor het knooppunt topologie om weer te geven in het dashboard.

* **Beschrijving** (type string)

  Hierin wordt beschreven in het knooppunt topologie in meer detail.

* **Afbeelding** (type string)

  Het pad naar een installatiekopie in de WebApp-oplossing om weer te geven wanneer de informatie over de topologie-knooppunt in het dashboard wordt weergegeven.

* **OeeOverall**, **OeePerformance**, **OeeAvailability**, **OeeQuality**, **Kpi1**, **Kpi2** (type `<performance_definition>`)

  Deze eigenschappen definiëren minimum, doel en maximale waarden van de operationele afbeelding die wordt gebruikt om waarschuwingen te genereren. Deze eigenschappen worden ook de acties uit te voeren als er een waarschuwing wordt gedetecteerd.

De `<factory_configuration>` en `<production_line_configuration>` items hebben een eigenschap:

* **GUID** (type string)

  Unieke identificatie van het knooppunt topologie.

`<factory_configuration>` heeft een eigenschap:

* **Locatie** (type `<location_definition>`)

  Hiermee geeft u op waar de factory zich bevindt.

`<station_configuration>` heeft eigenschappen:

* **OpcUri** (type string)

  Deze eigenschap moet worden ingesteld op de URI van de OPC UA van OPC UA-server.
  Omdat de waarde moet globaal uniek zijn door OPC UA-specificatie, wordt deze eigenschap wordt gebruikt om het knooppunt van de topologie station te identificeren.

* **OpcNodes**, die een matrix met OPC UA-knooppunten zijn (type `<opc_node_description>`)

`<location_definition>` heeft eigenschappen:

* **Plaats** (type string)

  Naam van plaats die het dichtst bij de locatie

* **Land/regio** (type string)

  Land van de locatie

* **Breedtegraad** (type double)

  Breedtegraad van de locatie

* **Lengtegraad** (type double)

  Lengtegraad van de locatie

`<performance_definition>` heeft eigenschappen:

* **Minimale** (type double)

  Laagste drempel de waarde kunt bereiken. Als de huidige waarde lager dan deze drempelwaarde is, wordt een waarschuwing gegenereerd.

* **Doel** (type double)

  Ideaal doelwaarde.

* **Maximale** (type double)

  Hoogste drempel de waarde kunt bereiken. Als de huidige waarde hoger dan deze drempelwaarde is, wordt een waarschuwing gegenereerd.

* **MinimumAlertActions** (type `<alert_action>`)

  Definieert de set acties die kunnen worden uitgevoerd als reactie op een minimale waarschuwing.

* **MaximumAlertActions** (type `<alert_action>`)

  Definieert de set acties die kunnen worden uitgevoerd als reactie op een waarschuwing voor een maximale.

`<alert_action`> heeft eigenschappen:

* **Type** (type string)

  Het type van de actie bij waarschuwing. De volgende typen zijn bekend:

  * **AcknowledgeAlert**: de status van de waarschuwing naar bevestigde moet wijzigen.
  * **CloseAlert**: alle oudere waarschuwingen van hetzelfde type moeten niet meer worden weergegeven in het dashboard.
  * **CallOpcMethod**: een OPC UA-methode moet worden aangeroepen.
  * **OpenWebPage**: een browservenster moet worden geopend met aanvullende contextuele informatie.

* **Beschrijving** (type string)

  Beschrijving van de actie die wordt weergegeven in het dashboard.

* **Parameter** (type string)

  Parameters die zijn vereist voor het uitvoeren van de actie. De waarde is afhankelijk van het actietype.

  * **AcknowledgeAlert**: Er is geen parameter die is vereist.
  * **CloseAlert**: Er is geen parameter die is vereist.
  * **CallOpcMethod**: het knooppunt informatie en de parameters van de OPC UA-methode aan te roepen in de indeling "NodeId van bovenliggend knooppunt, NodeId van methode om aan te roepen, URI van de OPC UA-server."
  * **OpenWebPage**: de URL om weer te geven in het browservenster.

`<opc_node_description>` bevat informatie over OPC UA-knooppunten in een station (OPC UA-server). Knooppunten die geen bestaande OPC UA-knooppunten vertegenwoordigen, maar die worden gebruikt als opslag gebruiken in de berekeningslogica van Connected Factory zijn ook geldig. Het heeft de volgende eigenschappen:

* **NodeId** (type string)

  Adres van de OPC UA-knooppunt in het station (OPC UA-server van)-adresruimte. Syntaxis moet zoals opgegeven in de OPC UA-specificatie voor een NodeId.

* **Symbolische naam** (type string)

  De naam moet worden weergegeven in het dashboard wanneer de waarde van deze OPC UA-knooppunt wordt weergegeven.

* **Relevantie** (matrix van het typetekenreeks)

  Hiermee wordt aangegeven waarvoor berekening van de OEE- of KPI de waarde van de OPC UA-knooppunt is van belang. Elk matrixelement kan een van de volgende waarden zijn:

  * **OeeAvailability_Running**: de waarde is van belang voor berekening van de beschikbaarheid van OEE is.
  * **OeeAvailability_Fault**: de waarde is van belang voor berekening van de beschikbaarheid van OEE is.
  * **OeePerformance_Ideal**: de waarde is van belang voor berekening van de OEE-prestaties en is meestal een constante waarde.
  * **OeePerformance_Actual**: de waarde is van belang voor berekening van de OEE-prestaties.
  * **OeeQuality_Good**: de waarde is van belang voor berekening van de OEE-kwaliteit is.
  * **OeeQuality_Bad**: de waarde is van belang voor berekening van de OEE-kwaliteit is.
  * **Kpi1**: de waarde is van belang voor berekening van KPI1.
  * **Kpi2**: de waarde is van belang voor berekening van KPI2.

* **OpCode** (type string)

  Geeft aan hoe de waarde van de OPC UA-knooppunt in Time Series Insight-query's en OEE-/ KPI-berekeningen worden verwerkt. Elke query Time Series Insight is bedoeld voor een specifieke periode, die is een parameter van de query en biedt een resultaat. De OpCode bepaalt hoe het resultaat wordt berekend en kan een van de volgende waarden:

  * **Diff**: verschil tussen het laatste en de eerste waarde in de periode.
  * **Gem.**: het gemiddelde van alle waarden in de periode.
  * **Som**: de som van alle waarden in de periode.
  * **Laatste**: op dit moment niet gebruikt.
  * **Aantal**: het aantal waarden in de periode.
  * **Maximale**: de maximale waarde in de periode.
  * **Min**: de minimale waarde in de periode.
  * **Const**: het resultaat is de waarde gespecificeerd door de eigenschap ConstValue.
  * **SubMaxMin**: het verschil tussen het maximale en de minimale waarde.
  * **TimeSpan**: het interval.

* **Eenheden** (type string)

  Definieert een eenheid van de waarde om weer te geven in het dashboard.

* **Zichtbaar** (het type boolean)

  Bepaalt of de waarde moet worden weergegeven in het dashboard.

* **ConstValue** (type double)

  Als de **OpCode** is **Const**, dan is deze eigenschap de waarde van het knooppunt.

* **Minimale** (type double)

  Als de huidige waarde onder deze waarde valt, wordt klikt u vervolgens een minimale waarschuwing gegenereerd.

* **Maximale** (type double)

  Als de huidige waarde boven deze waarde genereert, wordt klikt u vervolgens een maximale waarschuwing gegenereerd.

* **MinimumAlertActions** (type `<alert_action>`)

  Definieert de set acties die kunnen worden uitgevoerd als reactie op een minimale waarschuwing.

* **MaximumAlertActions** (type `<alert_action>`)

  Definieert de set acties die kunnen worden uitgevoerd als reactie op een waarschuwing voor een maximale.

Op het niveau van het station, ziet u ook **simulatie** objecten. Deze objecten zijn alleen gebruikt voor het configureren van de verbonden Factory-simulatie en moeten niet worden gebruikt voor het configureren van een echte topologie.

## <a name="how-the-configuration-data-is-used-at-runtime"></a>Hoe de configuratiegegevens wordt gebruikt tijdens runtime

Alle eigenschappen die in het configuratiebestand gebruikt kunnen worden gegroepeerd in verschillende categorieën, afhankelijk van hoe ze worden gebruikt. Deze categorieën zijn:

### <a name="visual-appearance"></a>Visuele weergave

De eigenschappen in deze categorie definiëren het uiterlijk van de verbonden Factory-dashboard. Voorbeelden zijn:

* Name
* Description
* Installatiekopie
* Locatie
* Eenheden
* Zichtbaar

### <a name="internal-topology-tree-addressing"></a>Topologie van de interne structuur adressering

De Web-App onderhoudt de woordenlijst van een interne gegevens met gegevens van alle knooppunten van de topologie. De eigenschappen **Guid** en **OpcUri** als sleutels worden gebruikt voor toegang tot deze woordenlijst en moet uniek zijn.

### <a name="oeekpi-computation"></a>OEE-/ KPI-berekening

De OEE-/ KPI-afbeeldingen voor de verbonden Factory-simulatie bestaan parameters door:

* De OPC UA-knooppunt-waarden moeten worden opgenomen in de berekening.
* Hoe de afbeelding wordt berekend van de telemetriewaarden.

Verbonden Factory's worden de OEE-formules gebruikt zoals gepubliceerd door de https://www.oeefoundation.org.

Objecten van de OPC UA-knooppunt in stations inschakelen voor gebruik in de berekening van de OEE-/ KPI-tagging. De **relevantie** eigenschap wordt aangegeven voor welke OEE-/ KPI-afbeelding de waarde van de OPC UA-knooppunt moet worden gebruikt. De **OpCode** eigenschap definieert hoe de waarde in de berekening is opgenomen.

### <a name="alert-handling"></a>Afhandeling van waarschuwingen

Verbonden Factory ondersteunt een mechanisme voor eenvoudige minimum/maximum aantal op basis van een drempelwaarde voor het genereren van waarschuwingen. Er zijn een aantal vooraf gedefinieerde acties die u in reactie op deze waarschuwingen configureren kunt. Dit mechanisme beheren van de volgende eigenschappen:

* Maximum
* Minimum
* MaximumAlertActions
* MinimumAlertActions

## <a name="correlating-to-telemetry-data"></a>Aan de telemetriegegevens te correleren

Voor bepaalde bewerkingen, zoals het visualiseren van de laatste waarde of query's van Time Series Insight, maakt de Web-App moet een adresschema gebruiken voor de opgenomen telemetriegegevens. De telemetrie die verzonden naar verbonden Factory moet ook worden opgeslagen in interne gegevensstructuur. De twee eigenschappen voor het inschakelen van deze bewerkingen zijn op station (OPC UA-server) en OPC UA-knooppunt:

* **OpcUri**

  Identificeert vandaan (unieke) OPC UA-server de telemetrie. Deze eigenschap wordt in de opgenomen berichten verzonden als **ApplicationUri**.

* **NodeId**

  Geeft de knooppuntwaarde in de OPC UA-server. Moet de indeling van de eigenschap zijn zoals opgegeven in de OPC UA-specificatie. Deze eigenschap wordt in de opgenomen berichten verzonden als **NodeId**.

Controleer [dit](https://github.com/Azure/iot-edge-opc-publisher) GitHub-pagina voor meer informatie over hoe de telemetrische gegevens die worden is opgenomen in de verbonden Factory met behulp van de OPC-uitgever.

## <a name="example-how-kpi1-is-calculated"></a>Voorbeeld: Hoe KPI1 wordt berekend

De configuratie in de `ContosoTopologyDescription.json` bestand bepaalt hoe de OEE-/ KPI-afbeeldingen worden berekend. Het volgende voorbeeld laat zien hoe de eigenschappen in dit bestand de berekening van KPI1 beheren.

In de verbonden Factory KPI1 wordt gebruikt voor het meten van het aantal producten is in het afgelopen uur. Elk station (OPC UA-server) in de verbonden Factory-simulatie biedt een OPC UA-knooppunt (`NodeId: "ns=2;i=385"`), waarmee u de telemetrie voor het berekenen van deze KPI.

De configuratie voor deze OPC UA-knooppunt lijkt op het volgende codefragment:

```json
{
  "NodeId": "ns=2;i=385",
  "SymbolicName": "NumberOfManufacturedProducts",
  "Relevance": [ "Kpi1", "OeeQuality_Good" ],
  "OpCode": "SubMaxMin"
},
```

Deze configuratie kunt query's naar de telemetriewaarden van dit knooppunt met Time Series Insights. De Time Series Insights-query worden opgehaald:

* Het aantal waarden.
* De minimale waarde.
* De maximale waarde.
* Het gemiddelde van alle waarden.
* De som van alle waarden voor alle unieke **OpcUri** (**ApplicationUri**), **NodeId** -paren in een bepaalde periode.

Een kenmerk van de **NumberOfManufactureredProducts** knooppuntwaarde is dat alleen toeneemt. Voor het berekenen van het aantal producten in de timespan verbonden Factory maakt gebruik van de **OpCode** **SubMaxMin**. De berekening worden de minimale waarde aan het begin van het interval en de maximale waarde aan het einde van de timespan opgehaald.

De **OpCode** configureert u de berekeningslogica voor het berekenen van het resultaat van het verschil tussen de maximale en minimale waarde in de configuratie. De resultaten worden vervolgens onder tot het hoogste niveau (wereldwijde) doen samengevoegd en weergegeven in het dashboard.

## <a name="next-steps"></a>Volgende stappen

Een aanbevolen volgende stap is te leren hoe u [een gateway implementeren op Windows of Linux voor de oplossingsverbetering voor verbonden Factory](iot-accelerators-connected-factory-gateway-deployment.md).
