---
title: Gegevens visualiseren in de update Explorer | Microsoft Docs
description: Azure Time Series Insights bijwerken Explorer
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/28/2018
ms.openlocfilehash: bf091eec271e3fb27f6ab312ff5d0096efa7f90c
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856074"
---
# <a name="visualize-data-in-the-explorer-update"></a>Gegevens visualiseren in de Verkenner-update

Dit artikel beschrijft de functies en opties die beschikbaar zijn in de Azure Time Series Insights (preview) Explorer-webtoepassing.

## <a name="prerequisites"></a>Vereisten

Voordat u de Azure Time Series Insights (preview) Explorer gebruikt, moet u het volgende doen:

* Een Time Series Insights-omgeving ingericht hebt. Meer informatie over het inrichten van een Time Series Insights-omgeving.
* Gegevenstoegang verlenen tot de Time Series Insights-omgeving die u hebt gemaakt voor het account. Toegang kan worden opgegeven aan anderen, evenals zelf.
* Een gebeurtenisbron toevoegen aan de Time Series Insights-omgeving om gegevens te pushen naar de omgeving.

## <a name="learn-about-the-azure-time-series-insights-preview-explorer"></a>Meer informatie over de Azure Time Series Insights (preview) Explorer

  ![Explorer-een][1]

Azure Time Series Insights (preview) Explorer wordt opgedeeld in de volgende zeven elementen:

1. Navigatiebalk TSI (preview) kunt u schakelen tussen pagina's voor analyse en het model.
1. Hiërarchiestructuur TSI (preview) Selecteer specifieke gegevenselementen weer te geven.
1. Tijdreeks TSI (preview) worden ook alle elementen van de momenteel geselecteerde gegevens bevat.
1. TSI (preview) grafiek deelvenster weergegeven uw huidige werken-grafiek.
1. Tijdlijn van TSI (preview) kunt u uw werkende periode wijzigen.
1. TSI (preview) app-balk bevat uw opties voor het beheer van gebruiker (zoals de huidige tenant), en kunt u wijzigen thema-en taalinstellingen.

## <a name="tsi-preview-environment-panel"></a>TSI (preview) omgeving deelvenster

De omgeving-paneel geeft alle TSI-omgevingen die u toegang tot hebt weer. Dit omvat een overzicht van omgevingen met betalen per gebruik (preview) en S1/S2-omgevingen (GA). Klik op de TSI-omgeving die u wilt gebruiken.

  ![Verkenner-twee][2]

## <a name="time-series-insights-preview-navigation-menu"></a>Time Series Insights (preview) navigatiemenu

  ![Verkenner-drie][3]

Het navigatiemenu kunt u schakelen tussen de TSI-toepassingen:

* Analyseer – Hiermee kunt u de grafiek en gedetailleerde analyses uitvoeren op de gemodelleerde of unmodeled time series-gegevens.

* Model - kunt u nieuwe TSI-update-typen, hiërarchieën en exemplaren aan uw model TSI pushen.

## <a name="time-series-insights-update-model-authoring"></a>Time Series Insights bijwerken ontwerpen van het Model

Deze toepassing biedt u de mogelijkheid om uit te voeren CRUD-bewerkingen op uw Time Series-Model.  

* Type TSM - TSI-typen kunt definiëren variabelen of formules voor het uitvoeren van berekeningen, ze zijn gekoppeld aan een bepaald TSI-exemplaar. Een type kan een of meer variabelen hebben.
* TSM hiërarchie - hiërarchieën zijn systematische organisaties van uw gegevens. Hiërarchieën weer de relaties tussen verschillende entiteiten in uw gegevens van TSI.
* TSM instance - exemplaren zijn de tijdreeksen zelf. In de meeste gevallen is dit de **DeviceID** of **AssetID**, dit is de unieke id van de activa in de omgeving.

Lees voor meer informatie over de TSM [tijden reeks modellen](./time-series-insights-update-tsm.md).

## <a name="time-series-insights-preview-model-search-panel"></a>Time Series Insights (preview) paneel voor zoeken van Model

Het deelvenster model zoeken kunt u eenvoudig zoeken naar en navigeer naar uw hiërarchie TSM om te vinden van de specifieke tijd series-instanties die u wilt weergeven in de grafiek. Wanneer u uw instanties selecteert, worden ze niet alleen worden toegevoegd aan de huidige grafiek maar worden ook toegevoegd aan de gegevens ook.

  ![Verkenner-vier][4]

## <a name="time-series-insights-preview-well"></a>Time Series Insights preview () en

De bron worden exemplaarvelden en andere metagegevens die zijn gekoppeld aan de geselecteerde time series-instanties weergegeven. De selectievakjes in aan de rechterkant kunt u specifieke exemplaren van de huidige grafiek weergeven of verbergen. U kunt ook specifieke gegevenselementen verwijderen uit uw huidige gegevens door te klikken op de rode x-besturingselement aan de rechterkant van het element.

  ![Verkenner-vijf][5]

U kunt ook pop-out het paneel telemetrie voor een beter verticale weergave van de elementen in uw gegevens goed.

  ![Verkenner-6][6]

