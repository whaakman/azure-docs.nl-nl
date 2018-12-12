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
ms.date: 12/03/2018
ms.openlocfilehash: 3a2958d838f0646ac030421b3785fc11a1466dd6
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53086722"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Gegevens visualiseren in de Preview-versie van de Verkenner

Dit artikel beschrijft functies en opties die beschikbaar zijn in Azure Time Series Insights Preview [explorer web-app](https://insights.timeseries.azure.com/preview/samples).

## <a name="prerequisites"></a>Vereisten

Voordat u de Verkenner van Azure Time Series Insights Preview gebruiken, moet u:

* Een Time Series Insights-omgeving instellen hebt. Zie voor meer informatie, [zelfstudie: Azure Time Series Insights Preview](./time-series-insights-update-create-environment.md).
* Gegevenstoegang verlenen tot de Time Series Insights-omgeving die u hebt gemaakt voor het account. U kunt toegang tot andere ook over uzelf opgeven.
* Een gebeurtenisbron toevoegen aan de Time Series Insights-omgeving om gegevens te pushen naar de omgeving.

## <a name="learn-about-the-azure-time-series-insights-preview-explorer"></a>Meer informatie over de Azure Time Series Insights Preview-explorer

  ![Explorer-een][1]

De Verkenner van Azure Time Series Insights Preview bestaat uit de volgende elementen:

* **Navigatiebalk**: Hiermee kunt u tussen pagina's voor analyse en het model schakelen.
* **Hiërarchiestructuur**: u kunt aangeven specifieke gegevenselementen weer te geven.
* **Time series en**: geeft de momenteel geselecteerde gegevenselementen weer.
* **Grafiek deelvenster**: Hiermee wordt de grafiek van uw huidige werken.
* **Tijdlijn**: kunt u de tijdsduur van uw werkende wijzigen.
* **App-balk**: bevat uw gebruikersbeheeropties, zoals de huidige tenant, en Hiermee kunt u wijzigen thema-en taalinstellingen.

## <a name="time-series-insights-preview-environment-panel"></a>Time Series Insights-Preview-omgeving deelvenster

De omgeving-paneel geeft alle Time Series Insights-omgevingen die u toegang tot hebt weer. De lijst bevat betalen per gebruik omgevingen Preview en S1/S2-omgevingen (GA). Klik op de Time Series Insights-omgeving die u wilt gebruiken.

  ![Verkenner-twee][2]

## <a name="time-series-insights-preview-navigation-menu"></a>Navigatiemenu van Time Series Insights-Preview

  ![Verkenner-drie][3]

U kunt met het navigatiemenu schakelen tussen de Time Series Insights-apps:

* **Analyseren**: Hiermee kunt u de grafiek en gedetailleerde analyses uitvoeren op de gemodelleerde of unmodeled time series-gegevens.

* **Model**: Hiermee kunt u een push nieuwe Time Series Insights Preview typen, hiërarchieën en exemplaren aan uw Time Series Insights-model.

## <a name="time-series-insights-preview-model-authoring"></a>Time Series Insights Preview model ontwerpen

U kunt met deze app kunt maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen op uw Time Series-Model uitvoeren.  

* **Time Series-Model type**: Time Series Insights typen kunt definiëren variabelen of formules voor het uitvoeren van berekeningen. Ze zijn gekoppeld aan een bepaalde Time Series Insights-exemplaar. Een type kan een of meer variabelen hebben.
* **Time Series-Model hiërarchie**: hiërarchieën zijn systematische organisaties van uw gegevens. Hiërarchieën weer de relaties tussen verschillende entiteiten in uw Time Series Insights-gegevens.
* **Time Series-Model exemplaar**:-exemplaren zijn de tijdreeksen zelf. In de meeste gevallen zijn ze de apparaat-id of AssetID, dit de unieke id van de activa in de omgeving is.

Zie voor meer informatie over de Time Series-Model, [tijden reeks modellen](./time-series-insights-update-tsm.md).

## <a name="time-series-insights-preview-model-search-panel"></a>Paneel voor Time Series Insights Preview model zoeken

Het deelvenster model zoeken kunt u eenvoudig zoeken en navigeer naar uw hiërarchie Time Series-Model om te vinden van de specifieke tijd series-instanties die u wilt weergeven in de grafiek. Wanneer u uw instanties selecteert, worden ze ook op zowel de huidige grafiek en de gegevens toegevoegd.

  ![Verkenner-vier][4]

## <a name="time-series-insights-preview-well"></a>Time Series Insights Preview goed

De bron worden exemplaarvelden en andere metagegevens die zijn gekoppeld aan de geselecteerde time series-instanties weergegeven. De selectievakjes in aan de rechterkant kunt u specifieke exemplaren van de huidige grafiek weergeven of verbergen. U kunt ook specifieke gegevenselementen verwijderen uit uw huidige gegevens door te klikken op de rode x-besturingselement aan de rechterkant van het element.

  ![Verkenner-vijf][5]

U kunt ook pop-out het paneel telemetrie voor een beter verticale weergave van de elementen in uw gegevens goed.

  ![Verkenner-6][6]

> [!NOTE]
> Als u het volgende bericht ziet, heeft het exemplaar geen gegevens tijdens de periode die is geselecteerd. U kunt het probleem op te lossen de tijdsspanne verhogen of bevestigen dat het exemplaar van gegevens pushen.
>
> ![Explorer 7][7]

## <a name="time-series-insights-preview-chart"></a>Time Series Insights voorbeeldgrafiek

U kunt de time series-instanties als regels weergeven met de grafiek. U kunt het deelvenster van de omgeving, gegevensmodel en tijd van span het Configuratiescherm door te klikken op de webbesturingselementen groter maken van de grafiek samenvouwen.

  ![Verkenner-8][8]

1. **Geselecteerde datumbereik**: besturingselementen welke elementen beschikbaar voor visualisatie zijn.

1. **Binnenste datum bereik schuifregelaar hulpprogramma**: gebruik van de twee besturingselementen voor het eindpunt door ze via de gewenste tijdspanne te slepen.

1. **Tijd van span samenvouwen besturingselement**: samengevouwen en breidt de tijd span Configuratiescherm-editor.

1. **Besturingselement voor y-as opmaken**: bladeren door de beschikbare opties voor de y-as:

    * `Default`: Elke regel is een afzonderlijke y-as.
    * `Stacked`: Hiermee kunt u meerdere regels op de dezelfde y-as-stack met de y-as gegevens wijzigen op basis van de geselecteerde regel.
    * `Shared`: Alle y-as-gegevens samen weergegeven.

1. **Huidige gegevenselement**: het geselecteerde element en de bijbehorende gegevens.

U kunt verder inzoomen in een specifieke gegevenssegment door te klikken met de linkermuisknop een gegevenspunt in de huidige grafiek en sleept u het geselecteerde gebied naar het eindpunt van uw keuze. Met de rechtermuisknop op het gebied grijs weergegeven, geselecteerde en inzoomen op zoals weergegeven in dit volgende afbeelding:

  ![Explorer 9][9]

Nadat u de actie zoomen uitvoert, ziet u de geselecteerde gegevensset. Klik op het besturingselement voor het opmaken van y-as om te bladeren naar de drie weergaven van de y-as van uw Time Series Insights-gegevens.

  ![Verkenner-tien][10]

Hier ziet u een voorbeeld van gedeelde y-assen:

  ![Verkenner-11][11]

## <a name="time-series-insights-preview-time-editor-panel"></a>Time Series Insights Preview tijd editor Configuratiescherm

Als u met Time Series Insights Preview werkt, selecteert u eerst een tijdsduur. De gegevensset die beschikbaar is voor het omgaan met de Time Series Insights Preview widgets Hiermee bepaalt u de geselecteerde periode. De volgende webbesturingselementen zijn beschikbaar in Time Series Insights Preview voor het selecteren van de tijdspanne werken.

  ![Verkenner-twaalf][12]

1. **Schuifregelaar voor interne datumbereik**: gebruik van de twee besturingselementen voor het eindpunt door ze via de gewenste tijdspanne te slepen. Deze interne-datumbereik wordt beperkt door de schuifregelaar buitenste datumbereik.

1. **Vergroot of verklein de knoppen voor datumbereik**: toename of afname uw tijd omspannen door een van de knoppen voor het interval dat u wilt selecteren.

1. **Tijd van span samenvouwen besturingselement**: dit webbesturingselement kunt u de besturingselementen, met uitzondering van de schuifregelaar binnenste datumbereik verbergen.

1. **Besturingselement voor schuifregelaar outer datumbereik**: Gebruik de eindpunt-besturingselementen het buitenste datum bereik, die beschikbaar zijn voor het besturingselement inner datumbereik te selecteren.

1. **Snelle tijden datum vervolgkeuzelijst voor bereik**: Hiermee kunt u snel tussen span selecties vooraf ingestelde tijd, zoals de laatste 30 minuten, de afgelopen 12 uur of een aangepast bereik schakelen. Als u deze waarde wijzigt, wijzigt ook de beschikbare interval bereiken die worden beschreven in de schuifregelaar voor grootte-interval.

1. **Intervalgrootte schuifregelaar**: Hiermee kunt u in en uit de intervallen via dezelfde inzoomen periode. Deze actie biedt meer controle over verkeer tussen grote segmenten van de tijd. Vloeiend trends op segmenten zo klein is als een milliseconde, zodat u kunt om te zien met hoge resolutie, gedetailleerde delen van uw gegevens worden weergegeven. Startpunt van de schuifregelaar-standaard is ingesteld als de meest optimale weergave van de gegevens van uw selectie, waardoor resolutie, de snelheid van de query en de granulatie.

1. **Datumbereik naar en van het webbesturingselement voor**: met dit webbesturingselement kunt u eenvoudig op en selecteer de gewenste datum en tijd bereiken. U kunt ook het besturingselement gebruiken om over te schakelen tussen de verschillende tijdzones. Nadat de wijzigingen zijn aangebracht, toe te passen op de huidige werkruimte, selecteert u **opslaan**.

  ![Verkenner-13][13]

## <a name="time-series-insights-preview-navigation-panel"></a>Navigatiepaneel van Time Series Insights-Preview

Het navigatiepaneel Time Series Insights Preview biedt de volgende functionaliteit:

  ![Verkenner-14][14]

### <a name="current-session-share-link-control"></a>Huidige bestandsshare koppeling sessiebeheer

  ![Verkenner-vijftien][15]

Selecteer de koppeling webbesturingselement voor (gemarkeerd) voor het genereren van een URL als u wilt opslaan of delen van uw huidige Azure Time Series Insights werksessie, waaronder:

* Momenteel geselecteerde tijdsbereik
* Intervalgrootte van geselecteerde
* Geselecteerde gegevens goed

### <a name="tenant-section"></a>Sectie tenant

  ![Verkenner-zestien][16]

* Geeft de huidige Time Series Insights-account aanmeldingsgegevens.
* Hiermee kunt u schakelen tussen de beschikbare Time Series Insights-thema's.

### <a name="theme-selection"></a>Thema selecteren

Azure Time Series Insights Preview ondersteunt twee thema's:

* **Licht thema**: het standaardthema dat wordt weergegeven in dit document.
* **Donker thema**: de explorer wordt weergegeven zoals hier wordt weergegeven:

  ![Verkenner-17][17]

U kunt hier ook wijzigen tussen ondersteunde talen.

## <a name="s1s2-environment-controls"></a>Besturingselementen voor S1/S2-omgeving

### <a name="time-series-insights-preview-terms-panel"></a>Deelvenster termen van Time Series Insights-Preview

In deze sectie geldt alleen voor bestaande S1/S2-omgevingen wordt geprobeerd de explorer gebruiken in de bijgewerkte gebruikersinterface. Het is raadzaam het product van de algemene beschikbaarheid en de Preview-versie gebruiken in combinatie. Er is bepaalde functionaliteit van de bestaande gebruikersinterface aan de bijgewerkte explorer toegevoegd, maar krijgt u de volledige gebruikersinterface-ervaring voor S1/S2-omgeving in de bestaande Time Series Insights-Verkenner.  

In plaats van de hiërarchie ziet u het deelvenster Time Series Insights termen, waar u query's in uw omgeving definiëren. Hiermee kunt u uw gegevens op basis van een predicaat filteren.

  ![Verkenner-18][18]

Het deelvenster met Time Series Insights Preview-voorwaarden editor bevat de volgende parameters:

**Waar**: waar u de component kunt u snel filter de gebeurtenissen met behulp van de set met operanden die worden vermeld in de volgende tabel. Als u een zoekopdracht uitvoeren door een operand selecteren, wordt het predicaat automatisch bijgewerkt op basis van die zoekopdracht. Ondersteunde Operandtypen zijn onder andere:

| Bewerking | Ondersteunde typen   | Opmerkingen |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Double, DateTime, TimeSpan | |
| `=`, `!=`, `<>` | String, Bool, Double, DateTime, TimeSpan NULL |
| `IN` | String, Bool, Double, DateTime, TimeSpan NULL | Alle operands moeten zijn van hetzelfde type of NULL-constante. |
| `HAS` | Reeks | Alleen constante letterlijke tekenreeksen zijn toegestaan op de rechterkant. Lege tekenreeks en NULL zijn niet toegestaan. |

Meer informatie over ondersteunde query operations en gegevenstypen vindt [Time Series-expressie (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx).

### <a name="examples-of-where-clauses"></a>Voorbeelden van de locatie waar de EU

  ![Verkenner-19][19]

**Meting**: deze vervolgkeuzelijst wordt weergegeven voor alle numerieke kolommen (**verdubbelt**) die u kunt gebruiken als elementen voor de huidige grafiek.

**Splitsen op basis van**: deze vervolgkeuzelijst wordt weergegeven van alle beschikbare categorische kolommen (tekenreeksen) in uw model dat u kunt uw gegevens door te groeperen. U kunt maximaal vijf voorwaarden om weer te geven op de dezelfde x-as toevoegen. Voer de gewenste parameters en selecteer vervolgens **toevoegen** om toe te voegen een nieuwe term.

  ![Verkenner-twintig][20]

U kunt weergeven en verbergen van elementen in het deelvenster van de grafiek het zichtbaar door pictogram te selecteren zoals in de volgende afbeelding. U kunt query's volledig verwijderen door te klikken op de rode **x**.

  ![Verkenner-20-1][21]

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen:
* [Azure Time Series Insights Preview opslag en inkomend](./time-series-insights-update-storage-ingress.md)
* [Gegevens modelleren](./time-series-insights-update-tsm.md)
* [Vaststellen en oplossen van problemen](./time-series-insights-update-how-to-troubleshoot.md)

<!-- Images -->
[1]: media/v2-update-explorer/explorer-one.png
[2]: media/v2-update-explorer/explorer-two.png
[3]: media/v2-update-explorer/explorer-three.png
[4]: media/v2-update-explorer/explorer-four.png
[5]: media/v2-update-explorer/explorer-five.png
[6]: media/v2-update-explorer/explorer-six.png
[7]: media/v2-update-explorer/explorer-seven.png
[8]: media/v2-update-explorer/explorer-eight.png
[9]: media/v2-update-explorer/explorer-nine.png
[10]: media/v2-update-explorer/explorer-ten.png
[11]: media/v2-update-explorer/explorer-eleven.png
[12]: media/v2-update-explorer/explorer-twelve.png
[13]: media/v2-update-explorer/explorer-thirteen.png
[14]: media/v2-update-explorer/explorer-fourteen.png
[15]: media/v2-update-explorer/explorer-fifteen.png
[16]: media/v2-update-explorer/explorer-sixteen.png
[17]: media/v2-update-explorer/explorer-seventeen.png
[18]: media/v2-update-explorer/explorer-eighteen.png
[19]: media/v2-update-explorer/explorer-nineteen.png
[20]: media/v2-update-explorer/explorer-twenty.png
[21]: media/v2-update-explorer/explorer-twenty-one.png
