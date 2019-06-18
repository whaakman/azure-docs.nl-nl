---
title: Gegevens visualiseren in de Verkenner van Azure Time Series Insights Preview | Microsoft Docs
description: In dit artikel worden de functies en opties die beschikbaar zijn in de web-app van Azure Time Series Insights Preview explorer beschreven.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 05/03/2019
ms.custom: seodec18
ms.openlocfilehash: c4f3053063ce33d2777387da2c53effd61b05f1a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66399859"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Gegevens visualiseren in de Preview-versie van de Verkenner

Dit document beschrijft de gebruikersinterface en functies voor de gebruikerservaring en interface van de Azure Time Series Insights Preview [demo-web-app](https://insights.timeseries.azure.com/preview/demo). Specifiek, wordt de indeling van de gehoste voorbeeld, opties voor het aanpassen van interface en navigatie door de opgegeven demo beschreven.

## <a name="prerequisites"></a>Vereisten

Als u wilt beginnen met de Verkenner van Azure Time Series Insights Preview, moet u:

* Een Time Series Insights-omgeving instellen hebt. Voor meer informatie over het inrichten van een exemplaar, probeert de [Azure Time Series Insights Preview](./time-series-insights-update-create-environment.md) zelfstudie.
* [Gegevenstoegang bieden](./time-series-insights-data-access.md) aan de Time Series Insights-omgeving die u hebt gemaakt voor het account. U kunt toegang tot andere ook over uzelf opgeven.
* Een gebeurtenisbron toevoegen aan de Time Series Insights-omgeving om gegevens te pushen naar de omgeving:
  * Informatie over [verbinding maken met een event hub](./time-series-insights-how-to-add-an-event-source-eventhub.md).
  * Informatie over [verbinding maken met een IoT-hub](./time-series-insights-how-to-add-an-event-source-iothub.md).

## <a name="learn-about-the-preview-explorer"></a>Meer informatie over de Preview-explorer

De Verkenner van Azure Time Series Insights Preview bestaat uit de volgende elementen:

[![Het Verkenner-weergave](media/v2-update-explorer/explorer-one.png)](media/v2-update-explorer/explorer-one.png#lightbox)

- <a href="#environment-drop-down-list">Omgeving deelvenster</a>: Uw Azure Time Series Insights-omgevingen weergegeven.
- <a href="#navigation-menu">Navigatiemenu</a>: Gebruiken om te schakelen tussen de **analyseren** en **Model** pagina's.
- <a href="#hierarchy-tree">Hiërarchiestructuur</a>: Gebruik deze om te selecteren van bepaalde elementen in model en de gegevens weer te geven.
- <a href="#preview-well">Time series en</a>: Geeft de momenteel geselecteerde gegevenselementen in de tabelindeling met kleurcodering.
- <a href="#preview-chart">Grafiek deelvenster</a>: Uw huidige werkmap-grafiek weergegeven.
- <a href="#time-editor-panel">Tijdlijn</a>: Gebruik dit voor het wijzigen van de tijdspanne werken.
- <a href="#navigation-panel">App-balk</a>: De gebruikersbeheeropties, zoals de huidige tenant bevat. U kunt het thema-en taalinstellingen wijzigen.

## <a name="environment-drop-down-list"></a>De vervolgkeuzelijst omgeving

De vervolgkeuzelijst omgeving wordt weergegeven van alle Time Series Insights-omgevingen die u toegang tot hebt. De lijst bevat betalen per gebruik omgevingen, zoals de Preview van Time Series Insights. De lijst bevat ook een S1/S2-omgevingen, die algemeen beschikbaar zijn.

1. Selecteer de pijl omlaag naast uw omgeving weergegeven.

   [![Het Configuratiescherm](media/v2-update-explorer/explorer-two.png)](media/v2-update-explorer/explorer-two.png#lightbox)

1. Selecteer vervolgens de gewenste omgeving.

## <a name="navigation-menu"></a>Navigatiemenu

  [![Het navigatiemenu](media/v2-update-explorer/explorer-three.png)](media/v2-update-explorer/explorer-three.png#lightbox)

Gebruik het navigatiemenu om te kiezen tussen twee weergaven:

* **Analyseren**: Gebruik dit voor de grafiek en gedetailleerde analyses uitvoeren op de gemodelleerde of unmodeled time series-gegevens.
* **Model**: Gebruik dit voor push nieuwe Time Series Insights Preview typen, hiërarchieën en exemplaren aan uw Time Series Insights-model.

## <a name="hierarchy-tree"></a>Hiërarchiestructuur

De hiërarchische structuur worden geselecteerde gegevenselementen met modellen, specifieke apparaten en sensoren op uw apparaten weergegeven.

### <a name="model-search-panel"></a>Paneel voor zoeken van model

Het deelvenster model zoeken kunt u eenvoudig zoeken naar en navigeer naar uw hiërarchie Time Series-Model om te vinden van de specifieke tijd series-instanties die u wilt weergeven in de grafiek. Nadat u uw instanties selecteert, worden ze ook op zowel de huidige grafiek en de gegevens toegevoegd.

  [![Het deelvenster model zoeken](media/v2-update-explorer/explorer-four.png)](media/v2-update-explorer/explorer-four.png#lightbox)

### <a name="model-authoring"></a>Ontwerpen van het model

De Azure Time Series Insights Preview ondersteunt volledige maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen op uw Time Series-Model.

* **Time Series-Model type**: U kunt typen van Time Series Insights gebruiken om variabelen of formules voor het uitvoeren van berekeningen te definiëren. Ze zijn gekoppeld aan een bepaalde Time Series Insights-exemplaar. Een type kan een of meer variabelen hebben.
* **Time Series-Model hiërarchie**: Hiërarchieën zijn systematische organisaties van uw gegevens. Hiërarchieën weer de relaties tussen verschillende entiteiten in uw Time Series Insights-gegevens.
* **Time Series-Model exemplaar**: -Exemplaren zijn de tijdreeksen zelf. In de meeste gevallen zijn de **DeviceID** of **AssetID**, dit is de unieke id van de activa in de omgeving.

Zie voor meer informatie over de Time Series-Model, [tijden reeks modellen](./time-series-insights-update-tsm.md).

## <a name="preview-well"></a>Preview-versie en

De bron weergeven-instantievelden en andere metagegevens die zijn gekoppeld aan de geselecteerde exemplaren van de Time Series Insights Als u de selectievakjes in aan de rechterkant selecteert, kunt u specifieke exemplaren van de huidige grafiek weergeven of verbergen. U kunt ook specifieke gegevenselementen in uw huidige gegevens verwijderen door het selecteren van de rode **verwijderen** (Prullenbak) besturingselement aan de linkerkant van het element.

  [![De Preview-versie en](media/v2-update-explorer/explorer-five.png)](media/v2-update-explorer/explorer-five.png#lightbox)

Opnieuw configureren van de indeling van uw **analyseren** grafiek pagina, selecteer het pictogram weglatingstekens in de rechterbovenhoek:

  [![Opties voor telemetrie-indeling](media/v2-update-explorer/explorer-six.png)](media/v2-update-explorer/explorer-six.png#lightbox)

> [!NOTE]
> Als u het volgende bericht ziet, geen exemplaar van de gegevens tijdens de periode die is geselecteerd. Los het probleem, de tijdspanne vergroten of bevestigen dat het exemplaar van gegevens pushen.
>
> ![Geen melding gegevens](media/v2-update-explorer/explorer-seven.png)

## <a name="preview-chart"></a>Voorbeeld van grafiek

U kunt de Time Series Insights-exemplaren als regels weergeven met het diagram. U kunt het deelvenster van de omgeving, gegevensmodel en tijd van span het Configuratiescherm samenvouwen door te selecteren van de webbesturingselementen op de grafiek groter maken.

  [![Overzicht van de Preview-grafiek](media/v2-update-explorer/explorer-eight.png)](media/v2-update-explorer/explorer-eight.png#lightbox)

- **Geselecteerde datumbereik**: Hiermee wordt bepaald welke elementen beschikbaar voor visualisatie zijn.

- **Binnenste datum bereik schuifregelaar**: De twee endpoint-besturingselementen door ze te slepen gedurende de periode die u wilt gebruiken.

- **Tijd van span samenvouwen besturingselement**: Samengevouwen en breidt de tijd span Configuratiescherm-editor.

- **Besturingselement voor y-as opmaken**: Bladeren door de opties voor de y-as beschikbaar:

    * `Default`: Elke regel is een afzonderlijke y-as.
    * `Stacked`: Gebruik dit voor meerdere regels op de dezelfde y-as-stack met de y-as gegevens wijzigen op basis van de geselecteerde regel.
    * `Shared`: Alle y-as-gegevens samen weergegeven.

- **Huidige gegevenselement**: Het geselecteerde element en de bijbehorende gegevens.

Om in te zoomen verder in een specifieke gegevenssegment, met de linkermuisknop op een gegevenspunt in de huidige grafiek en sleep het geselecteerde gebied naar het eindpunt van uw keuze. Met de rechtermuisknop op het grijze gebied van de geselecteerde en selecteer **zoomen**, zoals in dit volgende afbeelding:

  [![Inzoomen op Preview-grafiek](media/v2-update-explorer/explorer-nine.png)](media/v2-update-explorer/explorer-nine.png#lightbox)

Na het uitvoeren van de **zoomen** actie, ziet u de geselecteerde gegevens instellen. Selecteer het besturingselement van de y-as-indeling doorloopt u de drie weergaven van de y-as van uw Time Series Insights-gegevens.

  [![Preview-versie grafiek y-as](media/v2-update-explorer/explorer-ten.png)](media/v2-update-explorer/explorer-ten.png#lightbox)

Hier ziet u een voorbeeld van gedeelde Y-assen:

  [![Preview-versie gedeeld Y-assen](media/v2-update-explorer/explorer-eleven.png)](media/v2-update-explorer/explorer-eleven.png#lightbox)

## <a name="time-editor-panel"></a>Deelvenster van editor tijd

Als u met Time Series Insights Preview werkt, selecteert u eerst een tijdsduur. De gegevensset die beschikbaar is voor het omgaan met de Time Series Insights Preview widgets Hiermee bepaalt u de geselecteerde periode. De volgende webbesturingselementen zijn beschikbaar in Time Series Insights Preview voor het selecteren van de tijdspanne werken:

  [![Deelvenster tijd selecteren](media/v2-update-explorer/explorer-twelve.png)](media/v2-update-explorer/explorer-twelve.png#lightbox)

1. **Binnenste datum bereik schuifregelaar**: De twee endpoint-besturingselementen door ze te slepen gedurende de periode die u wilt gebruiken. Deze interne datumbereik wordt beperkt door de schuifregelaar datum buitenste bereik.

1. **Vergroot of verklein de knoppen voor datumbereik**: Vergroten of verkleinen van de tijdspanne door een van de knoppen voor het interval dat u wilt selecteren.

1. **Tijd van span samenvouwen besturingselement**: Dit webbesturingselement kunt u de besturingselementen, met uitzondering van de schuifregelaar van binnenste datum bereik verbergen.

1. **Buitenste bereik schuifregelaar datumbesturingselement**: Gebruik de eindpunt-besturingselementen om te selecteren van de buitenste datumbereik, die beschikbaar zijn voor uw interne datumbesturingselement bereik.

1. **Snelle tijden datumbereik vervolgkeuzelijst**: Gebruiken om te snel schakelen tussen span selecties vooraf ingestelde tijd, zoals de laatste **30 minuten**, wordt de **afgelopen 12 uur**, of een **aangepast datumbereik**. Als u deze waarde wijzigt, wijzigt ook de beschikbare interval bereiken die worden beschreven in de schuifregelaar voor grootte-interval.

1. **Intervalgrootte schuifregelaar**: Gebruik deze om in te zoomen en afmelden bij intervallen via de dezelfde periode. Deze actie biedt meer controle over verkeer tussen grote segmenten van de tijd. Vloeiend trends op segmenten zo klein is als een milliseconde wordt weergegeven. U kunt deze gebruiken om te zien met hoge resolutie, gedetailleerde delen van uw gegevens. Startpunt van de schuifregelaar-standaard is ingesteld als de meest optimale weergave van de gegevens van uw selectie, waardoor resolutie, de snelheid van de query en de granulatie.

1. **Besturingselement voor bereik naar en van web**: Met dit webbesturingselement, kunt u eenvoudig de datum en tijd bereiken die u wilt selecteren. U kunt ook het besturingselement gebruiken om over te schakelen tussen de verschillende tijdzones. Nadat u de wijzigingen toe te passen op de huidige werkruimte, selecteert u **opslaan**.

   [![Naar en van deelvenster selecteren](media/v2-update-explorer/explorer-thirteen.png)](media/v2-update-explorer/explorer-thirteen.png#lightbox)

## <a name="navigation-panel"></a>Navigatiepaneel

Het navigatiepaneel Time Series Insights Preview-versie wordt weergegeven aan de bovenkant van uw Time Series Insights-app. De volgende functies biedt.

### <a name="current-session-share-link-control"></a>Huidige bestandsshare koppeling sessiebeheer

  [![Pictogram voor delen](media/v2-update-explorer/explorer-fifteen.png)](media/v2-update-explorer/explorer-fifteen.png#lightbox)

Selecteer de nieuwe **delen** pictogram voor het delen van een URL-koppeling met uw team.

  [![De URL van uw instantie delen](media/v2-update-explorer/url-share.png)](media/v2-update-explorer/url-share.png#lightbox)

### <a name="tenant-section"></a>Sectie tenant

  [![Selectie van de tenant](media/v2-update-explorer/explorer-sixteen.png)](media/v2-update-explorer/explorer-sixteen.png#lightbox)

* Uw huidige Time Series Insights aanmeldingsaccount informatie weergegeven.
* Gebruik dit om over te schakelen tussen de beschikbare Time Series Insights-thema's.
* Gebruiken om te bekijken van de Preview-versie [demo-web-app](https://insights.timeseries.azure.com/preview/demo).

### <a name="theme-selection"></a>Thema selecteren

Als u wilt een nieuw thema selecteren, selecteert u het pictogram van uw profiel zich in de rechterbovenhoek. Selecteer **thema wijzigen**.

  [![Thema selecteren](media/v2-update-explorer/theme-selection.png)](media/v2-update-explorer/theme-selection.png#lightbox)

> [!TIP]
> Taal selecteren is ook beschikbaar door uw profiel-pictogram te selecteren.

Azure Time Series Insights Preview ondersteunt twee thema's:

* **Licht thema**: Het standaardthema dat wordt weergegeven in dit document.
* **Donker thema**: Hiermee maakt u de explorer, zoals hier wordt weergegeven:

  [![Geselecteerde donker thema](media/v2-update-explorer/explorer-seventeen.png)](media/v2-update-explorer/explorer-seventeen.png#lightbox)

## <a name="s1s2-environment-controls"></a>Besturingselementen voor S1/S2-omgeving

### <a name="preview-terms-panel"></a>Paneel voor Preview-voorwaarden

In deze sectie geldt alleen voor bestaande S1/S2-omgevingen wordt geprobeerd de explorer gebruiken in de bijgewerkte gebruikersinterface. Mogelijk wilt u het product algemeen beschikbaar is en de Preview-versie gebruiken in combinatie. Er is bepaalde functionaliteit van de bestaande gebruikersinterface aan de bijgewerkte explorer toegevoegd, maar krijgt u de volledige gebruikersinterface-ervaring voor S1/S2-omgeving in de bestaande Time Series Insights-Verkenner. 

In plaats van de hiërarchie ziet u het deelvenster Time Series Insights termen, waar u query's in uw omgeving definiëren. Gebruik deze om uw gegevens op basis van een predicaat te filteren.

  [![Deelvenster waar query](media/v2-update-explorer/explorer-eighteen.png)](media/v2-update-explorer/explorer-eighteen.png#lightbox)

Het deelvenster met Time Series Insights Preview-voorwaarden editor bevat de volgende parameters:

**Where**: Gebruik waar u de component snel uw om gebeurtenissen te filteren met behulp van de set met operanden die worden vermeld in de volgende tabel. Als u een zoekopdracht uitvoeren door een operand selecteren, wordt het predicaat automatisch bijgewerkt op basis van die zoekopdracht. Ondersteunde Operandtypen zijn onder andere:

| Bewerking | Ondersteunde typen   | Opmerkingen |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Double, DateTime, TimeSpan | |
| `=`, `!=`, `<>` | String, Bool, Double, DateTime, TimeSpan NULL |
| `IN` | String, Bool, Double, DateTime, TimeSpan NULL | Alle operands moeten zijn van hetzelfde type of NULL-constante. |
| `HAS` | String | Alleen constante letterlijke tekenreeksen zijn toegestaan aan de rechterkant. Lege tekenreeks en NULL zijn niet toegestaan. |

Zie voor meer informatie over ondersteunde querybewerkingen en gegevenstypen, [Time Series-expressie (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx).

### <a name="examples-of-where-clauses"></a>Voorbeelden van de locatie waar de EU

  [![Waar component-voorbeelden](media/v2-update-explorer/explorer-nineteen.png)](media/v2-update-explorer/explorer-nineteen.png#lightbox)

**Meting**: Een vervolgkeuzelijst die wordt weergegeven de numerieke kolommen (**verdubbelt**) u kunt gebruiken als elementen voor de huidige grafiek.

**Splitsen op basis van**: Deze vervolgkeuzelijst wordt weergegeven van alle beschikbare categorische kolommen (tekenreeksen) in uw model dat u kunt uw gegevens door te groeperen. U kunt maximaal vijf voorwaarden om weer te geven op de dezelfde x-as toevoegen. Geef de parameters die u wilt en selecteer vervolgens **toevoegen** om toe te voegen een nieuwe term.

  [![Aangevraagde en gefilterde weergave een](media/v2-update-explorer/explorer-twenty.png)](media/v2-update-explorer/explorer-twenty.png#lightbox)

U kunt weergeven en verbergen van elementen in het deelvenster grafiek door de zichtbaar pictogram te selecteren, zoals wordt weergegeven in de volgende afbeelding. Om volledig te verwijderen query's, selecteer de rode **X**.

  [![Aangevraagde en gefilterde weergave twee](media/v2-update-explorer/explorer-twenty-one.png)](media/v2-update-explorer/explorer-twenty-one.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [opslag- en uitgangsclaims](./time-series-insights-update-storage-ingress.md) in Azure Time Series Insights Preview.
- Lees het document Time Series Insights Preview op [gegevensmodellering](./time-series-insights-update-tsm.md).
- Informatie over [hoe u problemen vaststellen en oplossen](./time-series-insights-update-how-to-troubleshoot.md) uw Time Series Insights-exemplaar.