Opmerking: als u het volgende pictogram ziet, het exemplaar heeft geen gegevens tijdens de periode die is geselecteerd.  Als u wilt oplossen, u kunt verhogen van het interval dat is geselecteerd en/of bevestigen dat het exemplaar van gegevens pushen.

  ![Explorer 7][7]

## <a name="time-series-insights-preview-chart"></a>Time Series Insights (preview) grafiek

De grafiek kunt u weergeven van time series-instanties als regels. U kunt het deelvenster van de omgeving, gegevensmodel en tijd van span het Configuratiescherm door te klikken op de webbesturingselementen groter maken van de grafiek samenvouwen.

  ![Verkenner-8][8]

1. Geselecteerde datumbereik: het geselecteerde datumbereik voor het deelvenster van de grafiek, deze bepaalt welke elementen zijn beschikbaar voor visualisatie.

1. Binnenste datum bereik-schuifregelaar - endpoint-besturingselementen met de twee te klikken en slepen ze na verloop van de gewenste tijd omvatten.

1. Tijd span samenvouwen control - dit besturingselement samenvouwen en breidt de tijd span Configuratiescherm-editor.

1. Y-as-indeling beheer – klikt u op dit besturingselement doorloopt u de beschikbare opties voor de y-as. De beschikbare opties voor de y-as zijn:

    * Standaard: elke regel bevat een afzonderlijke y-as
    * Gestapeld – kunt hiermee u meerdere regels op de dezelfde y-as-stack met de y-as gegevens wijzigen op basis van de geselecteerde regel
    * Gedeeld: alle y-as-gegevens samen weergegeven

1. Huidige gegevenselement: het geselecteerde element en de bijbehorende gegevens.

U kunt verder inzoomen in een specifieke gegevenssegment door te klikken een gegevenspunt in de huidige grafiek met de linkermuisknop terwijl de muisknop ingedrukt en klikt u vervolgens het geselecteerde gebied slepen naar het eindpunt van uw keuze. Met de rechtermuisknop op het gebied grijs weergegeven, geselecteerde en inzoomen op, zoals hieronder wordt weergegeven. U kunt ook het volgende doen:

  ![Explorer 9][9]

Na het uitvoeren van de actie inzoomen, ziet u nu de geselecteerde gegevensset. Klik op het besturingselement voor het opmaken van y-as doorloopt u de drie verschillende y-as-weergaven van uw gegevens van TSI.

  ![Verkenner-tien][10]

Hier ziet u een voorbeeld van een gedeelde y-assen.

  ![Verkenner-11][11]

## <a name="time-series-insights-preview-time-editor-panel"></a>Time Series Insights (preview) deelvenster tijd Editor

Als u werkt met TSI selecteert u eerst een tijdspanne. De geselecteerde periode wordt de gegevensset die beschikbaar is voor het omgaan met de update TSI widgets beheren. De volgende webbesturingselementen zijn beschikbaar in de TSI-update voor het selecteren van de tijdspanne werken.

  ![Verkenner-twaalf][12]

1. **Schuifregelaar voor interne datumbereik** - gebruik van de twee eindpunt besturingselementen door te klikken en slepen na verloop van de gewenste tijd omvatten. Dit bereik 'Binnenste datum' zal worden beperkt door de schuifregelaar van de 'Buitenste datum' bereik hieronder genoemde.

1. Vergroten en verkleinen datum bereik knoppen ** - vergroten of verkleinen van de tijdspanne door te klikken op een van de knoppen voor het interval dat u wilt.

1. **Tijd van span samenvouwen besturingselement** -dit webbesturingselement kunt u de besturingselementen, met uitzondering van de schuifregelaar van binnenste datum bereik verbergen.

1. **Besturingselement voor schuifregelaar outer datumbereik** -met behulp van de eindpunt-besturingselementen het buitenste datumbereik die beschikbaar zijn voor het bereik 'Binnenste datum' besturingselement selecteren.

1. **Snelle tijden datumbereik vervolgkeuzelijst** -biedt u de mogelijkheid snel schakelen tussen de vooraf ingestelde tijd span instellingen zoals de laatste 30 minuten, de afgelopen 12 uur, aangepaste bereik, enzovoort. Als u deze waarde wijzigt, wijzigt ook de beschikbare interval bereiken in de schuifregelaar voor grootte van interval wordt hierna besproken.

1. **Intervalgrootte schuifregelaar** -het interval grootte schuifregelaar hulpprogramma kunt u om in te zoomen en afmelden bij intervallen via de dezelfde periode. Dit biedt meer controle over verkeer tussen grote segmenten van de tijd die smooth trends op segmenten zo klein is als de milliseconde nauwkeurig, zodat u kunt om te zien met hoge resolutie, gedetailleerde delen van uw gegevens weergeven. Startpunt van de schuifregelaar-standaard is ingesteld als de meest optimale weergave van de gegevens van uw selectie. Netwerktaakverdeling resolutie, de snelheid van de query en de granulatie.

