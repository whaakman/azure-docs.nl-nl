---
title: Azure AI-handleiding voor voorspeld onderhoud oplossingen | Microsoft Docs
description: Een uitgebreide beschrijving van de wetenschappelijke gegevens, die ook door voorspeld onderhoud oplossingen in verschillende branches.
services: machine-learning
author: fboylu
manager: cgronlun
editor: ''
ms.assetid: 2e8b66db-91eb-432b-b305-6abccca25620
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: fboylu
ms.openlocfilehash: ff2e1660ffcc1f397697b27084e000371c7c84f3
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938006"
---
# <a name="azure-ai-guide-for-predictive-maintenance-solutions"></a>Azure AI-handleiding voor voorspeld onderhoud oplossingen

## <a name="summary"></a>Samenvatting

Voorspeld onderhoud (**PdM**) is een populaire toepassing van predictive analytics waarmee u kunt bedrijven in verschillende branches hoge asset gebruik bereiken en in de operationele kosten worden bespaard. Deze handleiding verenigt de zakelijke en analytische richtlijnen en aanbevolen procedures om correct te ontwikkelen en implementeren van PdM oplossingen met behulp van de [AI van Microsoft Azure-platform](https://azure.microsoft.com/overview/ai-platform) technologie.

Deze handleiding bevat om te beginnen branchespecifieke bedrijfsscenario's en het proces van deze scenario's voor PdM in aanmerking komt. De vereiste gegevens en modelleringstechnieken PdM oplossingen bouwen worden ook gegeven. De belangrijkste inhoud van de handleiding is op het proces voor wetenschap - met inbegrip van de stappen voor het voorbereiden van gegevens, functie-engineering, model maken en uitoefening model. Als aanvulling op de belangrijkste concepten, bevat deze handleiding een set van oplossingssjablonen om te helpen toepassingsontwikkeling PdM versnellen. De handleiding verwijst ook naar nuttig training resources voor de arts voor meer informatie over de AI achter de wetenschappelijke gegevens. 

### <a name="data-science-guide-overview-and-target-audience"></a>Gegevens wetenschappelijke handleiding overzicht en doel doelgroep
Het eerste deel van deze handleiding worden beschreven in de meeste zakelijke problemen, de voordelen van de implementatie van PdM om deze problemen op te lossen en bevat een aantal algemene gebruiksvoorbeelden. Besluitvormers (BDM) profiteren van deze inhoud. De tweede helft worden de wetenschappelijke gegevens achter PdM en geeft een lijst van PdM oplossingen die zijn gebouwd met behulp van de principes die in deze handleiding worden beschreven. Het bevat ook leertrajecten en verwijzingen naar trainingsmateriaal. Technische besluitvormers (TDMs) handig deze inhoud.

| Beginnen met... | Als u... |
|:---------------|:---------------|
| [Bedrijfsscenario voor voorspeld onderhoud](#Business-case-for-predictive-maintenance) |een besluitvormer (Besluitvormer) wilt downtime en operationele kosten verminderen en gebruik van de apparatuur te verbeteren |
| [Gegevenswetenschap voor voorspeld onderhoud](#Data-Science-for-predictive-maintenance) |een technische besluitvormer (TDM) evalueren PdM technologieën om te begrijpen van de unieke gegevensverwerking en AI-vereisten voor voorspeld onderhoud |
| [Oplossingssjablonen voor voorspeld onderhoud](#Solution-templates-for-predictive-maintenance)|een architect van software of AI-ontwikkelaars snel staan een demo of een bewijs van concept opzoeken |
| [Training resources voor voorspeld onderhoud](#Training-resources-for-predictive-maintenance) | een of meer van de bovenstaande en wilt weten van de fundamentele concepten achter de wetenschappelijke gegevens, hulpprogramma's en technieken.

### <a name="prerequisite-knowledge"></a>Vereiste kennis
De inhoud Besluitvormer verwacht geen eerdere gegevens wetenschappelijke kennis hebben van de lezer. Basiskennis van statistieken en gegevenswetenschap is voor de inhoud TDM handig. Kennis van de Azure-gegevens en AI-services, Python, R, XML en JSON wordt aanbevolen. AI technieken worden geïmplementeerd in Python en R-pakketten. Oplossingssjablonen worden geïmplementeerd met Azure-services, ontwikkelingsprogramma's en SDK's.

## <a name="business-case-for-predictive-maintenance"></a>Bedrijfsscenario voor voorspeld onderhoud

Bedrijven vereisen kritieke apparatuur piek-efficiëntie en hun rendement op investering investeringen in mee worden uitgevoerd. Deze activa kunnen variëren van vliegtuigmotoren, turbines, liften of industriële chillers - die miljoenen - omlaag naar de dagelijkse apparaten zoals fotokopieerapparaten, koffiezetmachines of Waterkoelers kosten.
- Standaard is de meeste bedrijven zijn afhankelijk van _corrigerende onderhoud_, waar de onderdelen worden vervangen als en wanneer ze mislukken. Corrigerende onderhoud zorgt ervoor onderdelen volledig worden gebruikt (dus niet slijtende onderdeel leven), maar het bedrijf verbonden kosten van uitvaltijd, arbeid en niet-gepland Onderhoudsvereisten (uit uren of onhandig locaties).
- Op de volgende niveau, bedrijven oefening _preventief onderhoud_, waar ze bepalen van de nuttige levensduur voor een deel en onderhouden of voordat er een fout vervangen. Preventief onderhoud wordt niet-geplande en onherstelbare fouten voorkomen. Maar de hoge kosten van geplande downtime onder gebruik van het onderdeel vóór de volledige levensduur van het gebruik en arbeid nog steeds blijven.
- Het doel van _voorspeld onderhoud_ is het optimaliseren van de balans tussen corrigerende en preventief onderhoud, doordat _in de tijd_ vervanging van componenten. Deze aanpak vervangt alleen deze onderdelen wanneer ze zich dicht bij een storing. Door uit te breiden onderdeel lifespans (vergeleken met preventief onderhoud) en het verminderen van niet-gepland onderhoud en kosten van arbeid (via corrigerende onderhoud), bedrijven kunnen krijgen tot kostenbesparingen en concurrerende voordelen.

## <a name="business-problems-in-pdm"></a>Zakelijke problemen in PdM
Bedrijven geconfronteerd hoog operationele risico vanwege onverwachte fouten en inzicht in de hoofdoorzaak van problemen in complexe systemen kan beperken. Enkele van de belangrijkste zakelijke vragen zijn:

- Detecteren afwijkingen in apparatuur of systeemprestaties of functionaliteit.
- Voorspellen of een actief in de nabije toekomst kan mislukken.
- Schatting maken van de resterende levensduur van activa.
- Identificeer de belangrijkste oorzaken van storingen in voor een asset.
- Identificeer onderhoudsacties moeten worden uitgevoerd, door wanneer op een asset.

Gebruikelijke doel instructies uit PdM zijn:

- Operationele risico's van missie kritieke apparatuur te beperken.
- Rendement van activa vergroten door het voorspellen van fouten voordat ze optreden.
- Kosten voor onderhoud doordat just in time onderhoudsbewerkingen beheren.
- Klant afslijting verlagen, de installatiekopie van het merk en verloren verkoop verbeteren.
- Lagere voorraadkosten met te verminderen voorraden voorspellen van het punt opnieuw ordenen.
- Ontdek de patronen die zijn verbonden met verschillende problemen met het onderhoud.
- Geef KPI's (key performance indicators) zoals health scores voor asset voorwaarden.
- Geschatte resterende levensduur van activa.
- Aanbevolen tijdige onderhoudsactiviteiten.
- Alleen in de inventaris van de tijd inschakelen door het schatten van de volgorde datums voor vervanging van onderdelen.

Deze instructies doel vormen het beginpunt voor:

- _gegevenswetenschappers_ te analyseren en oplossen van problemen met specifieke voorspeld.
- _cloud-architecten en ontwikkelaars_ samenstellen van een end-to-end-oplossing.

## <a name="qualifying-problems-for-predictive-maintenance"></a>In aanmerking komende problemen voor voorspeld onderhoud
Het is belangrijk om te benadrukken dat niet alle gebruiksvoorbeelden of zakelijke problemen effectief kunnen worden opgelost door PdM. Er zijn drie belangrijke in aanmerking komend criteria die moeten worden overwogen tijdens probleem selectie:

- Het probleem moet kunnen worden voorspeld aard; dat wil zeggen, moet er een doel of het resultaat te voorspellen. Het probleem hebt ook een duidelijke pad van de actie die moet worden voorkomen dat mislukken wanneer ze worden gedetecteerd.
- Het probleem moet een record van de operationele geschiedenis van de apparatuur die bevat _goed of slecht resultaten_. De set acties genomen om onjuiste resultaten moeten ook beschikbaar als onderdeel van deze records. Foutenrapporten, onderhoud logboeken van de degradatie van de prestaties, herstel en vervang Logboeken ook belangrijk zijn. Reparaties uitgevoerd om te verbeteren en vervanging records zijn ook nuttig.
- De opgenomen geschiedenis moet worden weergegeven in _relevante_ gegevens _voldoende_ voldoende kwaliteit ter ondersteuning van het gebruiksvoorbeeld. Zie voor meer informatie over gegevens relevantie en toereikend waren [eisen voor voorspeld onderhoud](#Data-requirements-for-predictive-maintenance).
- Ten slotte moet het bedrijf domein deskundigen die een duidelijk beeld van het probleem hebben. Ze moet op de hoogte van de interne processen en procedures om u te helpen de analist begrijpen en de gegevens interpreteren. Ze moeten ook worden de benodigde wijzigingen aanbrengen in bestaande bedrijfsprocessen te verzamelen van de juiste gegevens voor de problemen indien nodig.

## <a name="sample-pdm-use-cases"></a>Voorbeeld PdM gebruiksvoorbeelden
Deze sectie richt zich op een verzameling van PdM gebruiksvoorbeelden van verschillende branches zoals ruimtevaart, hulpprogramma's en transport. Elke sectie begint met een zakelijk probleem en beschrijft de voordelen van PdM, de relevante gegevens rondom het zakelijke probleem en ten slotte de voordelen van een oplossing PdM.

| Zakelijke probleem | Voordelen van PdM |
|:-----------------|-------------------|
|**Luchtvaart**      |                   |
|_Vertraging en annuleringen Flight_ door mechanische problemen. Fouten die in de tijd kunnen niet worden hersteld kunnen veroorzaken vlucht worden geannuleerd en planning en bewerkingen verstoren. |PdM oplossingen kunnen de kans dat een vliegtuig worden vertraagd of geannuleerd vanwege mechanische fouten voorspellen.|
|_Vliegtuig engine delen fout_: vliegtuig engine onderdeel vervangingen zijn een van de meest voorkomende onderhoudstaken binnen de branche luchtvaartmaatschappij. Onderhoud oplossingen vereisen zorgvuldig beheer van het onderdeel voorraad, levering en planning|Om te kunnen verzamelen intelligence op onderdeel betrouwbaarheid leidt tot een aanzienlijke vermindering van investeringskosten.|
|**Finance** |                         |
|_ATM-fout_ is een veelvoorkomend probleem in de branche bankieren. Het probleem hier is de kans dat een ATM-geld intrekking transactie wordt onderbroken vanwege een fout papier papierstoring of onderdeel in de dispenser geld rapporteren. Op basis van de voorspellingen van transactie fouten, kunnen ATM's worden gebruikt voor proactief om te voorkomen dat er fouten optreden.| In plaats van dat de machine halverwege via een transactie mislukt, is het wenselijk alternatief programma de machine service weigeren op basis van de voorspelling.|
|**Energie** |                          |
|_Turbine fouten liquidatie_: Windturbines zijn de belangrijkste energiebron in milieu verantwoordelijk landen en hebben betrekking op hoge kosten. Een belangrijk onderdeel Windturbines is de generator motor. de fout geeft turbine ongeschikt. Het is ook zeer kostbaar zijn om op te lossen.|Het voorspellen van KPI's zoals MTTF (gemiddelde tijd bij een storing), kunt u de energiebedrijven voorkomen dat turbine mislukken, en zorg ervoor dat de minimale downtime. Fout kansen technici voor het bewaken van turbines die zijn waarschijnlijk snel geen kennis en onderhoud op basis van tijd regelingen plannen. Voorspellende modellen bieden inzicht in verschillende factoren bijdragen aan de fout waarmee technici beter begrip van dat de belangrijkste oorzaken van problemen.|
|_Circuitonderbreker fouten_: distributie van elektriciteit naar thuis en in bedrijven vereist power regels worden operationele te allen tijde energie levering garanderen. Stroomonderbrekers te beperken of te voorkomen dat schade aan power regels tijdens overbelasting of ongewenst weersomstandigheden. Het zakelijke probleem is om te voorspellen Circuitonderbreker fouten.| PdM oplossingen helpen reparatiekosten te verlagen en de levensduur van apparatuur zoals stroomonderbrekers verhogen. Ze helpen de kwaliteit van het netwerk power verbeteren door te verminderen onverwachte fouten en serviceonderbrekingen.|
|**Transport en logistiek** |    |
|_Lift deur fouten_: lift grote bedrijven bieden een volledige stackservice voor miljoenen functionele liften over de hele wereld. Lift-veiligheid, betrouwbaarheid en beschikbaarheid zijn de belangrijkste zorgen voor hun klanten. Deze bedrijven bijhouden deze en verschillende andere kenmerken via sensoren om hen te helpen met corrigerende en preventief onderhoud. In een korte, is het belangrijkste probleem klant lift deuren defect. Het zakelijke probleem wordt in dit geval een voorspellende knowledge base-toepassing die voorspelt dat de mogelijke oorzaken van klep van fouten.| Liften zijn investering investeringen voor mogelijk een 20-30-jaar geldig. Zodat elke potentiële verkoop uiterst concurrerende; verwachtingen voor de service en ondersteuning zijn daarom hoog. Voorspeld onderhoud kan leveren deze bedrijven met een voordeel ten opzichte van hun concurrenten in hun product en serviceaanbiedingen.|
|_Fouten steeds_: steeds fouten account voor de helft van alle ontsporingen trainen en miljarden bedrijfstak van de globale spoor kosten. Wheel fouten veroorzaken ook rails verslechteren, soms veroorzaakt de spoor worden voortijdig wordt afgebroken. Spoor einden leiden tot onherstelbare gebeurtenissen zoals ontsporingen. Om te voorkomen dat dergelijke gevallen, spoor de prestaties van wheels controleren en ze vervangen in een preventieve manier. Het zakelijke probleem is de voorspelling van wheel-fouten.| Voorspeld onderhoud van wheels helpt met just in time wheels te vervangen |
|_Trein train deur fouten_: een belangrijke reden voor vertragingen in de trein operations deur fouten train auto is. Het zakelijke probleem is om te voorspellen train deur fouten.|Vroege bewust te maken van een storing deur of het aantal dagen tot een storing deur helpt de zakelijke optimaliseren trainen deur gepland onderhoud.|

De volgende sectie opgehaald in de gedetailleerde informatie over de voordelen van de PdM hierboven besproken.

## <a name="data-science-for-predictive-maintenance"></a>Gegevenswetenschap voor voorspeld onderhoud

Deze sectie bevat algemene richtlijnen beschreven die van de principes van de wetenschap en praktijken voor PdM. Het is bedoeld om u te helpen een TDM, oplossing systeemarchitect of een ontwikkelaar de vereisten en het proces voor het bouwen van end-to-end AI-toepassingen voor PdM begrijpen. U kunt in deze sectie samen met een overzicht van de demo's lezen en bewijs van concept sjablonen vermeld in [sjablonen van de oplossing voor voorspeld onderhoud](#Solution-templates-for-predictive-maintenance). Vervolgens kunt u deze principes en aanbevelingen voor het implementeren van uw oplossing PdM in Azure.

> [!NOTE]
> Deze handleiding is niet bedoeld om te leren van de lezer wetenschappelijke gegevens. Diverse nuttige bronnen zijn beschikbaar voor Lees hier meer over in de sectie voor [lesmateriaal voor voorspeld onderhoud](#Training-resources-for-predictive-maintenance). De [oplossingssjablonen](#Solution-templates-for-predictive-maintenance) die worden vermeld in de handleiding illustratie van deze AI technieken voor specifieke PdM problemen.

## <a name="data-requirements-for-predictive-maintenance"></a>Eisen voor voorspeld onderhoud

Het succes van eventuele leren, is afhankelijk van (a) de kwaliteit van wat is wordt geleerd en (b) de mogelijkheid van de cursist. Voorspellende modellen leert patronen van historische gegevens en het voorspellen van toekomstige resultaten met bepaalde kans op basis van deze patronen waargenomen. Een model voorspellende nauwkeurigheid is afhankelijk van de relevantie, toereikend waren en kwaliteit van de trainings- en -gegevens. De nieuwe gegevens die wordt 'berekend' met behulp van dit model moet dezelfde functies en schema hebben als de gegevens van de training en testen. De functie-kenmerken (type, dichtheid, distributie, enzovoort) van de nieuwe gegevens moeten overeenkomen met die van de trainings- en gegevenssets. De focus van deze sectie is van de behoeften van dergelijke gegevens.

### <a name="relevant-data"></a>Relevante gegevens

Eerst moet de gegevens worden _relevant zijn voor het probleem_. Houd rekening met de _fout steeds_ gebruiksvoorbeeld besproken hoger - de trainingsgegevens functies met betrekking tot de wheel-bewerkingen moet bevatten. Als het probleem te voorspellen, het uitvallen van de _tractiesysteem_, de trainingsgegevens heeft tot de verschillende onderdelen voor het tractiesysteem omvatten. Het eerste geval is bedoeld voor een specifiek onderdeel dat gericht is op het tweede geval het uitvallen van een grotere subsysteem. De algemene aanbeveling is voor het ontwerpen van systemen voorspelling over specifieke onderdelen in plaats van grotere subsystemen sinds de laatste wordt verspreid zijn, meer gegevens. De expert domein (Zie [in aanmerking komende problemen voor voorspeld onderhoud](#Qualifying-problems-for-predictive-maintenance)) kunnen helpen bij het selecteren van de meest relevante subsets van gegevens voor de analyse. De relevante gegevensbronnen worden besproken in uitgebreider [gegevens voorbereiden voor voorspeld onderhoud](#Data-preparation-for-predictive-maintenance).

### <a name="sufficient-data"></a>Voldoende gegevens
Twee vragen veelgestelde met betrekking tot de gegevens van geschiedenis van fout: (1) 'hoeveel mislukte gebeurtenissen zijn vereist voor het model trainen?' (2) 'het aantal records wordt beschouwd als 'voldoende'?' Er zijn geen nauwkeurige antwoorden, maar alleen regels van miniatuur. Meer het aantal mislukte gebeurtenissen, betere voor (1), het model. Voor (2), en het exacte aantal mislukte gebeurtenissen is afhankelijk van de gegevens en de context van het probleem wordt opgelost. Maar aan de kant spiegelen als een machine te vaak mislukt vervolgens het bedrijf wordt vervangen, waarmee exemplaren van de fout wordt beperkt. Hier ook is de richtlijnen van het domein deskundige belangrijk. Er zijn echter methoden omgaan met de uitgifte van _zeldzame gebeurtenissen_. Ze worden besproken in het gedeelte [imbalanced gegevens verwerken](#Handling-imbalanced-data).

### <a name="quality-data"></a>Quality-gegevens
De kwaliteit van de gegevens is essentieel - de kenmerkwaarde voor elke manier moet _nauwkeurige_ in combinatie met de waarde van de doelvariabele. Gegevenskwaliteit is een goed studied gebied in beveiligingsstatistieken en -gegevens management en daarom buiten het bereik voor deze handleiding.

> [!NOTE]
> Er zijn verschillende bronnen en enterprise-producten om kwaliteitsgegevens te bezorgen. Hieronder vindt u een voorbeeld van verwijzingen:
> - Dasu, T, Johnson, T., experimentele gegevensanalyse en gegevens reinigen, Wiley, 2003.
> - [Experimentele Data-analyse, Wikipedia (Engelstalig)](https://en.wikipedia.org/wiki/Exploratory_data_analysis)
> - [Hellerstein, J, kwantitatieve gegevens gereinigd voor grote Databases](http://db.cs.berkeley.edu/jmh/papers/cleaning-unece.pdf)
> - [de Jonge, E, van der loo, M, inleiding tot de gegevens met R reinigen](https://cran.r-project.org/doc/contrib/de_Jonge+van_der_Loo-Introduction_to_data_cleaning_with_R.pdf)

## <a name="data-preparation-for-predictive-maintenance"></a>Voorbereiden van gegevens voor voorspeld onderhoud

### <a name="data-sources"></a>Gegevensbronnen

De relevante gegevensbronnen voor voorspeld onderhoud omvatten, maar zijn niet beperkt tot:
- Fout-geschiedenis
- Geschiedenis van onderhoud of herstellen
- De omstandigheden machine
- Apparatuur metagegevens

#### <a name="failure-history"></a>Fout-geschiedenis
Mislukte gebeurtenissen zijn vaak voorkomen in PdM toepassingen. Tijdens het bouwen van voorspellende modellen, moet het algoritme moet echter voor meer informatie over een onderdeel van de normale operationele patroon, evenals de patronen is mislukt. De trainingsgegevens moet dus een voldoende aantal voorbeelden van beide categorieën bevatten. Geschiedenis van onderhoudsmodus records en delen vervanging zijn goede bronnen om te zoeken die is mislukt. Met behulp van enige kennis van het domein, kunnen ook afwijkingen in de trainingsgegevens worden gedefinieerd als codefouten.

#### <a name="maintenancerepair-history"></a>Geschiedenis van onderhoud of herstellen
Geschiedenis van het onderhoud van een activum bevat details over de onderdelen vervangen, herstelactiviteiten enzovoort worden uitgevoerd. Deze gebeurtenissen opnemen vermindering patronen. Gebrek aan deze essentiële gegevens in de trainingsgegevens kan leiden tot misleidende model resultaten. Geschiedenis van de fout kan ook binnen onderhoud geschiedenis als speciale foutcodes of volgorde datums voor onderdelen die worden gevonden. Extra gegevensbronnen die invloed hebben op patronen fout moeten worden onderzocht en geleverd door experts domein.

#### <a name="machine-operating-conditions"></a>De omstandigheden machine
Op basis (of andere) streaming sensorgegevens van de apparatuur in bewerking is een belangrijke gegevensbron. Een sleutel veronderstelling in PdM is dat de status van een machine na verloop van tijd tijdens het dagelijkse beheer vermindert. De gegevens wordt verwacht tijd verschillende functies die dit patroon veroudering en eventuele afwijkingen die leidt tot een vermindering vastleggen bevatten. De tijdelijke aspect van de gegevens is vereist voor de algoritme voor meer informatie over de fout en zonder fouten patronen gedurende een bepaalde periode. Op basis van deze gegevenspunten, leert het algoritme te voorspellen hoeveel meer tijdseenheden een machine kunt blijven werken voordat deze is mislukt.

#### <a name="static-feature-data"></a>Gegevens van de statische functie
Statische functies zijn metagegevens over de apparatuur. Voorbeelden zijn het merk apparatuur, model, productiedatum, start de service, de locatie van het systeem en andere technische specificaties.

Voorbeelden van relevante gegevens voor de [voorbeeld PdM gebruiksvoorbeelden](#Sample-PdM-use-cases) zijn Zie tabel hieronder:

| Gebruiksvoorbeeld | Voorbeelden van relevante gegevens |
|:---------|---------------------------|
|_Vlucht vertraging en annuleringen_ | Vlucht route-informatie in de vorm van overdracht zijden en logboeken van de pagina. Arm vluchtgegevens bevat routering details zoals vertrek/aankomst datum, tijd, luchthaven, layovers enzovoort. Pagina logboek bevat een reeks met de fout en het onderhoud codes vastgelegd door de medewerkers van de grond onderhoud.|
|_Vliegtuig motoren delen ontstaan_ | Gegevens verzameld van sensoren in het vliegtuig die informatie over de conditie van de verschillende onderdelen geven. Onderhoudsrecords te identificeren wanneer onderdeel fouten zijn opgetreden en wanneer ze zijn vervangen.|
|_ATM-fout_ | Sensormetingen voor elke transactie (nederlegging geld/selectievakje) en aflevering geld. Gegevens op de meting van de tussenruimte tussen notities, houd er rekening mee dikte, houd er rekening mee aankomst afstand, controleert u kenmerken, enzovoort. Onderhoudsrecords waarmee de laatste tijd de dispenser geld foutcodes, reparatie-informatie is aangevuld.|
|_O turbine mislukt_ | Sensoren bewaken turbine voorwaarden zoals temperatuur, o richting, power gegenereerd, generator snelheid, enzovoort. Gegevens zijn verzameld vanaf meerdere Windturbines van o farms zich in verschillende regio's. Elke turbine hebben doorgaans, meerdere sensormetingen metingen op een vast tijdsinterval doorgeven.|
|_Fouten Circuitonderbreker_ | Onderhoud logboeken die corrigerende preventieve en systematische acties opnemen. Operationele gegevens met automatische en handmatige opdrachten verzonden naar stroomonderbrekers zoals voor acties openen en sluiten. De metagegevens van apparaten zoals datum van vervaardiging, locatie, model, enzovoort. Circuitonderbreker specificaties zoals spanning niveaus, geolocatie, ambient voorwaarden.|
|_Lift deur fouten_| Lift-metagegevens zoals type lift, productiedatum, onderhoud frequentie, gebouw type, enzovoort. Operationele informatie zoals het aantal cycli deur, gemiddelde deur sluiten tijd. De geschiedenis van de fout met oorzaken.|
|_Wheel-fouten_ | Sensorgegevens dat metingen steeds versnelling, geregeld exemplaren, aangedreven afstand, snelheid, enzovoort. Statische gegevens op wheels zoals fabrikant geproduceerd datum. Fout gegevens afgeleid van onderdeel volgorde database die volgorde datums en hoeveelheden bijhouden.|
|_Trein train deur fouten_ | Deur openen en sluiten tijdstippen, andere operationele gegevens zoals de huidige toestand van de trein deuren. Statische gegevens omvat activa-id, de tijd en de voorwaarde waarde kolommen.|

### <a name="data-types"></a>Gegevenstypen
De bovenstaande gegevensbronnen gegeven, zijn de twee belangrijkste gegevenstypen waargenomen in PdM domein:

- _Tijdelijke gegevens_: operationele telemetrie, machine voorwaarden, werk volgorde typen, prioriteitscodes die op het moment van opname tijdstempels hebben. Fout, onderhoud/repair en de gebruiksgeschiedenis heeft ook tijdstempels die zijn gekoppeld aan elke gebeurtenis.
- _Statische gegevens_: Machine functies en onderdelen van de operator in het algemeen zijn statisch omdat hiermee de technische specificaties van machines of operator kenmerken beschreven. Als deze functies na verloop van tijd wijzigt kunnen, moeten ze ook tijdstempels gekoppeld hebben.

Variabelen manier en doel moeten worden voorverwerkt/omgezet in [numerieke categorische en andere gegevenstypen](https://www.statsdirect.com/help/basics/measurement_scales.htm) , afhankelijk van de algoritme die wordt gebruikt.

### <a name="data-preprocessing"></a>Gegevens voorverwerken
Als een vereiste voor het _functie-engineering_, de gegevens uit verschillende stromen voor het opstellen van een schema van waaruit het is gemakkelijk te ontwikkelen die functies voorbereiden. De gegevens eerst visualiseren als een tabel met records. Elke rij in de tabel vertegenwoordigt een exemplaar van de training, en de kolommen vertegenwoordigen _manier_ functies (ook wel onafhankelijke kenmerken of variabelen). Organiseer de gegevens zodat de laatste kolom of kolommen is de _doel_ (afhankelijke variabele). Voor elk exemplaar training toewijzen een _label_ als de waarde van deze kolom.

Voor tijdelijke gegevens, de duur van sensorgegevens in tijdseenheden te verdelen. Elke record moet behoren tot een tijdseenheid voor een asset _en moet bieden verschillende informatie_. Tijdseenheden worden gedefinieerd op basis van zakelijke behoeften in veelvouden van seconden, minuten, uren, dagen, maanden, enzovoort. De tijdseenheid _heeft niet dezelfde zijn als de frequentie van verzamelen van gegevens_. Als de frequentie hoog is, kan de gegevens een aanzienlijk verschil van de ene eenheid naar de andere niet weergeven. Stel bijvoorbeeld dat temperatuur elke 10 seconden is verzameld. Met datzelfde interval voor trainingsgegevens alleen wordt vergroot het aantal voorbeelden zonder eventuele aanvullende informatie. Voor deze aanvraag heeft een strategie voor een beter kan worden gebruikt de gegevens die meer dan 10 minuten of een uur op basis van de zakelijke rechtvaardiging gemiddelde.

Voor statische gegevens
- _Onderhoudsrecords_: onderhoud onbewerkte gegevens heeft een activa-id en de tijdstempel van informatie over onderhoudsactiviteiten die zijn uitgevoerd op een bepaald tijdstip. Transformeren onderhoudsactiviteiten in _categorische_ kolommen, waarbij elke categorie descriptor uniek wordt toegewezen aan een specifieke onderhoudsactie. Het schema voor onderhoudsrecords omvat activa-id, de tijd en de bewerking voor siteonderhoud.

- _Fout records_: fouten of oorzaken voor de fout kunnen worden vastgelegd als specifieke foutcodes of mislukte gebeurtenissen die zijn gedefinieerd door specifieke zakelijke voorwaarden. In gevallen waar de apparatuur meerdere foutcodes heeft, het domein deskundige moet te identificeren die betrekking op de doelvariabele hebben. De resterende foutcodes of voorwaarden gebruiken om samen te stellen _manier_ functies die met deze fouten samenhangen. Het schema voor registraties van mislukte omvat activa-id, tijd, mislukt of reden voor fout - indien beschikbaar.

- _Metagegevens van de computer en de operator_: de computer en de operator gegevens samenvoegen in één schema een asset koppelen aan de operator, samen met hun respectieve kenmerken. Het schema voor de machine voorwaarden omvat activa-id, asset-functies, operator-id en functies van de operator.

Andere gegevens dat voorverwerking stappen omvatten _afhandeling van de ontbrekende waarden_ en _normalisatie_ van kenmerkwaarden. Een gedetailleerde discussie valt buiten het bereik van deze handleiding: Zie de volgende sectie voor sommige nuttig verwijzingen.

Met de bovenstaande voorverwerkte gegevensbronnen aanwezig is, de laatste transformatie voordat de functie-engineering lid worden van de bovenstaande tabellen op basis van de activa-id en het tijdstempel is. De resulterende tabel moet null-waarden voor de kolom is mislukt als de machine zich in de normale werking. Null-waarden kunnen worden toegeschreven met een indicator voor een normale werking. Deze fout-kolom gebruiken om te maken _labels voor het voorspellende model_. Zie voor meer informatie het gedeelte over [modelleren technieken voor voorspeld onderhoud](#Modeling-techniques-for-predictive-maintenance).

## <a name="feature-engineering"></a>Functie-engineering
Functie-engineering is de eerste stap voor het modelleren van de gegevens. De functie in het proces van de wetenschappelijke gegevens [Hier wordt beschreven](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/create-features). Een _functie_ is een Voorspellend kenmerk voor het model - zoals temperatuur, druk en trillingen. Voor PdM omvat het functie-engineering status van een machine via de historische gegevens verzameld over de duur van een aanzienlijke wordt onttrokken. In dit opzicht is verschilt het van de peers zoals externe controle afwijkingsdetectie en foutdetectie. 

### <a name="time-windows"></a>Tijdvensters
Externe controle houdt de gebeurtenissen die zich als van voordoen reporting _tijdstippen_. Afwijkingsdetectie detectie modellen evalueren (score) inkomende gegevensstromen naar vlag afwijkingen vanaf punten in de tijd. Foutdetectie classificeert fouten zijn van specifieke typen wanneer deze zich punten in de tijd voordoen. Daarentegen PdM omvat het voorspellen van fouten via een _toekomstige periode_, op basis van functies die het gedrag van de machine via vertegenwoordigen _historische periode_. Voor PdM zijn de gegevens van de functie van afzonderlijke punten tijd ruis worden voorspeld. Daarom moet de gegevens voor elke functie _smoothened_ door gegevenspunten via tijdvensters samen te voegen.

### <a name="lag-features"></a>Functies lag
De zakelijke vereisten definiëren hoe ver het model heeft om te voorspellen in de toekomst. Op zijn beurt deze duur kunt u beter bepalen hoe ver terug het model, heeft om te zoeken' voor het maken van deze voorspellingen. Deze 'opzoeken terug' periode heet de _lag_, en gedurende deze periode lag engineering functies worden aangeroepen _functies lag_. Deze sectie wordt beschreven lag functies die kunnen worden samengesteld uit gegevensbronnen met tijdstempels en het maken van de functie van statische gegevensbronnen. Lag onderdelen zijn meestal _numerieke_ aard.

> [!IMPORTANT]
> Grootte van het venster wordt bepaald via experimenteren en met behulp van een domein deskundige moet worden voltooid. Hoewel dezelfde bevat voor de selectie en de definitie van de functies lag, hun aggregaties en het type van windows.

#### <a name="rolling-aggregates"></a>Rolling statistische functies
Voor elke record van een activum, een rolling-venster met een grootte van "de letter W" gekozen als het aantal tijdseenheden voor het berekenen van de statistische functies. Vertraging van functies worden vervolgens berekend met behulp van de perioden W _vóór de datum_ van dat record. De blauwe regels worden weergegeven in afbeelding 1 sensorwaarden voor een asset voor elke eenheid is vastgelegd. Ze geven een oplopende gemiddelde van waarden van de functie via een venster met een grootte W = 3. Het oplopende gemiddelde wordt berekend via alle records met tijdstempels in het bereik t<sub>1</sub> (in oranje) aan t<sub>2</sub> (in groen). De waarde voor W is doorgaans in minuten of uren, afhankelijk van de aard van de gegevens. Maar voor sommige problemen, verzamelen van een grote W (spreek 12 maanden) bieden de hele geschiedenis van een actief totdat de tijd van de record.

![Afbeelding 1. Statistische functies rolling](./media/cortana-analytics-playbook-predictive-maintenance/rolling-aggregate-features.png) afbeelding 1. Statistische functies rolling

Voorbeelden van statistische functies via een tijdvenster rolling zijn aantal, gemiddelde, metingen CUMESUM (cumulatieve som), minimale/maximale waarden. Bovendien worden Variantie standaardafwijking en aantal uitschieters buiten N standaardafwijkingen vaak gebruikt. Voorbeelden van statistische functies die kunnen worden toegepast voor de [gebruiksvoorbeelden](#Sample-PdM-use-cases) in deze handleiding worden hieronder weergegeven. 
- _Vertraging Flight_: aantal foutcodes gedurende de laatste dag/week.
- _Vliegtuig engine onderdeel fout_: rolling middelen en standaarddeviatie som gedurende de afgelopen dag, week, enzovoort. Met deze metriek moet samen met het domein zakelijke deskundigen worden bepaald.
- _ATM-fouten_: rolling manier, Mediaan, bereik, standaardafwijkingen, aantal uitschieters dan drie standaardafwijkingen, hogere en lagere CUMESUM.
- _Trein train deur fouten_: aantal gebeurtenissen gedurende de afgelopen dag, week, twee weken enzovoort.
- _Circuitonderbreker fouten_: fout in afgelopen week, jaar, drie jaar enzovoort telt.

Een andere nuttige techniek in PdM is trend wijzigingen, pieken en verandert met algoritmen die afwijkingen gedetecteerd in de gegevens vast te leggen.

#### <a name="tumbling-aggregates"></a>Daling statistische functies
Label voor elke record van een asset, een venster van de grootte van _W -<sub>k</sub>_  is gedefinieerd, waarbij _k_ is het aantal vensters van grootte _W_. Statistische functies vervolgens worden gemaakt via _k_ _daling windows_ _W-k, W -<sub>(k-1)</sub>,..., W -<sub>2</sub>, W -<sub>1</sub>_  voor de perioden voordat de tijdstempel van een record. _k_ mag een klein aantal om vast te leggen op korte termijn effecten zijn of een groot aantal om vast te leggen op lange termijn vermindering patronen. (Zie afbeelding 2).

![Afbeelding 2. Statistische functies daling](./media/cortana-analytics-playbook-predictive-maintenance/tumbling-aggregate-features.png) afbeelding 2. Statistische functies tumbling

Bijvoorbeeld: lag functies voor het geval van o turbines gebruik kan worden gemaakt met W = 1 en k = 3. Ze impliceren de vertraging voor elk van de afgelopen drie maanden met boven- en uitschieters.

### <a name="static-features"></a>Statische functies

Technische specificaties van de apparatuur zoals datum van vervaardiging, modelnummer, locatie, zijn enkele voorbeelden van statische functies. Ze worden behandeld als _categorische_ variabelen voor modellering. Enkele voorbeelden voor het gebruiksvoorbeeld Circuitonderbreker zijn spanning, huidige energiecapaciteit, transformatortype en stroombron. Voor wheel-fouten is het type band wheels (gelegeerd tegenover staal) een voorbeeld.

De gegevens voorbereiding inspanningen besproken tot nu toe mag leiden tot de gegevens worden geordend zoals hieronder wordt weergegeven. Training, test en validatie van gegevens, moet deze logische schema (in dit voorbeeld laat zien in eenheden van dagen) hebben.

| Activa-id | Tijd | <Feature Columns> | Label |
| ---- | ---- | --- | --- |
| A123 |1 dag | . . . | . |
| A123 |Dag 2 | . . . | . |
| ...  |...   | . . . | . |
| B234 |1 dag | . . . | . |
| B234 |Dag 2 | . . . | . |
| ...  |...   | . . . | . |

De laatste stap in de functie-engineering is de **labels** van de doelvariabele. Dit proces is afhankelijk van de techniek modelleren. Op zijn beurt afhankelijk de techniek modellering van het zakelijke probleem en de aard van de beschikbare gegevens. Labels wordt in de volgende sectie besproken.

> [!IMPORTANT]
> Voorbereiden van gegevens en functie-engineering zijn belangrijk is als het modelleren van technieken voor geslaagde PdM oplossingen aankomen. De expert domein en de vanuit de praktijk moeten investeren veel tijd in die binnenkomen bij de juiste functies en gegevens voor het model. Een kort voorbeeld veel books op functie-engineering, worden hieronder weergegeven:
> - Pyle, D. gegevens voorbereiden voor gegevensanalyse (de Morgan Kaufmann reeksen in beheersystemen gegevens), dat 1999
> - Zheng, A., Casari, A. functie-Engineering voor Machine Learning: beginselen en technieken voor Gegevenswetenschappers, O'Reilly, 2018.
> - Dong, G. Liu, H. (Editors) functie Engineering voor Machine Learning en gegevensanalyse (Chapman & Hall/CRC gegevensanalyse en kennis detectie reeks) CRC Press, 2018.

## <a name="modeling-techniques-for-predictive-maintenance"></a>Modelleringstechnieken voor voorspeld onderhoud

Deze sectie worden de belangrijkste modelleringstechnieken voor PdM problemen, samen met hun constructie specifiek label methoden behandeld. U ziet dat een techniek voor één model kan worden gebruikt in verschillende branches. De model-techniek is gekoppeld aan het wetenschappelijke gegevensprobleem in plaats van de context van de gegevens bij de hand.

> [!IMPORTANT]
> De keuze van de labels voor de gevallen mislukt en de strategie voor de labels  
> moet in overleg met het domein deskundige worden bepaald.

### <a name="binary-classification"></a>Binaire classificatie
Binaire classificatie wordt gebruikt voor _de kans dat een apparaat is mislukt binnen een toekomstige periode te voorspellen_ - opgeroepen de _toekomstige horizon periode X_. X wordt bepaald door het zakelijke probleem en de gegevens bij de hand in overleg met het domein deskundige. Voorbeelden zijn:
- _minimale doorlooptijd_ vereist voor het vervangen van onderdelen, onderhoud resources implementeren, uitvoeren van onderhoud om een probleem dat zeer waarschijnlijk zal optreden in die periode te voorkomen.
- _minimum aantal gebeurtenissen dat_ dat kan gebeuren voordat er een probleem optreedt.

In deze techniek wordt twee soorten training voorbeelden aangegeven. Voorbeeld van een positieve _waarmee wordt aangegeven met een fout_, met het label = 1. Een negatieve voorbeeld, dat aangeeft dat de normale bewerkingen, met het label = 0. De doelvariabele, en die dus de labelwaarden _categorische_. Het model moet elke nieuwe voorbeeld kan mislukken of normaal werken via de volgende X tijdseenheden identificatie.

#### <a name="label-construction-for-binary-classification"></a>Label constructie voor binaire classificatie
De vraag hier is: ' Wat is de kans dat de asset in de volgende mislukt X tijdseenheden? ' Om deze vraag, label X records vóór de fout van een actief als 'ongeveer mislukt' te beantwoorden (label = 1), en andere records als 'normale' een label (label = 0). (Zie afbeelding 3).

![Afbeelding 3. Labels voor binaire classificatie](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-binary-classification.png) afbeelding 3. Labels voor binaire classificatie

Hieronder vindt u voorbeelden van de strategie voor enkele van de gebruiksvoorbeelden labels.
- _Vertragingen Flight_: X 1 dag kan worden gekozen voor het voorspellen van vertragingen in de volgende 24 uur. Vervolgens alle vlucht die binnen 24 uur voordat fouten worden aangeduid als 1.
- _ATM-geld afgezien fouten_: een doel is mogelijk om de kans storing van een transactie in het volgende één uur te bepalen. In dat geval worden alle transacties die hebben plaatsgevonden in het afgelopen uur van de fout zijn gelabeld als 1. Als u wilt voorspellen kans op mislukte via de volgende N valuta worden vrijgesteld, notities alle notities vrijgesteld binnen de laatste N-opmerkingen van een fout aangeduid als 1.
- _Circuitonderbreker fouten_: het doel is mogelijk om te voorspellen van de volgende fout voor Circuitonderbreker-opdracht. In dat geval is X gekozen als één toekomstige opdracht.
- _Deur fouten trainen_: X kan worden gekozen als twee dagen.
- _Turbine fouten liquidatie_: X kan worden gekozen als twee maanden.

### <a name="regression-for-predictive-maintenance"></a>Regressie voor voorspeld onderhoud
Regressie modellen worden gebruikt om _berekenen van de resterende levensduur (resterende Levensduur) van een asset_. Resterende Levensduur is gedefinieerd als de hoeveelheid tijd die een asset functioneert voordat u de volgende fout zich voordoet. Elke training-voorbeeld is een record die bij een tijdseenheid hoort _nY_ voor een asset waar _n_ is het veelvoud. De resterende Levensduur van elke nieuwe voorbeeld als moet worden berekend door het model een _doorlopend nummer_. Dit aantal geeft aan dat de periode resterende vóór de fout.

#### <a name="label-construction-for-regression"></a>Label constructie voor regressie
De vraag hier is: 'Wat is de resterende levensduur (resterende Levensduur) van de apparatuur?' Voor elke record vóór de fout het label voor het aantal eenheden van vóór de volgende fout resterende tijd te berekenen. Labels zijn bij deze methode Continue variabelen. (Zie afbeelding 4)

![Afbeelding 4. Labels voor regressie](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-regression.png) afbeelding 4. Labels voor regressie

Voor regressie, labels gebeurt met een verwijzing naar een punt van mislukken. De berekening is niet mogelijk zonder te weten hoe lang de asset is het bedrijf overleven voordat er een fout. Dus worden daarentegen binaire indeling, activa zonder fouten in de gegevens niet gebruikt voor het model. Dit probleem beste wordt verholpen door een andere statistische methode aangeroepen [Survival Analysis](https://en.wikipedia.org/wiki/Survival_analysis). Maar potentiële problemen kunnen optreden wanneer deze methode wordt toegepast op PdM gebruiksvoorbeelden die betrekking hebben op tijd wisselende gegevens met regelmatige intervallen. Zie voor meer informatie over Survival Analysis [dit een pager](https://www.cscu.cornell.edu/news/statnews/stnews78.pdf).

### <a name="multi-class-classification-for-predictive-maintenance"></a>Meerdere klasse-classificatie voor voorspeld onderhoud
Meerdere klasse classificatie technieken kunnen worden gebruikt in PdM oplossingen voor twee scenario's:
- Voorspellen _twee toekomstige resultaten_: het eerste resultaat is _een tijdsbereik voor fouten_ voor een asset. De asset is toegewezen aan een van meerdere mogelijke perioden. Het tweede resultaat is de kans op fouten in een toekomstige periode vanwege _een van de meerdere hoofdmap zorgt ervoor dat_. Deze voorspelling kan de bemanning onderhoud moeten worden gecontroleerd symptomen en plan onderhoudsschema's.
- Voorspellen _het meest waarschijnlijke oorzaak_ van een opgegeven fout. Dit resultaat raadt aan om de juiste set met onderhoudsacties om op te lossen, een fout. Een gerangschikte lijst met hoofdoorzaken en aanbevolen reparaties kunt technici prioriteren van hun acties voor herstellen na een storing.

#### <a name="label-construction-for-multi-class-classification"></a>Label constructie voor meerdere klasse-classificatie
De vraag hier is: ' Wat is de kans dat een asset in de volgende mislukt _nZ_ tijdseenheden waar _n_ is het aantal perioden? ' Als u wilt deze vraag te beantwoorden, labelen nZ records vóór de fout van een activum met behulp van buckets tijd (3Z 2Z, Z). Label alle andere registreert 'normale' (label = 0). Bij deze methode wordt de doelvariabele bevat _categorische_ waarden. (Zie afbeelding 5).

![Afbeelding 5. Labels voor multiklassen classificatie voor fout tijdreeksvoorspelling](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-failure-time-prediction.png) afbeelding 5. Labels voor classificatie van meerdere klasse voor fout tijdreeksvoorspelling

De vraag hier is: ' Wat is de kans dat de asset in de volgende mislukt X tijdseenheden vanwege hoofdprobleem oorzaak _P<sub>ik</sub>_? " waar _ik_ is het aantal mogelijke oorzaken. Om deze vraag, label X records vóór de fout van een actief als beantwoorden ' bijna mislukken vanwege hoofdoorzaak _P<sub>ik</sub>_' (label = _P<sub>ik</sub>_). Label alle records als 'normale' (label = 0). Bij deze methode ook zijn labels categorische (Zie afbeelding 6).

![Afbeelding 6. Labels voor multiklassen classificatie voor de hoofdmap oorzaak prognose](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-root-cause-prediction.png) afbeelding 6. Labels voor classificatie voor de hoofdmap oorzaak prognose meerdere klasse

Het model wijst een kans is mislukt als gevolg van elk _P<sub>ik</sub>_  en de kans op fouten. Deze kansen kunnen worden besteld met grootte om toe te staan van de voorspelling van de problemen die kunnen optreden in de toekomst zijn.

De vraag hier is: 'welke onderhoudsacties u kunt beter na een storing?' Deze vraag te beantwoorden labels _vereist niet dat een toekomstige horizon moeten worden verzameld_, omdat de fout is niet in de toekomst voorspellen van het model. Het is net voorspellen van het meest waarschijnlijke oorzaak _zodra de storing er al is gebeurd_.

## <a name="training-validation-and-testing-methods-for-predictive-maintenance"></a>Training, validatie en testen methoden voor voorspeld onderhoud
De [Team gegevens wetenschap proces](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) biedt een uitgebreid overzicht van de cyclus model train test valideren. Deze sectie wordt beschreven aspecten die uniek is voor PdM.

### <a name="cross-validation"></a>Cross-validatie
Het doel van [cross-validatie](https://en.wikipedia.org/wiki/Cross-validation_(statistics)) definiëren van een gegevensset aan 'test' het model in de fase training. Deze gegevensset heet de _validatie set_. Deze techniek helpt limiet problemen zoals _overfitting_ en biedt een inzicht in hoe het model wordt generalize met een onafhankelijke gegevensset. Dat wil zeggen, een onbekende gegevens ingesteld die kan worden vanuit een echte probleem. De routine training en testen voor PdM moet rekening houden met de verschillende aspecten van de tijd beter generaliseren op verborgen toekomstige gegevens.

Groot aantal machine learning-algoritmen is afhankelijk van een aantal hyperparameters die de model-prestaties aanzienlijk kunt wijzigen. De optimale waarden van deze hyperparameters worden niet automatisch worden berekend wanneer het model te trainen. Ze moeten worden opgegeven met het wetenschappelijk gegevens. Er zijn verschillende manieren om goede waarden van hyperparameters te vinden.

De meest voorkomende een is _k tweevoudige kruisvalidatie_ die in de voorbeelden willekeurig in splitst _k_ vouwen. Voor elke set van hyperparameters waarden, voert u het leeralgoritme _k_ tijden. Bij elke iteratie gebruiken in de voorbeelden in de huidige Vouw als een set validatie en de rest van de voorbeelden als een trainingset. Het algoritme trainen via training voorbeelden en de maatstaven voor prestaties via validatie voorbeelden berekenen. Aan het einde van deze lus compute het gemiddelde van _k_ maatstaven voor prestaties. Kies de waarden die u de beste gemiddelde prestaties hebt voor elke set van hyperparameter waarden. De taak van het kiezen van hyperparameters is vaak experimentele aard.

Gegevens worden geregistreerd in PdM problemen, als een tijdreeks van gebeurtenissen die afkomstig van verschillende gegevensbronnen zijn. Deze records kunnen worden besteld volgens de tijd van labels. Daarom als de dataset is gesplitst _willekeurig_ in trainings- en validatie-set _sommige van de voorbeelden training zijn in de tijd later zijn dan enkele voorbeelden van validatie_. Toekomstige prestaties van hyperparameter waarden worden geschat op basis van bepaalde gegevens dat is ontvangen _voordat_ model is getraind. Deze schattingen mogelijk overmatig optimistische, vooral als de tijdreeks niet stilstaan en zich verder gedurende een bepaalde periode ontwikkelen. Hierdoor kunnen de waarden van de gekozen hyperparameter suboptimale zijn.

De aanbevolen manier is de voorbeelden splitsen in trainings- en validatie ingesteld in een _tijdsafhankelijke_ manier, waarbij alle validatie voorbeelden later moment dan alle training voorbeelden zijn. Training van het algoritme via de set trainingsgegevens voor elke set van hyperparameter waarden. Meten van het model prestaties ten opzichte van de set met dezelfde validatie. Kies hyperparameter waarden die de beste prestaties weergeven. Hyperparameter waarden wordt gekozen door train/validatie gesplitste resultaat in de prestaties beter toekomstige model dan met de gekozen willekeurig door kruisvalidatie waarden.

Het laatste model kan worden gegenereerd door een leeralgoritme training via gehele trainingsgegevens met behulp van de beste hyperparameter waarden.

### <a name="testing-for-model-performance"></a>Voor de model-prestaties testen
Als een model is gebouwd, is een schatting van de toekomstige prestaties op nieuwe gegevens vereist. Een goede schatting maken van de metriek prestaties van hyperparameter waarden berekend ten opzichte van de validatie-set of een metriek gemiddelde prestaties berekend op basis van kruisvalidatie. Deze schattingen zijn vaak overmatig optimistische. Het bedrijf wellicht vaak enkele aanvullende richtlijnen over hoe ze willen het model te testen.

De aanbevolen manier voor PdM is in de voorbeelden in training, validatie, splitsen en testgegevens wordt ingesteld in een _tijdsafhankelijke_ manier. Alle voorbeelden van de test moet later in de tijd dan alle trainings- en validatie voorbeelden. Na de splitsing het model is gegenereerd en de prestaties te meten, zoals eerder beschreven.

Als tijdreeks stilstaan en eenvoudig te voorspellen, genereren willekeurige en tijdsafhankelijke benaderingen vergelijkbare schattingen van toekomstige prestaties. Maar wanneer tijdreeks zijn niet stilstand en/of moeilijk te voorspellen, genereert de aanpak tijdsafhankelijke meer realistische maakt een schatting van toekomstige prestaties.

### <a name="time-dependent-split"></a>Tijdsafhankelijke splitsen
Deze sectie beschrijft aanbevolen procedures voor het implementeren van tijdsafhankelijke splitsen. Hieronder vindt u een wederzijdse splitsing tijdsafhankelijke tussen trainings- en testsets.

Stel dat een stream van gebeurtenissen zoals metingen van verschillende sensoren voorzien van een tijdstempel. Functies en labels van training en test voorbeelden via tijdschema die meerdere gebeurtenissen bevatten gedefinieerd. Bijvoorbeeld voor binaire classificatie op basis van gebeurtenissen in het verleden functies maken en labels op basis van toekomstige gebeurtenissen 'X' in de toekomst-eenheden maken (Zie de secties over [functie-engineering](#Feature-engineering) en [modelleren technieken](#Modeling-techniques-applied-to-PdM-use-cases)). Het labeling tijdsbestek van een voorbeeld komt dus hoger is dan het tijdsbestek van de functies.

Voor de splitsing tijdsafhankelijke, kies een _trainen van de ingestelde tijd T<sub>c</sub>_  waarmee een model te trainen met hyperparameters afgestemd met behulp van historische gegevens maximaal T<sub>c</sub>. Om te voorkomen dat lekken van toekomstige labels die buiten T<sub>c</sub> in de trainingsgegevens, kies de tijd van laatste label training voorbeelden moet X eenheden voordat T<sub>c</sub>. Elk vierkant vertegenwoordigt in het voorbeeld in afbeelding 7 wordt weergegeven, een record in de gegevensset waarop functies en labels worden berekend als hierboven beschreven. De afbeelding ziet de records die in trainings- en testdoeleinden sets voor X = 2 en W = 3 moeten gaan:

![Afbeelding 7. Tijdsafhankelijke splitsen voor binaire classificatie](./media/cortana-analytics-playbook-predictive-maintenance/time-dependent-split-for-binary-classification.png) afbeelding 7. Tijdsafhankelijke splitsen voor binaire classificatie

De groene kwadraten vertegenwoordigen records die horen bij de tijdseenheden die kunnen worden gebruikt voor training. Elk voorbeeld training wordt gegenereerd op basis van de afgelopen drie punten voor het genereren van de functie en twee toekomstige perioden voor het labelen voordat T<sub>c</sub>. Wanneer een deel van de twee punten in de toekomst ligt buiten T<sub>c</sub>, in dat geval uitsluiten van de set trainingsgegevens, omdat er geen zichtbaarheid wordt uitgegaan buiten T<sub>c</sub>.

De zwarte kwadraten vertegenwoordigen de records van de laatste gelabelde gegevensset die niet moet worden gebruikt in de set trainingsgegevens, krijgt de bovenstaande beperking. Deze records wordt ook niet worden gebruikt in het testen van gegevens, aangezien ze voordat T zijn<sub>c</sub>. Bovendien afhankelijk hun labeling tijdschema gedeeltelijk van het tijdsbestek training die is niet ideaal. Trainings- en gegevens moet afzonderlijke labeling tijdschema om te voorkomen dat gegevens worden gelekt label hebben.

De tot nu toe besproken techniek kunt u overlapping tussen trainings- en testdoeleinden voorbeelden die tijdstempels in de buurt T<sub>c</sub>. Een oplossing voor het bereiken van groter scheiding is moeten worden uitgesloten van de voorbeelden die binnen W tijdseenheden T<sub>c</sub> ingesteld van de test. Maar een agressieve gesplitste is afhankelijk van beschikbaarheid van voldoende gegevens.

Regressie modellen gebruikt voor het voorspellen van de resterende Levensduur meer ondervindt van het Lekkageprobleem. Via de methode willekeurige gesplitste leidt tot extreme te veel aanpassen. Regressie problemen, de splitsing moet zodanig zijn dat de records die horen bij activa met fouten voordat T<sub>c</sub> gaat u naar de trainingset. Records van activa met fouten na de afsluitdatum Ga in de testset.

Een andere aanbevolen procedure voor het splitsen van gegevens voor trainings- en testdoeleinden is het gebruik van een splitsing van asset-ID. De splitsing moet zodanig zijn dat geen van de activa die in de trainingset worden gebruikt in de prestaties van het model te testen. Met deze aanpak heeft een model een grotere kans om meer realistische resultaten voorzien van nieuwe activa.

### <a name="handling-imbalanced-data"></a>Verwerken van imbalanced gegevens
In de problemen met de classificatie, als er meer voorbeelden van een klasse dan van anderen, de gegevensset genoemd _imbalanced_. In het ideale geval voldoende vertegenwoordigers van elke klasse in de trainingsgegevens voorkeur onderscheid worden gemaakt tussen verschillende klassen inschakelen. Als een klasse minder dan 10% van de gegevens is, wordt de gegevens geacht imbalanced. De klasse underrepresented heet een _minderheid klasse_.

Veel PdM problemen hebben dergelijke imbalanced gegevenssets heeft, waarbij één klasse ernstig is underrepresented vergeleken met de andere klasse of klassen te maken. In sommige gevallen kan de klasse minderheid alleen 0,001% van de totale gegevenspunten vormen. Klasse onbalans is niet uniek is voor PdM. Andere domeinen waar storingen en afwijkingen zeldzame instanties zijn hebben een soortgelijk probleem, voor voorbeelden, fraude te detecteren en inbraakdetectie netwerk te maken. Deze fouten vormen in de voorbeelden van de klasse minderheid.

Met de klasse verstoord gegevens, prestaties van de meeste standaard learning-algoritmen ermee is geknoeid, omdat ze zijn bedoeld om u te minimaliseren, de frequentie van algemene fouten. Voor een gegevensset met 99% negatieve en positieve voorbeelden 1% kan een model voor 99% nauwkeurigheid hebben die door alle exemplaren negatief labels worden weergegeven. Maar het model wordt niet alle positieve voorbeelden; classificeren dus zelfs als de nauwkeurigheid hoog is, wordt de algoritme niet een handig is. Als gevolg daarvan kan conventionele evaluatie metrische gegevens zoals _algehele nauwkeurigheid van de frequentie van fouten_ ontoereikend zijn voor imbalanced learning. Als u bijvoorbeeld imbalanced gegevenssets, worden andere metrische gegevens worden gebruikt voor evaluatie van het model:
- Precisie
- Terughalen
- F1 scores
- Kosten aangepast ROC (ontvanger operationele kenmerken)

Zie voor meer informatie over deze metrische gegevens [model evaluatie](#Model-evaluation).

Er zijn echter enkele methoden die helpen bij het verhelpen van klasse onbalans probleem. De twee belangrijkste zijn _steekproef nemen technieken_ en _gevoelige learning kosten_.

#### <a name="sampling-methods"></a>Steekproeven methoden
Imbalanced learning omvat het gebruik van de methoden voor het wijzigen van de set trainingsgegevens aan een set met gelijke taakverdeling gegevens. Steekproeven methoden mogen niet worden toegepast op de testset. Er zijn verschillende steekproeven technieken, de meeste meteen forward zijn _willekeurige oversampling_ en _onder steekproeven_.

_Willekeurige oversampling_ omvat een steekproef van minderheidsbelang klasse selecteren en deze toevoegt aan een set trainingsgegevens repliceren van deze voorbeelden. Als gevolg daarvan kan het aantal voorbeelden in minderheid klasse is toegenomen en uiteindelijk een balans vinden tussen het aantal voorbeelden van verschillende klassen. Een nadeel van oversampling is dat de classificatie te te specifiek, wat leidt tot te veel aanpassing van labels kunnen leiden tot meerdere exemplaren van bepaalde voorbeelden. Het model nauwkeurig hoge training kan tonen, maar het is mogelijk dat de prestaties van verborgen testgegevens suboptimale.

Als u daarentegen _willekeurige onder steekproeven_ is een steekproef van een meerderheid klasse selecteren en deze voorbeelden verwijderen uit een set trainingsgegevens. Voorbeelden verwijderen uit het merendeel klasse kan echter de classificatie mogen worden belangrijke concepten met betrekking tot de meeste klasse veroorzaken. _Hybride steekproeven_ waarbij minderheid klasse te veel opgevangen en meerderheid klasse wordt onder-tegelijkertijd tijd is een andere levensvatbaar benadering.

Er zijn veel geavanceerde steekproeven technieken. De methode die is gekozen, is afhankelijk van de eigenschappen van gegevens en de resultaten van herhalende experimenten door het wetenschappelijk gegevens.

#### <a name="cost-sensitive-learning"></a>Gevoelige learning kosten
In PdM zijn fouten die deel uitmaken van de klasse minderheid van belang zijn meer dan de normale voorbeelden. De focus is daarom voornamelijk op de prestaties van de algoritme op fouten. Onjuist voorspellen van een positieve klasse als een negatieve klasse kost meer dan vice versa. Deze situatie wordt vaak aangeduid als ongelijke verlies of asymmetrische kosten van verkeerd classificeren elementen verschillende klassen. De ideale classificatie moet hoge nauwkeurigheid leveren via de klasse minderheid zonder boeten op de nauwkeurigheid van de meeste-klasse.

Er zijn meerdere manieren om dit evenwicht. Om te beperken van het probleem ongelijke verloren zijn gegaan, een hoge kosten foutieve indeling van de klasse minderheid toewijzen en probeer de totale kosten te minimaliseren. Algoritmen, zoals _SVMs (ondersteuning Vector Machines)_ vast van deze methode inherent, doordat de kosten van positieve en negatieve voorbeelden worden opgegeven tijdens de training. Op dezelfde manier zoals versterking methoden _boosted decision trees_ meestal goede prestaties met imbalanced gegevens weergeven.

## <a name="model-evaluation"></a>Model-evaluatie
Onjuiste indeling is een ernstig probleem voor PdM scenario's waarbij de kosten van het gegeven voor het bedrijf hoog is. Bijvoorbeeld een beslissing naar een vliegtuig op basis van een onjuist voorspelling van motoren ontstaan baan verstoren schema's en reizen plannen. Maken van een machine offline van een regel assembly kan leiden tot verlies van inkomsten. Evaluatie van het model met de juiste prestatiewaarden voor nieuwe testgegevens is daarom essentieel.

Typische prestatiewaarden gebruikt om te evalueren PdM modellen worden hieronder besproken:

- [Nauwkeurigheid](https://en.wikipedia.org/wiki/Accuracy_and_precision) is de populairste metrische gegevens die voor de beschrijving van de prestaties van een classificatie. Maar nauwkeurigheid gevoelig is voor gegevens distributies en is een inefficiënte meting voor scenario's met imbalanced gegevenssets. Andere metrische gegevens worden in plaats daarvan gebruikt. Hulpprogramma's zoals [verwarring matrix](https://en.wikipedia.org/wiki/Confusion_matrix) worden gebruikt voor het berekenen en reden over de nauwkeurigheid van het model.
- [Precisie](https://en.wikipedia.org/wiki/Precision_and_recall) van PdM modellen hebben betrekking op de frequentie van gegeven. Lagere precisie van het model is in het algemeen komt overeen met een hogere snelheid van gegeven.
- [Intrekken](https://en.wikipedia.org/wiki/Precision_and_recall) frequentie geeft aan hoeveel van de fouten in de testset 's juist geïdentificeerd door het model. Hogere intrekken tarieven betekent dat het model is mislukt bij het identificeren van de waarde true mislukt.
- [F1 score](https://en.wikipedia.org/wiki/F1_score) is het harmonische gemiddelde van precision en intrekken met de waarde die variëren tussen 0 (slechtste) op 1 (aanbevolen).

Voor binaire classificatie
- [Ontvanger besturingssysteem curven (ROC)](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) is ook een populaire metriek. In de ROC-curve model prestaties op basis van een vaste operationele punt op de ROC geïnterpreteerd.
- Maar voor PdM problemen, _decile tabellen_ en _lift-grafieken_ informatiever zijn. Ze zich richten op de positieve klasse (fouten) en bieden een complexere beeld van de algoritme-prestaties dan ROC-curve.
  - _Decile tabellen_ worden gemaakt met de test-voorbeelden in aflopende volgorde van kansen is mislukt. De geordende voorbeelden vervolgens gegroepeerd in deciles (10% van de voorbeelden met de hoogste kans en 20%, 30%, enzovoort). De verhouding (true positief tarief) /(random baseline) voor elke decile helpt de prestaties van de algoritme bij elke decile schatten. De basislijn willekeurige neemt waarden 0,1, 0,2, enzovoort.
  - [Lift-grafieken](http://www2.cs.uregina.ca/~dbd/cs831/notes/lift_chart/lift_chart.html) de decile waar positief snelheid versus willekeurige true positief frequentie voor alle deciles tekenen. De eerste deciles zijn meestal de focus van de resultaten, omdat ze de grootste voordelen weergeven. Eerste deciles kunnen ook worden gezien als representatief voor 'risico', wanneer gebruikt voor PdM.

## <a name="model-operationalization-for-predictive-maintenance"></a>Model uitoefening voor voorspeld onderhoud

Het voordeel de wetenschap gegevens oefening is gerealiseerde alleen wanneer het getrainde model wordt gemaakt operationeel. Dat wil zeggen, moet het model worden geïmplementeerd in de bedrijfssystemen voorspellingen op basis van nieuwe, eerder verborgen gegevens.  De nieuwe gegevens moet exact overeen met de _model handtekening_ van het getrainde model op twee manieren:
- alle functies moeten aanwezig zijn in elke logische instantie (spreek een rij in een tabel) van de nieuwe gegevens.
- de nieuwe gegevens vooraf moeten worden verwerkt en elk van de functies engineering, op dezelfde manier als de trainingsgegevens.

Dit proces wordt uitgedrukt in tal van manieren in academic en andere documentatie. Maar de volgende instructies dezelfde betekenis:
- _Beoordeling van nieuwe gegevens_ met het model
- _Het model toepassen_ op nieuwe gegevens
- _Operationeel_ het model
- _Implementeer_ het model
- _Het model uitvoeren_ voor nieuwe gegevens

Zoals eerder gezegd, wijkt model uitoefening voor PdM af van de peers. Scenario's met betrekking tot afwijkingsdetectie en foutdetectie doorgaans implementeren _online score berekenen_ (ook wel _score berekenen voor realtime_). Hier, het model _scores_ elke binnenkomende record, en retourneert een voorspelling. Voor afwijkingsdetectie, de voorspelling wordt aangegeven dat een afwijkingsdetectie opgetreden (voorbeeld: een klasse SVM). Foutdetectie is het type of de klasse van de fout.

Daarentegen PdM omvat _batchscoreberekening_. Om te voldoen aan de handtekening van het model, moeten de functies in de nieuwe gegevens op dezelfde manier als de trainingsgegevens worden ontworpen. Voor grote gegevenssets die wordt gebruikt voor nieuwe gegevens worden functies tijdens tijdvensters geaggregeerd en gewaardeerd in batch. Batchscoreberekening gewoonlijk wordt uitgevoerd in gedistribueerde systemen zoals [Spark](http://spark.apache.org/) of [Azure Batch](https://docs.microsoft.com/azure/batch/batch-api-basics). Er zijn een aantal alternatieven - beide suboptimale:
- Streaming gegevens engines ondersteunen aggregatie via windows in het geheugen. Zo kan worden stelde ze ondersteunen online score berekenen. Maar deze systemen via breder windows geschikt is voor compacte gegevens in smalle windows tijd of sparse-elementen zijn. Ze kunnen niet worden geschaald voor de dense gegevens via breder tijdvensters, zoals in scenario's PdM.
- Als de batchscoreberekening niet beschikbaar is, wordt de oplossing is om aan te passen online score berekenen voor het afhandelen van nieuwe gegevens in kleine batches tegelijk.

## <a name="solution-templates-for-predictive-maintenance"></a>Oplossingssjablonen voor voorspeld onderhoud

Het laatste gedeelte van deze handleiding biedt een lijst met PdM oplossingssjablonen, zelfstudies en experimenten die zijn geïmplementeerd in Azure. Deze toepassingen PdM kunnen worden geïmplementeerd in een Azure-abonnement binnen enkele minuten in sommige gevallen. Ze kunnen worden gebruikt als bewijs van concept demo's, sandboxes om te experimenteren met alternatieven of accelerators voor werkelijke productie-implementaties. Deze sjablonen bevinden zich in de [AI-galerie van Azure](http://gallery.azure.ai) of [Azure GitHub](https://github.com/Azure). Deze andere voorbeelden worden doorgevoerd in de oplossingssjabloon voor deze na verloop van tijd.

| # | Titel | Beschrijving |
|--:|:------|-------------|
| 1 | [Voorspeld onderhoud Azure Machine Learning-voorbeeld](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance) |Voorbeeld PdM te voorspellen, via de volgende N-tijdseenheden is mislukt. Dit voorbeeld wordt aangeduid met een Azure ML-Workbench-project en is ideaal voor beginnende gebruikers van PdM. [Aanvullende documentatie](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-predictive-maintenance) betrekking hebben op dit voorbeeld.|
| 2 | [Sjabloon voor voorspeld onderhoud Azure-oplossing](https://github.com/Azure/AI-PredictiveMaintenance) | Een end-to-end-framework voor het demonstreren van meerdere PdM-scenario's. Deze sjabloon biedt twee scenario's: de eerste is een nieuwe aanvraag voor gebruik van realtime fout voorwaarde classificatie. Het tweede scenario is gewoon een integratie van oplossing [1] naar de oplossingssjabloon voor deze. Dit laat zien hoe de dezelfde geïmplementeerde infrastructuur voor het toevoegen van andere scenario's voor nieuwe of bestaande hergebruiken.|
| 3 | [Deep Learning voor voorspeld onderhoud](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) | Azure laptop met een demo-oplossing van het gebruik van LSTM (lang korte geheugen) netwerken (een klasse terugkerende Neural Networks) voor voorspeld onderhoud met een [blogbericht op dit voorbeeld](https://azure.microsoft.com/blog/deep-learning-for-predictive-maintenance).|
| 4 | [Handleiding voor het modelleren in R voorspeld onderhoud](https://gallery.azure.ai/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) | PdM modellering handleiding met scripts in R.|
| 5 | [Azure voorspeld onderhoud voor ruimtevaart](https://gallery.azure.ai/Solution/Predictive-Maintenance-for-Aerospace-1) | Een van de eerste PdM oplossingssjablonen op basis van Azure ML v1.0 voor het onderhoud van vliegtuigmotoren. Deze handleiding afkomstig is van dit project. |
| 6 | [Azure AI Toolkit voor IoT-rand](https://github.com/Azure/ai-toolkit-iot-edge) | AI in de IoT-rand met TensorFlow; Toolkit pakketten grondige learning-modellen in Azure IoT Edge-compatibele Docker-containers en deze modellen als REST-API's weer te geven.
| 7 | [Azure IoT voor voorspeld onderhoud](https://github.com/Azure/azure-iot-predictive-maintenance) | Azure IoT Suite pc's - vooraf geconfigureerde oplossing. Onderhoud van vliegtuigmotoren-PdM sjabloon met IoT Suite. [Een ander document](https://docs.microsoft.com/azure/iot-suite/iot-suite-predictive-overview) en [scenario](https://docs.microsoft.com/azure/iot-suite/iot-suite-predictive-walkthrough) betrekking hebben op hetzelfde project. |
| 8 | [Sjabloon voor voorspeld onderhoud met SQL Server-R-Services](https://gallery.azure.ai/Tutorial/Predictive-Maintenance-Template-with-SQL-Server-R-Services-1) | Demonstratie van de resterende levensduur scenario op basis van R-services. |
| 9 | [Handleiding voor het modelleren van voorspeld onderhoud](https://gallery.azure.ai/Collection/Predictive-Maintenance-Modelling-Guide-1) | Vliegtuig onderhoud gegevensset functie gebouwd met behulp van R met [experimenten](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Modelling-Guide-Experiment-1) en [gegevenssets](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Modelling-Guide-Data-Sets-1) en [Azure notebook](https://gallery.azure.ai/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) en [experimenten](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2)in AzureML v1.0|

## <a name="training-resources-for-predictive-maintenance"></a>Training resources voor voorspeld onderhoud

De [leertraject voor Azure AI voor voorspeld onderhoud](https://github.com/Azure/AI-PredictiveMaintenance/blob/master/docs/azure-ai-learning-path-for-predictive-maintenance.md) biedt het trainingsmateriaal voor een beter begrip van de concepten en math achter de algoritmen en technieken die worden gebruikt in PdM problemen. 

Microsoft Azure biedt gratis inhoud en training voor algemene AI-concepten en praktijken.

| Training resource  | Beschikbaarheid |
|:-------------------|--------------|
| [AI-ontwikkelaar in Azure](http://azure.microsoft.com/training/learning-paths/azure-ai-developer) | Openbaar |
| [Microsoft AI School](http://aischool.microsoft.com/learning-paths) | Openbaar |
| [Azure AI leren van GitHub](https://github.com/Azure/connectthedots/blob/master/readme.md) | Openbaar |
| [LinkedIn Learning](http://www.linkedin.com/learning) | Openbaar |
| [Microsoft AI Youtube Webinars](https://www.youtube.com/watch?v=NvrH7_KKzoM&t=4s) | Openbaar |
| [Microsoft AI weergeven](http://channel9.msdn.com/Shows/AI-Show) | Openbaar |
| [LearnAI@MS](http://learnanalytics.microsoft.com) | Voor Microsoft-Partners |
| [Microsoft Partner Network](http://learningportal.microsoft.com) | Voor Microsoft-Partners |

Bovendien zijn gratis MOOCS (grote open onlinecursussen) op AI aangeboden online door onderwijsinstellingen zoals Stanford en MIT en andere educatieve bedrijven.