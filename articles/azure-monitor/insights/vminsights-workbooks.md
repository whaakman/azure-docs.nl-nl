---
title: Interactieve rapporten maken met Azure Monitor werkmappen | Microsoft Docs
description: Vereenvoudig complexe rapportage met vooraf gedefinieerde en aangepaste geparameteriseerde werkmappen voor Azure Monitor voor virtuele machines.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/05/2019
ms.author: magoedte
ms.openlocfilehash: 90c236347380bb5d5e51db56d0f431d2659a7258
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288709"
---
# <a name="create-interactive-reports-with-azure-monitor-workbooks"></a>Interactieve rapporten maken met Azure Monitor-werkmappen

Werkmappen tekst, combineren [query's bijgehouden](../log-query/query-language.md), metrische gegevens en parameters in uitgebreide interactieve rapporten. Werkmappen kunnen worden bewerkt door andere teamleden die toegang tot de dezelfde Azure-resources hebben.

Werkmappen zijn handig voor scenario's zoals:

* Het gebruik van uw virtuele machine verkennen wanneer u de metrische gegevens van belang van tevoren weet niet: CPU-gebruik, schijfruimte, geheugen, netwerkafhankelijkheden, enzovoort. In tegenstelling tot andere tools voor gebruiksanalyse kunnen werkmappen u combineren van meerdere soorten visualisaties en analyses, waardoor ze ideaal voor dit soort vrije verkennen.
* Uitleg over naar uw team hoe een recent ingerichte virtuele machine wordt uitgevoerd, door metrische gegevens voor de belangrijkste prestatiemeteritems en andere gebeurtenissen weer te geven.
* De resultaten van een experiment wijzigen van de grootte van uw virtuele machine delen met andere leden van uw team. U kunt uitgelegd van de doelstellingen van het experiment met tekst en vervolgens elk gebruik weergegeven metrische gegevens en analytics-query's gebruikt voor het evalueren van het experiment, samen met duidelijke call-outs voor of elke metrische waarde boven of onder-doel is.
* De impact van een storing rapportage over het gebruik van uw virtuele machine, combineren van gegevens, tekstuitleg en een beschrijving van de volgende stappen uit om te voorkomen dat storingen in de toekomst.

Azure Monitor voor VM's bevat verschillende werkmappen u aan de slag, en de volgende tabel geeft een overzicht van deze.

| Werkmap | Description | Bereik |
|----------|-------------|-------|
| Prestaties | Biedt een aanpasbare versie van onze Top N-lijst en grafieken weergeven in een enkele werkboek die gebruikmaakt van alle van de Log Analytics-prestatiemeteritems die u hebt ingeschakeld.| Op schaal |
| Prestatiemeteritems | Een Top N grafiek weergeven voor een groot aanbod van prestatiemeteritems. | Op schaal |
| Verbindingen | Verbindingen bieden een gedetailleerde weergave van de binnenkomende en uitgaande verbindingen van uw bewaakte VM's. | Op schaal |
| Actieve poorten | Geeft een lijst van de processen die zijn gebonden aan de poorten voor de bewaakte VM's en hun activiteiten in het gekozen tijdsbestek. | Op schaal |
| Open poorten | Geeft het aantal poorten openen op uw bewaakte VM's en de informatie over de poorten te openen. | Op schaal |
| Mislukte verbindingen | Het aantal mislukte verbindingen weergegeven op uw bewaakte VM's, worden de trend mislukt en als het percentage van fouten na verloop van tijd stijgt. | Op schaal |
| Beveiliging en audit | Een analyse van de TCP/IP-verkeer dat over algemene verbindingen, schadelijke verbindingen rapporten, waarbij het IP-adreseindpunten zich wereldwijd.  Als u wilt alle functies inschakelt, moet u detectie van beveiliging inschakelen. | Op schaal |
| TCP Traffic | Een gerangschikte lijst voor uw bewaakte VM's en het netwerk verzonden, ontvangen en totale verkeer in een raster en weergegeven als een trendlijn. | Op schaal |
| Vergelijking van verkeer | Deze werkmappen kunt u trends voor netwerk-verkeer voor een enkele machine of een groep machines vergelijken. | Op schaal |
| Prestaties | Biedt een aanpasbare versie van onze Prestatieweergave die gebruikmaakt van alle van de Log Analytics-prestatiemeteritems die u hebt ingeschakeld. | Eén VM | 
| Verbindingen | Verbindingen bieden een gedetailleerde weergave van de binnenkomende en uitgaande verbindingen van uw virtuele machine. | Eén VM |
 
## <a name="starting-with-a-template-or-saved-workbook"></a>Beginnen met een sjabloon of opgeslagen werkmap

Een werkmap is opgebouwd uit secties die bestaan uit onafhankelijk bewerkbare grafieken, tabellen, tekst, en besturingselementen voor de invoer. Voor meer informatie over werkmappen, laten we beginnen met het openen van een sjabloon en helpt bij het maken van een aangepaste werkmap. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **virtuele Machines**.

3. Selecteer een VM in de lijst.

4. Op de pagina virtuele machine in de **bewaking** sectie, selecteer **inzichten (preview)**.

5. Selecteer op de pagina van de inzichten VM **prestaties** of **Maps** tabblad en selecteer vervolgens **weergave werkmappen** via de koppeling op de pagina. 

    ![Schermafbeelding van de navigatie met werkmappen](media/vminsights-workbooks/workbook-option-01.png)

6. Selecteer in de vervolgkeuzelijst **Ga naar de galerie** vanaf de onderkant van de lijst.

    ![Schermafbeelding van de vervolgkeuzelijst werkmap](media/vminsights-workbooks/workbook-dropdown-gallery-01.png)

    Hiermee start u de werkmap-galerie met een aantal vooraf gedefinieerde werkmappen om u aan de slag te helpen.

7. We beginnen met de **standaardsjabloon**, bevindt zich onder de kop **Quick start**.

    ![Schermafbeelding van de werkmap-galerie](media/vminsights-workbooks/workbook-gallery-01.png)

## <a name="editing-workbook-sections"></a>Werkmap secties bewerken

Werkmappen hebben twee modi: **bewerkingsmodus**, en **Leesmodus**. Als de werkmap van de sjabloon standaard voor het eerst wordt gestart, wordt deze geopend **bewerkingsmodus**. Hier ziet u de inhoud van de werkmap, inclusief stappen en parameters die anders zijn verborgen. **Leesmodus** geeft een vereenvoudigde stijl van de rapportweergave. Leesmodus kunt u abstracte onmiddellijk de complexiteit die is opgenomen in een rapport maken terwijl u nog steeds de onderliggende mechanics slechts enkele klikken wanneer dat nodig is voor de wijziging.

![Azure Monitor voor virtuele machines werkmappen sectie besturingselementen bewerken](media/vminsights-workbooks/workbook-new-workbook-editor-01.png)

1. Wanneer u klaar bent bewerken van een sectie, klikt u op **gedaan bewerken** in de linkerbenedenhoek van de sectie.

2. Voor het maken van een duplicaat van een sectie, klikt u op de **in deze sectie klonen** pictogram. Het is een geweldige manier het herhalen voor een query zonder verlies van vorige iteraties dubbele secties te maken.

3. Als u wilt verplaatsen van een sectie in een werkmap, klikt u op de **omhoog** of **omlaag** pictogram.

4. Als u wilt een sectie definitief verwijderen, klikt u op de **verwijderen** pictogram.

## <a name="adding-text-and-markdown-sections"></a>Tekst en Markdown secties toe te voegen

Koppen, uitleg en opmerkingen toevoegen aan uw werkmappen omzetten kunnen een set met tabellen en grafieken in een boodschap. Tekst secties in ondersteuning voor werkmappen de [Markdown-syntaxis](https://daringfireball.net/projects/markdown/) voor tekst met opmaak, zoals kopteksten, vet, cursief en lijsten met opsommingstekens.

Als u wilt een tekstsectie toevoegen aan uw werkmap, gebruikt u de **tekst toevoegen** knop aan de onderkant van de werkmap of aan de onderkant van elke sectie.

## <a name="adding-query-sections"></a>Query secties toevoegen

![Gedeelte van de query in werkmappen](media/vminsights-workbooks/005-workbook-query-section.png)

Gedeelte query toevoegen aan uw werkmap, gebruikt u de **query toevoegen** knop aan de onderkant van de werkmap of aan de onderkant van elke sectie.

Query secties zijn zeer flexibel en kunnen worden gebruikt om aan te vragen beantwoorden zoals:

* Hoe was mijn CPU-gebruik tijdens dezelfde periode als een toename in netwerkverkeer?
* Wat is de ontwikkeling van de beschikbare ruimte op schijf in de afgelopen maand?
* Het aantal fouten bij het netwerk verbinden last hebben van mijn virtuele machine in de afgelopen twee weken? 

U ook worden niet alleen beperkt tot de werkmap van de query uit de context van de virtuele machine gestart. U kunt query's uitvoeren voor meerdere virtuele machines, evenals Log Analytics-werkruimten, zolang u toegang tot deze resources hebt.

Om op te nemen van gegevens uit andere Log Analytics-werkruimten of uit een specifieke Application Insights-app via de **werkruimte** id. Raadpleeg voor meer informatie over query's in meerdere bronnen, de [officiële richtlijnen](../log-query/cross-workspace-query.md).

### <a name="advanced-analytic-query-settings"></a>Geavanceerde analytische query-instellingen

Elke sectie bevat een eigen geavanceerde instellingen, die toegankelijk via de instellingen zijn ![werkmappen sectie besturingselementen bewerken](media/vminsights-workbooks/006-settings.png) pictogram gelegen aan de rechterkant van de **parameters toevoegen** knop.

![Azure Monitor voor virtuele machines werkmappen sectie besturingselementen bewerken](media/vminsights-workbooks/007-settings-expanded.png)

|         |          |
| ---------------- |:-----|
| **Aangepaste breedte**    | Maakt een item een willekeurige grootte, zodat u veel items op één regel zodat u kunt uw grafieken en tabellen beter te organiseren in uitgebreide interactieve rapporten passen.  |
| **Voorwaardelijk zichtbaar** | Als u wilt verbergen stappen op basis van een parameter in de Leesmodus opgeven. |
| **Een parameter exporteren**| Toestaan dat een geselecteerde rij in het raster of diagram ertoe leiden dat latere stappen waarden wijzigen of zichtbaar worden.  |
| **Query weergeven als er geen bewerking wordt uitgevoerd** | Geeft de query boven de grafiek of de tabel, zelfs wanneer in de leesmodus.
| **De knop Openen in Analytics weergeven wanneer er geen bewerkingen worden uitgevoerd** | De blauwe pictogram Analytics toevoegen aan de rechter hoek van de grafiek om één klik toegang te verlenen.|

De meeste van deze instellingen zijn relatief gemakkelijk, maar om te begrijpen **exporteert u een parameter** is het beter om te controleren van een werkmap die van deze functionaliteit gebruikmaakt.

Een van de vooraf gedefinieerde werkmappen - **TCP-verkeer**, vindt u informatie over metrische verbindingsgegevens van een virtuele machine.

De eerste sectie van de werkmap is gebaseerd op gegevens van een query. Het tweede gedeelte is ook gebaseerd op gegevens van een query, maar het selecteren van een rij in de eerste tabel, wordt de inhoud van de grafieken interactief bijwerken:

![Azure Monitor voor virtuele machines werkmappen sectie besturingselementen bewerken](media/vminsights-workbooks/008-workbook-tcp-traffic.png)

Het gedrag is mogelijk door het gebruik van de **wanneer een item is geselecteerd, exporteert u een parameter** geavanceerde instellingen zijn ingeschakeld in de query voor de tabel.

![Azure Monitor voor virtuele machines werkmappen sectie besturingselementen bewerken](media/vminsights-workbooks/009-settings-export.png)

Query voor de tweede maakt vervolgens gebruik van de geëxporteerde waarden als een rij is geselecteerd voor het maken van een set waarden die vervolgens worden gebruikt door de sectie met de kop en grafieken. Als er geen rij is geselecteerd, worden verborgen sectiekop en grafieken. 

De parameter verborgen in het tweede gedeelte gebruikt bijvoorbeeld de volgende verwijzing uit de geselecteerde rij in het raster:

```
VMConnection
| where TimeGenerated {TimeRange}
| where Computer in ("{ComputerName}") or '*' in ("{ComputerName}") 
| summarize Sent = sum(BytesSent), Received = sum(BytesReceived) by bin(TimeGenerated, {TimeRange:grain})
```

## <a name="adding-metrics-sections"></a>Metrische gegevens secties toevoegen

Metrische gegevens secties bieden u volledige toegang tot Azure Monitor metrische gegevens opnemen in uw interactieve rapporten. De vooraf gedefinieerde werkmappen wordt in Azure Monitor voor virtuele machines, doorgaans bevatten analytische query voor gegevens in plaats van metrische gegevens.  U kunt kiezen om te maken van werkmappen met metrische gegevens, zodat u kunt profiteren van het beste van beide functies op één plek. U hebt ook de mogelijkheid om op te halen metrische gegevens van resources in een van de abonnementen die u toegang tot hebt.

Hier volgt een voorbeeld van gegevens van virtuele machines worden opgehaald in een werkmap om te bieden van een visualisatie raster van CPU-prestaties:

![Azure Monitor voor virtuele machines werkmappen sectie besturingselementen bewerken](media/vminsights-workbooks/010-metrics-grid.png)

## <a name="adding-parameter-sections"></a>Parameter secties toevoegen

Werkmapparameters kunnen u waarden in de werkmap zonder handmatig bewerken in de secties query of tekst wijzigen. Dit verwijdert de vereiste van dat u hoeft te begrijpen van de onderliggende analytics-querytaal en aanzienlijk breidt de potentiële doelgroep van rapportage op basis van een werkmap.

De waarden van parameters in query-, tekst- of andere gedeelten van de parameter zijn vervangen door de naam van de parameter accolades, plaatsen, zoals ``{parameterName}``. Namen van taakparameters zijn beperkt tot dezelfde regels als JavaScript-id's, alfabetische tekens of onderstrepingstekens bevatten, gevolgd door alfanumerieke tekens of onderstrepingstekens bevatten. Bijvoorbeeld, **a1** is toegestaan, maar **1a** is niet toegestaan.

Parameters zijn lineair, starten vanaf de bovenkant van een werkmap en doorgestuurd naar latere stappen.  Parameters die zijn gedeclareerd later in een werkmap kunnen overschrijven parameters die eerder zijn gedeclareerd. Hiermee kunt ook parameters die query's gebruiken om toegang te krijgen tot de waarden van parameters die u eerder hebt gedefinieerd. In de stap van de parameter zelf zijn parameters ook lineaire, links naar rechts, waarbij parameters aan de rechterkant een parameter die eerder in dat dezelfde stap gedeclareerd kunnen vertrouwen.
 
Er zijn vier typen parameters, die momenteel worden ondersteund:

|                  |      |
| ---------------- |:-----|
| **Tekst**    | Kan de gebruiker om een tekstvak te bewerken en kunt u desgewenst een query in te vullen in de standaard-waarde opgeven. |
| **Vervolgkeuzelijst** | Kan de gebruiker om te kiezen uit een set waarden. |
| **Tijdsbereikkiezer**| Kan de gebruiker om te kiezen uit een vooraf gedefinieerde set waarden voor tijd of kies uit meer dan een aangepast tijdsbereik.|
| **Resourcekiezer** | Kan de gebruiker om te kiezen in de resources voor de werkmap geselecteerd.|

### <a name="using-a-text-parameter"></a>Met behulp van een tekstparameter

De waarde van een gebruiker typt in het tekstvak wordt vervangen rechtstreeks in de query, zonder aanhalingstekens of aanhalingstekens voor. Als de waarde die u moet een tekenreeks is de query aanhalingstekens rond de parameter moet hebben (zoals **{parameter}**).

De tekstparameter zorgt ervoor dat de waarde in een tekstvak in op overal worden gebruikt. Mag een tabelnaam, kolomnaam, naam van de functie, operator, enzovoort.  Het parametertype tekst is een instelling **standaardwaarde ophalen uit analytische query**, waarmee de auteur van werkmap een query gebruiken voor het vullen van de standaardwaarde voor het tekstvak.

Wanneer u de standaardwaarde van een logboekquery, worden alleen de eerste waarde van de eerste rij (rij 0, 0 kolom) wordt gebruikt als de standaardwaarde. Daarom is het raadzaam om uw query om terug te keren slechts één rij en één kolom te beperken. Andere gegevens die zijn geretourneerd door de query wordt genegeerd. 

Ongeacht de query retourneert de waarde wordt vervangen rechtstreeks met geen aanhalingstekens of aanhalingstekens. Als de query geen rijen worden geretourneerd, wordt het resultaat van de parameter is een lege tekenreeks (als de parameter niet vereist is) of een niet-gedefinieerde (als de parameter vereist is).

### <a name="using-a-drop-down"></a>Met behulp van een vervolgkeuzelijst

Het parametertype van de vervolgkeuzelijst kunt u een besturingselement voor vervolgkeuzelijst maken zodat de selectie van een of meer waarden.

De vervolgkeuzelijst is gevuld met een query voor of JSON. Als de query één kolom retourneert, wordt de waarden in die kolom zijn zowel de waarde als het label in het besturingselement voor vervolgkeuzelijst. Als de query twee kolommen retourneert, de eerste kolom is de waarde en de tweede kolom wordt het label wordt weergegeven in de vervolgkeuzelijst. Als de query drie kolommen worden geretourneerd, is de derde kolom wordt gebruikt om aan te geven van de standaardselectie in deze vervolgkeuzelijst. Deze kolom kan elk type, maar de eenvoudigste manier is om te gebruiken bool of numerieke typen, waarbij 0 is ingesteld op false en 1 is ingesteld op true.

Als de kolom een tekenreekstype is null/leeg tekenreeks wordt beschouwd als onwaar en een andere waarde wordt beschouwd als ' True '. Voor enkelvoudige selectie vervolgkeuzelijsten, wordt de eerste waarde met de waarde true als de standaardselectie gebruikt.  Alle waarden met de waarde true zijn voor meerdere selectie vervolgkeuzelijsten gebruikt als de standaardset geselecteerd. De items in de vervolgkeuzelijst worden weergegeven in welke volgorde de query rijen geretourneerd. 

We bekijken de parameters die aanwezig zijn in het rapport overzicht van verbindingen. Klik op het symbool bewerken naast **richting**.

![Azure Monitor voor virtuele machines werkmappen sectie besturingselementen bewerken](media/vminsights-workbooks/011-workbook-using-dropdown.png)

Hiermee wordt geopend de **Parameter bewerken** menu-item.

![Azure Monitor voor virtuele machines werkmappen sectie besturingselementen bewerken](media/vminsights-workbooks/012-workbook-edit-parameter.png)

De JSON kunt u een willekeurige tabel gevuld met inhoud genereren. Bijvoorbeeld, genereert de volgende JSON twee waarden in de vervolgkeuzelijst:

```
[
    { "value": "inbound", "label": "Inbound"},
    { "value": "outbound", "label": "Outbound"}
]
```

Een voorbeeld van een meer van toepassing is een vervolgkeuzelijst gebruiken om op te halen uit een set van prestatiemeteritems met de naam:

```
Perf
| summarize by CounterName, ObjectName
| order by ObjectName asc, CounterName asc
| project Counter = pack('counter', CounterName, 'object', ObjectName), CounterName, group = ObjectName
```

De query worden resultaten als volgt weergegeven:

![Voor prestaties teller vervolgkeuzelijst](media/vminsights-workbooks/013-workbook-edit-parameter-perf-counters.png)

Vervolgkeuzelijsten zijn zeer krachtige hulpprogramma's voor het aanpassen en het maken van interactieve rapporten.

### <a name="time-range-parameters"></a>Bereik tijdparameters

Terwijl u uw eigen aangepaste tijd bereik parameter via het parametertype van de vervolgkeuzelijst maken kunt, kunt u de out-of-box parametertype voor tijdsbereik ook gebruiken als u niet dezelfde mate van flexibiliteit hoeft. 

Time range parametertypen hebben 15 standaard bereiken die naar de laatste 90 dagen gaan van vijf minuten. Er is ook een optie om toe te staan selectie aangepast tijdsbereik, waardoor de operator van het rapport om te kiezen expliciete starten en stoppen van de waarden voor het tijdsbereik.

### <a name="resource-picker"></a>Resourcekiezer

Het parametertype van de resource-kiezer biedt u de mogelijkheid om het bereik van uw rapport voor bepaalde typen resources te. Een voorbeeld van een vooraf gedefinieerde werkmap die gebruikmaakt van het type resource datumkiezer is de **prestaties** werkmap.

![Vervolgkeuzelijst voor werkruimten](media/vminsights-workbooks/014-workbook-edit-parameter-workspaces.png)

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Opslaan en delen van werkmappen met uw team

Werkmappen zijn opgeslagen in een Log Analytics-werkruimte of een virtuele-machinebron, afhankelijk van hoe u toegang de werkmappen-galerie tot. De werkmap kan worden opgeslagen in de **mijn rapporten** persoonlijke in u of in de sectie de **gedeelde rapporten** sectie die toegankelijk is voor iedereen met toegang tot de resource. Als u wilt alle werkmappen in de bron weergeven, klikt u op de **Open** knop in de actiebalk.

Voor het delen van een werkmap die op dat moment wordt **mijn rapporten**:

1. Klik op **Open** in de actiebalk
2. Klik op de knop '...' naast de werkmap die u wilt delen
3. Klik op **verplaatsen naar gedeelde rapporten**.

Als u wilt delen een werkmap met een koppeling of via e-mail, klikt u op **delen** in de actiebalk. Houd er rekening mee dat ontvangers van de koppeling moeten toegang hebben tot deze resource in Azure portal om de werkmap weer te geven. Als u wilt bewerken, moeten ontvangers minimaal Inzender-rechten voor de resource.

Een koppeling naar een werkmap in een Azure-Dashboard vastmaken:

1. Klik op **Open** in de actiebalk
2. Klik op de knop '...' naast de werkmap die u wilt vastmaken
3. Klik op **Vastmaken aan dashboard**.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van de health-functie, [weergave Azure VM-status](vminsights-health.md), of als u afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md). 