1. **Datumbereik naar en van het webbesturingselement voor** - met deze webbesturingselement voor u kunt eenvoudig op en selecteer de gewenste datum en tijd bereiken. U kunt ook het besturingselement gebruiken om over te schakelen tussen de verschillende tijdzones. Klik op opslaan nadat u de wijzigingen die u wilt toepassen op uw huidige werkruimte knop.

  ![Verkenner-13][13]

## <a name="time-series-insights-preview-navigation-panel"></a>Navigatiepaneel van Time Series Insights (preview)

Het navigatiepaneel van TSI update biedt de volgende functionaliteit:

  ![Verkenner-14][14]

### <a name="current-session-share-link-control"></a>Huidige bestandsshare koppeling sessiebeheer

  ![Verkenner-vijftien][15]

Klik op het webbesturingselement voor omcirkeld koppeling voor het genereren van een URL als u wilt opslaan of delen van uw huidige Time Series Insights werken-sessie, waaronder:

* Momenteel geselecteerde tijdsbereik
* Intervalgrootte van geselecteerde
* Geselecteerde gegevens goed

### <a name="tenant-section"></a>Sectie tenant

  ![Verkenner-zestien][16]

* Huidige TSI aanmelding gegevens over uw account wordt weergegeven
* Hiermee kunt dat u schakelen tussen de beschikbare TSI-thema's.

### <a name="theme-selection"></a>Thema selecteren

De Azure TSI (preview) biedt ondersteuning voor twee thema's:

* **Licht thema**: dit is het standaardthema dat wordt weergegeven in dit document.
* **Donker thema**: deze optie wordt de explorer in een donker thema weergegeven zoals hieronder wordt weergegeven:

  ![Verkenner-17][17]

U kunt hier ook wijzigen tussen ondersteunde talen.

## <a name="s1s2-environment-controls"></a>Besturingselementen voor S1/S2-omgeving

### <a name="time-series-insights-preview-terms-panel"></a>Time Series Insights (preview) deelvenster termen

Deze sectie geldt alleen voor bestaande S1/S2-omgevingen, probeert de explorer gebruiken in de bijgewerkte gebruikersinterface. Het is raadzaam om dit voor het gebruik van het product algemeen beschikbaar en bijwerken (preview) in combinatie met elkaar te doen. We bepaalde functionaliteit van de bestaande gebruikersinterface aan de bijgewerkte explorer hebt toegevoegd, maar u weet dat u kunt altijd kunnen genieten van de volledige gebruikersinterface-ervaring voor S1/S2-omgeving in de bestaande TSI-Verkenner.  

In plaats van de hiërarchie ziet u het deelvenster van de termen TSI. Dit is waar u query's in uw omgeving definiëren. Dit biedt u de mogelijkheid om uw gegevens op basis van met behulp van een predicaat te filteren.

  ![Verkenner-18][18]

De TSI (preview)-Editor-deelvenster termen bevat de volgende parameters

**Waar**: waar u de component kunt u snel uw om gebeurtenissen te filteren met behulp van de set met operanden hieronder vermeld. Als u een zoekopdracht door selecteren/te klikken uitvoeren, het predicaat wordt automatisch bijwerken op basis van die zoekopdracht. Ondersteunde Operandtypen zijn onder andere:

| Bewerking | Ondersteunde typen   | Opmerkingen |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` |    Double, DateTime, TimeSpan  | |
| `=`, `!=`, `<>`   | String, Bool, Double, DateTime, TimeSpan NULL    |
| `IN` |    String, Bool, Double, DateTime, TimeSpan NULL |    Alle operands moeten zijn van hetzelfde type of NULL-constante. |
| `HAS` |   Reeks |    Alleen constante letterlijke tekenreeksen zijn toegestaan op de rechterkant. Lege tekenreeks en NULL zijn niet toegestaan. |

### <a name="examples-of-where-clauses"></a>Voorbeelden van de locatie waar de EU

  ![Verkenner-19][19]

**Meting**: dit alle numerieke kolommen vervolgkeuzelijst wordt weergegeven (**verdubbelt**) die u kunt gebruiken als elementen voor de huidige grafiek.

**Splitsen op basis van**: deze vervolgkeuzelijst bevat alle categorische kolommen (tekenreeksen) in uw model beschikbaar is, dat u gebruiken kunt om uw gegevens door te groeperen. U kunt maximaal vijf voorwaarden om weer te geven op de dezelfde x-as toevoegen. Voer de gewenste parameters en gebruik vervolgens de **toevoegen** om toe te voegen een nieuwe term.

  ![Verkenner-twintig][20]

U kunt verbergen en elementen in het deelvenster grafiek weergeven door te klikken op het pictogram van weergegeven zoals hieronder wordt weergegeven. U kunt query's volledig verwijderen door te klikken op de rode `X` hieronder wordt weergegeven.

  ![Verkenner-20-1][21]

## <a name="next-steps"></a>Volgende stappen

Lees de [Azure TSI (preview) Storage en binnenkomende](./time-series-insights-update-storage-ingress.md).

Meer informatie over de nieuwe [Time Series modellen](./time-series-insights-update-tsm.md).

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