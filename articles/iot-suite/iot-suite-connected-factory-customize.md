---
title: Azure IoT Suite verbonden factory aanpassen | Microsoft Docs
description: Een beschrijving van het aanpassen van het gedrag van de verbonden factory vooraf geconfigureerde oplossing.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: c#
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 90a6172dbd887ecda5a9f5d9082a4e136092bc10
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="customize-how-the-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Aanpassen hoe de gegevens van uw OPC UA-servers worden weergegeven in de verbonden factory-oplossing

## <a name="introduction"></a>Inleiding

De oplossing verbonden factory samenvoegt en gegevens uit de OPC UA-servers die is verbonden met de oplossing worden weergegeven. U kunt bladeren en opdrachten verzenden naar de OPC UA-servers in uw oplossing. Zie de [veelgestelde vragen over Connected Factory](iot-suite-faq-cf.md) voor meer informatie over OPC UA.

Voorbeelden van geaggregeerde gegevens in de oplossing zijn de algehele apparatuur efficiëntie (OEE) en de Key Performance Indicators (KPI's) die u in het dashboard op de factory-, lijn- en station niveaus bekijken kunt. De volgende schermafbeelding ziet u de waarden OEE en KPI's voor de **Assembly** station op **productie-regel 1**, in de **München** factory:

![Voorbeeld van OEE en KPI waarden in de oplossing][img-oee-kpi]

De oplossing kunt u gedetailleerde informatie weergeven vanaf specifieke gegevensitems uit de OPC UA-servers, zogenaamde *stations*. De volgende schermafbeelding ziet curve van het aantal geproduceerde items van een specifiek station:

![Curve van het aantal geproduceerde artikelen][img-manufactured-items]

Als u klikt op een van de grafieken, vindt u de gegevens verder met Time Series Insights (TSI):

![Gegevens met behulp van de Time Series Insights verkennen][img-tsi]

Dit artikel wordt beschreven:

- Hoe de gegevens beschikbaar worden gesteld aan de verschillende weergaven in de oplossing.
- Hoe u de manier waarop de oplossing kunt aanpassen, worden de gegevens weergegeven.

## <a name="data-sources"></a>Gegevensbronnen

De verbonden factory-oplossing worden gegevens uit de OPC UA-servers die is verbonden met de oplossing. De standaardinstallatie bevat verschillende OPC UA-servers met een factory simulatie. U kunt uw eigen servers OPC UA toevoegen die [verbinding maken via een gateway] [ lnk-connect-cf] aan uw oplossing.

U kunt de gegevensitems die een verbonden OPC UA-server om uw oplossing in het dashboard te verzenden kunt bladeren:

1. Navigeer naar de **selecteert u een server OPC UA** weergeven:

    ![Navigeer naar het selecteren van een weergave OPC UA-server][img-select-server]

1. Selecteer een server en klik op **Connect**. Klik op **doorgaan** wanneer de beveiligingswaarschuwing wordt weergegeven.

    > [!NOTE]
    > Deze waarschuwing alleen verschijnt eenmaal voor elke server en er wordt een vertrouwensrelatie tussen het dashboard van de oplossing en de server.

1. U kunt nu de gegevensitems die de server naar de oplossing verzenden kan bladeren. Items die worden verzonden naar de oplossing hebt een groen vinkje:

    ![Gepubliceerde items][img-published]

1. Als u een *beheerder* in de oplossing kunt u kiezen voor het publiceren van een gegevensitem zodat deze beschikbaar zijn in de verbonden factory-oplossing. U kunt ook de waarde van gegevensitems wijzigen en methoden aanroepen op de OPC UA-server als beheerder.

## <a name="map-the-data"></a>De gegevens moeten worden toegewezen

De oplossing verbonden factory toegewezen en de gepubliceerde-gegevensitems van de OPC UA-server samenvoegt met de verschillende weergaven in de oplossing. De verbonden factory-oplossing implementeert in uw Azure-account wanneer u de oplossing inricht. Een JSON-bestand in de Visual Studio verbonden factory oplossing slaat deze informatie over de Identiteitstoewijzing. U kunt weergeven en wijzigen van dit JSON-configuratiebestand in de verbonden fabriek Visual Studio-oplossing. U kunt de oplossing opnieuw implementeren nadat u een wijziging aanbrengt.

U kunt het configuratiebestand te gebruiken:

- Bewerk de bestaande gesimuleerde fabrieken, de regels voor productie en de stations.
- Gegevens van echte OPC UA-servers waarop u verbinding met de oplossing maken toewijzen.

Als u wilt klonen van een kopie van de verbonden factory Visual Studio-oplossing, kunt u de volgende git-opdracht gebruiken:

`git clone https://github.com/Azure/azure-iot-connected-factory.git`

Het bestand **ContosoTopologyDescription.json** definieert u de toewijzing van de items OPC UA-server gegevens naar de weergaven in het dashboard van de verbonden factory-oplossing. U vindt dit configuratiebestand in de **Contoso\Topology** map in de **WebApp** -project in Visual Studio-oplossing.

De inhoud van het JSON-bestand is ingedeeld als een hiërarchie van de factory-, productie-lijn- en station knooppunten. Deze hiërarchie definieert de navigatiehiërarchie in het dashboard verbonden factory. Waarden op elk knooppunt van de hiërarchie bepalen van de informatie die wordt weergegeven in het dashboard. Het JSON-bestand bevat bijvoorbeeld de volgende waarden voor de factory München:

```json
"Guid": "73B534AE-7C7E-4877-B826-F1C0EA339F65",
"Name": "Munich",
"Description": "Braking system",
"Location": {
    "City": "Munich",
    "Country": "Germany",
    "Latitude": 48.13641,
    "Longitude": 11.57754
},
"Image": "munich.jpg"
```

De naam, beschrijving en locatie worden weergegeven op deze weergave in het dashboard:

![München gegevens in het dashboard][img-munich]

Elke factory-, productie-lijn- en station hebt een eigenschap van de afbeelding. U vindt deze JPEG-bestanden in de **Content\img** map in de **WebApp** project. Deze installatiekopiebestanden weergeven in het dashboard verbonden factory.

Elk station bevat verschillende gedetailleerde eigenschappen waarmee de toewijzing van de gegevens OPC UA items definiëren. Deze eigenschappen worden beschreven in de volgende secties:

### <a name="opcuri"></a>OpcUri

De **OpcUri** waarde is de OPC UA toepassing URI die een unieke identificatie van de OPC UA-server. Bijvoorbeeld, de **OpcUri** waarde voor de assembly-station op productie regel 1 in München op het volgende lijkt: **urn: scada2194:ua:munich:productionline0:assemblystation**.

U kunt de URI's van de gekoppelde OPC UA-servers bekijken in het dashboard van oplossing:

![Het OPC UA-server URI's weergeven][img-server-uris]

### <a name="simulation"></a>Simulatie

De informatie in de **simulatie** knooppunt is specifiek voor de simulatie OPC UA die wordt uitgevoerd in de OPC UA-servers die standaard zijn ingericht. Dit wordt niet gebruikt voor een echte OPC UA-server.

### <a name="kpi1-and-kpi2"></a>Kpi1 en Kpi2

Deze knooppunten wordt beschreven hoe gegevens uit het station draagt bij aan de twee KPI-waarden in het dashboard. Deze KPI-waarden zijn in een standaardimplementatie eenheden per uur en kWh per uur. De oplossing KPI-waarden op het niveau van een station berekend en ze op het niveau van de factory en productie regel samenvoegt.

Elke KPI heeft een minimum, maximum- en doel-waarde. Elke KPI-waarde kunt ook definiëren voor meldingen voor de verbonden factory-oplossing om uit te voeren. Het volgende codefragment bevat de definities van de KPI voor de assembly-station op productie regel 1 in München:

```json
"Kpi1": {
  "Minimum": 150,
  "Target": 300,
  "Maximum": 600
},
"Kpi2": {
  "Minimum": 50,
  "Target": 100,
  "Maximum": 200,
  "MinimumAlertActions": [
    {
      "Type": "None"
    }
  ]
}
```

De volgende schermafbeelding ziet de KPI-gegevens in het dashboard.

![KPI-gegevens in het dashboard][lnk-kpi]

### <a name="opcnodes"></a>OpcNodes

De **OpcNodes** knooppunten de gepubliceerde gegevens herstellen vanaf de OPC UA-server en geef het verwerken van die gegevens.

De **NodeId** waarde identificeert de specifieke OPC UA NodeID van de OPC UA-server. Het eerste knooppunt in het station assembly voor productie-regel 1 in München heeft een waarde **ns = 2; ik 385 =**. Een **NodeId** waarde geeft aan dat het gegevensitem lezen uit de OPC UA-server en de **symbolische naam** biedt een gebruiksvriendelijke naam moet worden gebruikt in het dashboard voor die gegevens.

Andere waarden die zijn gekoppeld aan elk knooppunt worden in de volgende tabel samengevat:

| Waarde | Beschrijving |
| ----- | ----------- |
| Relevantie  | Waarden voor de KPI en OEE deze gegevens draagt bij aan. |
| OpCode     | Hoe de gegevens worden samengevoegd. |
| Eenheden      | De eenheden die worden gebruikt in het dashboard.  |
| Zichtbaar    | Of u wilt weergeven van deze waarde in het dashboard. Sommige waarden worden gebruikt voor berekeningen, maar niet weergegeven.  |
| Maximum    | De maximumwaarde die een waarschuwing in het dashboard. |
| MaximumAlertActions | Een actie moet worden uitgevoerd in reactie op een waarschuwing. Bijvoorbeeld: een opdracht verzenden naar een station. |
| ConstValue | Een constante waarde die wordt gebruikt in een berekening. |

## <a name="deploy-the-changes"></a>De wijzigingen te implementeren

Wanneer u klaar bent met het aanbrengen van wijzigingen naar de **ContosoTopologyDescription.json** -bestand, moet u de oplossing verbonden factory opnieuw implementeren bij uw Azure-account.

De **azure-iot-verbonden-factory** opslagplaats bevat een **build.ps1** PowerShell-script die u gebruiken kunt om te bouwen en implementeren van de oplossing.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de verbonden factory vooraf geconfigureerde oplossing door te lezen van de volgende artikelen:

* [Overzicht van de vooraf geconfigureerde oplossing voor verbonden factory's][lnk-rm-walkthrough]
* [Implementeer een gateway voor verbonden factory][lnk-connect-cf]
* [Machtigingen op de site azureiotsuite.com][lnk-permissions]
* [Veelgestelde vragen over verbonden factory's](iot-suite-faq-cf.md)
* [FAQ][lnk-faq]


[img-oee-kpi]: ./media/iot-suite-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-suite-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-suite-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-suite-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-suite-connected-factory-customize/published.png
[img-munich]: ./media/iot-suite-connected-factory-customize/munich.png
[img-server-uris]: ./media/iot-suite-connected-factory-customize/serveruris.png
[lnk-kpi]: ./media/iot-suite-connected-factory-customize/kpidisplay.png

[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md