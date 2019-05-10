---
title: Gegevens visualiseren in de Verkenner van Azure Time Series Insights Preview | Microsoft Docs
description: In dit artikel worden de functies en opties die beschikbaar zijn in de web-app van Azure Time Series Insights Preview explorer beschreven.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 05/03/2019
ms.custom: seodec18
ms.openlocfilehash: 862465a7611f1a2bc65dbb0c49c4de512bd239de
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65442102"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Gegevens visualiseren in de Preview-versie van de Verkenner

Dit document beschrijft de gebruikersinterface/UX-functies en de interface van de Azure Time Series Insights Preview [demo-web-app](https://insights.timeseries.azure.com/preview/demo). Specifiek, wordt de indeling van de gehoste voorbeeld, opties voor het aanpassen van interface en navigatie door de opgegeven demo beschreven.

## <a name="prerequisites"></a>Vereisten

Als u wilt beginnen met de Verkenner van Azure Time Series Insights Preview, moet u:

* Een Time Series Insights-omgeving instellen hebt. Voor meer informatie over het inrichten van een exemplaar Probeer onze [Azure Time Series Insights Preview](./time-series-insights-update-create-environment.md) zelfstudie.
* [Gegevenstoegang bieden](./time-series-insights-data-access.md) aan de Time Series Insights-omgeving die u hebt gemaakt voor het account. U kunt toegang tot andere ook over uzelf opgeven.
* Een gebeurtenisbron toevoegen aan de Time Series Insights-omgeving om gegevens te pushen naar de omgeving:
  * Informatie over [verbinding maken met een Event hub](./time-series-insights-how-to-add-an-event-source-eventhub.md)
  * Informatie over [verbinding maken met een IoT-hub](./time-series-insights-how-to-add-an-event-source-iothub.md)

## <a name="learn-about-the-preview-explorer"></a>Meer informatie over de Preview-explorer

De Verkenner van Azure Time Series Insights Preview bestaat uit de volgende elementen:

[![Het Verkenner-weergave](media/v2-update-explorer/explorer-one.png)](media/v2-update-explorer/explorer-one.png#lightbox)

1. <a href="#environment-dropdown">**Omgeving deelvenster**</a>: Uw Azure TSI-omgevingen weergegeven.
1. <a href="#navigation-menu">**Navigatiemenu**</a>: Hiermee kunt u tussen schakelen de **analyseren** en **Model** pagina's.
1. <a href="#hierarchy-tree">**Hiërarchiestructuur**</a>: Kunt u selecteren specifieke model en gegevenselementen weer te geven.
1. <a href="#preview-well">**Time series en**</a>: Geeft de momenteel geselecteerde gegevenselementen in de tabelindeling met kleurcodering.
1. <a href="#preview-chart">**Grafiek deelvenster**</a>:  Uw huidige werkmap-grafiek weergegeven.
1. <a href="#time-editor-panel">**Tijdlijn**</a>:  Kunt u de tijdsduur van uw werkende wijzigen.
1. <a href="#navigation-panel">**App-balk**</a>:  Bevat uw gebruikersbeheeropties, zoals de huidige tenant, en Hiermee kunt u wijzigen thema-en taalinstellingen.

## <a name="environment-dropdown"></a>Vervolgkeuzelijst voor omgeving

Vervolgkeuzelijst voor de omgeving wordt weergegeven voor alle van de Time Series Insights-omgevingen u toegang tot hebt. De lijst bevat betalen per gebruik-omgevingen (Preview) en S1/S2-omgevingen (algemene beschikbaarheid of algemene beschikbaarheid). 

1. Klik gewoon op de vervolgkeuzepijl naast uw omgeving weergegeven:

   [![Het Configuratiescherm](media/v2-update-explorer/explorer-two.png)](media/v2-update-explorer/explorer-two.png#lightbox)

1. Selecteer vervolgens de gewenste omgeving.

## <a name="navigation-menu"></a>Navigatiemenu

  [![Het navigatiemenu](media/v2-update-explorer/explorer-three.png)](media/v2-update-explorer/explorer-three.png#lightbox)

Het navigatiemenu kunt u kiezen tussen twee weergaven:

* **Analyseren**: Hiermee kunt u de grafiek en gedetailleerde analyses uitvoeren op de gemodelleerde of unmodeled time series-gegevens.
* **Model**: Hiermee kunt u een push nieuwe Time Series Insights Preview typen, hiërarchieën en exemplaren aan uw Time Series Insights-model.

## <a name="hierarchy-tree"></a>Hiërarchiestructuur

De hiërarchische structuur wordt weergegeven voor geselecteerde gegevenselementen, met inbegrip van modellen, specifieke apparaten en sensoren op uw apparaten.

### <a name="model-search-panel"></a>Paneel voor zoeken van model

Het deelvenster model zoeken kunt u eenvoudig zoeken en navigeer naar uw hiërarchie Time Series-Model om te vinden van de specifieke tijd series-instanties die u wilt weergeven in de grafiek. Wanneer u uw instanties selecteert, worden ze ook op zowel de huidige grafiek en de gegevens toegevoegd.

  [![Het deelvenster model zoeken](media/v2-update-explorer/explorer-four.png)](media/v2-update-explorer/explorer-four.png#lightbox)

### <a name="model-authoring"></a>Ontwerpen van het model

Azure Time Series Insights Preview biedt ondersteuning voor volledige maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen op uw Time Series-Model.  

* **Time Series-Model type**: Typen van Time Series Insights inschakelen definiëren variabelen of formules voor het uitvoeren van berekeningen. Ze zijn gekoppeld aan een bepaalde Time Series Insights-exemplaar. Een type kan een of meer variabelen hebben.
* **Time Series-Model hiërarchie**: Hiërarchieën zijn systematische organisaties van uw gegevens. Hiërarchieën weer de relaties tussen verschillende entiteiten in uw Time Series Insights-gegevens.
* **Time Series-Model exemplaar**: -Exemplaren zijn de tijdreeksen zelf. In de meeste gevallen zijn de **DeviceID** of **AssetID**, dit is de unieke id van de activa in de omgeving.

Zie voor meer informatie over de Time Series-Model, [tijden reeks modellen](./time-series-insights-update-tsm.md).

## <a name="preview-well"></a>Preview-versie en

De bron weergeven-instantievelden en andere metagegevens die zijn gekoppeld aan de geselecteerde TSI-exemplaren De selectievakjes in aan de rechterkant kunt u specifieke exemplaren van de huidige grafiek weergeven of verbergen. U kunt ook specifieke gegevenselementen verwijderen uit uw huidige gegevens door te klikken op de rode **verwijderen** (Prullenbak) besturingselement in de aan de linkerkant van het element.

  [![De Preview-versie en](media/v2-update-explorer/explorer-five.png)](media/v2-update-explorer/explorer-five.png#lightbox)

U kunt ook de configuratie van de indeling van uw **analyseren** grafiek pagina door het pictogram weglatingstekens selecteren in de rechterbovenhoek:

  [![Opties voor telemetrie-indeling](media/v2-update-explorer/explorer-six.png)](media/v2-update-explorer/explorer-six.png#lightbox)

> [!NOTE]
> Als u het volgende bericht ziet, heeft het exemplaar geen gegevens tijdens de periode die is geselecteerd. U kunt het probleem op te lossen de tijdsspanne verhogen of bevestigen dat het exemplaar van gegevens pushen.
>
> ![Geen melding gegevens](media/v2-update-explorer/explorer-seven.png)

## <a name="preview-chart"></a>Voorbeeld van grafiek

U kunt met het diagram TSI exemplaren weergeven als regels. U kunt het deelvenster van de omgeving, gegevensmodel en tijd van span het Configuratiescherm door te klikken op de webbesturingselementen groter maken van de grafiek samenvouwen.

  [![Overzicht van de Preview-grafiek](media/v2-update-explorer/explorer-eight.png)](media/v2-update-explorer/explorer-eight.png#lightbox)

1. **Geselecteerde datumbereik**: Hiermee wordt bepaald welke elementen beschikbaar voor visualisatie zijn.

1. **Binnenste datum bereik schuifregelaar hulpprogramma**: De twee endpoint-opties gebruiken door ze via de gewenste tijdspanne te slepen.

1. **Tijd van span samenvouwen besturingselement**: Samengevouwen en breidt de tijd span Configuratiescherm-editor.

1. **Besturingselement voor y-as opmaken**: Bladeren door de beschikbare opties voor de y-as:

    * `Default`: Elke regel is een afzonderlijke y-as.
    * `Stacked`: Hiermee kunt u meerdere regels op de dezelfde y-as-stack met de y-as gegevens wijzigen op basis van de geselecteerde regel.
    * `Shared`: Alle y-as-gegevens samen weergegeven.

1. **Huidige gegevenselement**: Het geselecteerde element en de bijbehorende gegevens.

U kunt verder inzoomen in een specifieke gegevenssegment door te klikken met de linkermuisknop een gegevenspunt in de huidige grafiek en sleept u het geselecteerde gebied naar het eindpunt van uw keuze. Met de rechtermuisknop op het gebied grijs weergegeven, gekozen en klik op **zoomen** zoals in dit volgende afbeelding:

  [![Inzoomen op Preview-grafiek](media/v2-update-explorer/explorer-nine.png)](media/v2-update-explorer/explorer-nine.png#lightbox)

Na het uitvoeren van de **zoomen** actie, ziet u de geselecteerde gegevensset. Klik op het besturingselement voor het opmaken van y-as om te bladeren naar de drie weergaven van de y-as van uw Time Series Insights-gegevens.

  [![Voorbeeld van grafiek y-as](media/v2-update-explorer/explorer-ten.png)](media/v2-update-explorer/explorer-ten.png#lightbox)

Hier ziet u een voorbeeld van gedeelde y-assen:

  [![Preview-versie gedeeld y-as](media/v2-update-explorer/explorer-eleven.png)](media/v2-update-explorer/explorer-eleven.png#lightbox)

## <a name="time-editor-panel"></a>Deelvenster van editor tijd

Als u met Time Series Insights Preview werkt, selecteert u eerst een tijdsduur. De gegevensset die beschikbaar is voor het omgaan met de Time Series Insights Preview widgets Hiermee bepaalt u de geselecteerde periode. De volgende webbesturingselementen zijn beschikbaar in Time Series Insights Preview voor het selecteren van de tijdspanne werken.

  [![Deelvenster tijd selecteren](media/v2-update-explorer/explorer-twelve.png)](media/v2-update-explorer/explorer-twelve.png#lightbox)

1. **Schuifregelaar voor interne datumbereik**: De twee endpoint-opties gebruiken door ze via de gewenste tijdspanne te slepen. Deze interne-datumbereik wordt beperkt door de schuifregelaar buitenste datumbereik.

1. **Vergroot of verklein de knoppen voor datumbereik**: Vergroten of verkleinen van de tijdspanne door een van de knoppen voor het interval dat u wilt selecteren.

1. **Tijd van span samenvouwen besturingselement**: Dit webbesturingselement kunt u de besturingselementen, met uitzondering van de schuifregelaar binnenste datumbereik verbergen.

1. **Besturingselement voor schuifregelaar outer datumbereik**: Gebruik de eindpunt-besturingselementen het buitenste datum bereik, die beschikbaar zijn voor het besturingselement inner datumbereik te selecteren.

1. **Snelle tijden datumbereik vervolgkeuzelijst**: Hiermee kunt u snel tussen span selecties vooraf ingestelde tijd, zoals de laatste schakelen **30 minuten**, wordt de **afgelopen 12 uur**, of een **aangepast datumbereik**. Als u deze waarde wijzigt, wijzigt ook de beschikbare interval bereiken die worden beschreven in de schuifregelaar voor grootte-interval.

1. **Intervalgrootte schuifregelaar**: Hiermee kunt u inzoomen in en uit de intervallen via de dezelfde periode. Deze actie biedt meer controle over verkeer tussen grote segmenten van de tijd. Vloeiend trends op segmenten zo klein is als een milliseconde, zodat u kunt om te zien met hoge resolutie, gedetailleerde delen van uw gegevens worden weergegeven. Startpunt van de schuifregelaar-standaard is ingesteld als de meest optimale weergave van de gegevens van uw selectie, waardoor resolutie, de snelheid van de query en de granulatie.

1. **Datumbereik naar en van het webbesturingselement voor**: Met dit webbesturingselement, kunt u eenvoudig op en selecteer de gewenste datum en tijd bereiken. U kunt ook het besturingselement gebruiken om over te schakelen tussen de verschillende tijdzones. Nadat de wijzigingen zijn aangebracht, toe te passen op de huidige werkruimte, selecteert u **opslaan**.

   [![Naar en van deelvenster selecteren](media/v2-update-explorer/explorer-thirteen.png)](media/v2-update-explorer/explorer-thirteen.png#lightbox)

## <a name="navigation-panel"></a>Navigatiepaneel

Het navigatiepaneel Time Series Insights Preview-versie wordt weergegeven aan de bovenkant van uw app in TSI. De volgende functies biedt.

### <a name="current-session-share-link-control"></a>Huidige bestandsshare koppeling sessiebeheer

  [![Pictogram voor delen](media/v2-update-explorer/explorer-fifteen.png)](media/v2-update-explorer/explorer-fifteen.png#lightbox)

Selecteer de nieuwe **delen** pictogram voor het delen van een URL-koppeling met uw team.

  [![De URL van uw instantie delen](media/v2-update-explorer/url-share.png)](media/v2-update-explorer/url-share.png#lightbox)

### <a name="tenant-section"></a>Sectie tenant

  [![Selectie van de tenant](media/v2-update-explorer/explorer-sixteen.png)](media/v2-update-explorer/explorer-sixteen.png#lightbox)

* Geeft de huidige Time Series Insights-account aanmeldingsgegevens.
* Hiermee kunt u schakelen tussen de beschikbare Time Series Insights-thema's.
* Hiermee kunt u weergeven van de Preview-versie [demo-web-app](https://insights.timeseries.azure.com/preview/demo).

### <a name="theme-selection"></a>Thema selecteren

Als u wilt een nieuw thema selecteren, klik op het pictogram van uw profiel zich in de rechterbovenhoek. Selecteer **thema wijzigen**.

  [![Thema selecteren](media/v2-update-explorer/theme-selection.png)](media/v2-update-explorer/theme-selection.png#lightbox)

> [!TIP]
> Taal selecteren is ook beschikbaar door te klikken op het pictogram van uw profiel.

Azure Time Series Insights Preview ondersteunt twee thema's:

* **Licht thema**: Het standaardthema dat wordt weergegeven in dit document.
* **Donker thema**:  Hiermee maakt u de explorer, zoals hier wordt weergegeven:

  [![Geselecteerde donker thema](media/v2-update-explorer/explorer-seventeen.png)](media/v2-update-explorer/explorer-seventeen.png#lightbox)

## <a name="s1s2-environment-controls"></a>Besturingselementen voor S1/S2-omgeving

### <a name="preview-terms-panel"></a>Paneel voor Preview-voorwaarden

In deze sectie geldt alleen voor bestaande S1/S2-omgevingen wordt geprobeerd de explorer gebruiken in de bijgewerkte gebruikersinterface. Het is raadzaam het product van de algemene beschikbaarheid en de Preview-versie gebruiken in combinatie. Er is bepaalde functionaliteit van de bestaande gebruikersinterface aan de bijgewerkte explorer toegevoegd, maar krijgt u de volledige gebruikersinterface-ervaring voor S1/S2-omgeving in de bestaande Time Series Insights-Verkenner.  

In plaats van de hiërarchie ziet u het deelvenster Time Series Insights termen, waar u query's in uw omgeving definiëren. Hiermee kunt u uw gegevens op basis van een predicaat filteren.

  [![Deelvenster waar query](media/v2-update-explorer/explorer-eighteen.png)](media/v2-update-explorer/explorer-eighteen.png#lightbox)

Het deelvenster met Time Series Insights Preview-voorwaarden editor bevat de volgende parameters:

**Where**: De where component kunt u snel filter de gebeurtenissen met behulp van de set met operanden die worden vermeld in de volgende tabel. Als u een zoekopdracht uitvoeren door een operand selecteren, wordt het predicaat automatisch bijgewerkt op basis van die zoekopdracht. Ondersteunde Operandtypen zijn onder andere:

| Bewerking | Ondersteunde typen   | Opmerkingen |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Double, DateTime, TimeSpan | |
| `=`, `!=`, `<>` | String, Bool, Double, DateTime, TimeSpan NULL |
| `IN` | String, Bool, Double, DateTime, TimeSpan NULL | Alle operands moeten zijn van hetzelfde type of NULL-constante. |
| `HAS` | Reeks | Alleen constante letterlijke tekenreeksen zijn toegestaan op de rechterkant. Lege tekenreeks en NULL zijn niet toegestaan. |

Meer informatie over ondersteunde query operations en gegevenstypen vindt [Time Series-expressie (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx).

### <a name="examples-of-where-clauses"></a>Voorbeelden van de locatie waar de EU

  [![Waar component-voorbeelden](media/v2-update-explorer/explorer-nineteen.png)](media/v2-update-explorer/explorer-nineteen.png#lightbox)

**Meting**: Een vervolgkeuzelijst die wordt weergegeven de numerieke kolommen (**verdubbelt**) u kunt gebruiken als elementen voor uw huidige grafiek.

**Splitsen op basis van**: Deze vervolgkeuzelijst wordt weergegeven van alle beschikbare categorische kolommen (tekenreeksen) in uw model dat u kunt uw gegevens door te groeperen. U kunt maximaal vijf voorwaarden om weer te geven op de dezelfde x-as toevoegen. Voer de gewenste parameters en selecteer vervolgens **toevoegen** om toe te voegen een nieuwe term.

  [![Aangevraagde en gefilterde weergave een](media/v2-update-explorer/explorer-twenty.png)](media/v2-update-explorer/explorer-twenty.png#lightbox)

U kunt weergeven en verbergen van elementen in het deelvenster van de grafiek het zichtbaar door pictogram te selecteren zoals in de volgende afbeelding. U kunt query's volledig verwijderen door te klikken op de rode **X**.

  [![Aangevraagde en gefilterde weergave twee](media/v2-update-explorer/explorer-twenty-one.png)](media/v2-update-explorer/explorer-twenty-one.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [opslag- en uitgangsclaims](./time-series-insights-update-storage-ingress.md) in Azure Time Series Insights Preview.

- Lees het document Time Series Insights Preview op [gegevensmodellering](./time-series-insights-update-tsm.md).

- Informatie over [hoe u problemen vaststellen en oplossen](./time-series-insights-update-how-to-troubleshoot.md) uw Time Series Insights-exemplaar.
