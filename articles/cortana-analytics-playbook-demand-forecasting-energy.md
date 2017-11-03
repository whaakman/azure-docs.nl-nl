---
title: Cortana Intelligence oplossing sjabloon Playbook voor het voorspellen van de vraag van energie | Microsoft Docs
description: Een oplossingssjabloon met Microsoft Cortana Intelligence waarmee forecast aanvraag voor een bedrijf, energie-hulpprogramma.
services: cortana-analytics
documentationcenter: 
author: ilanr9
manager: ilanr9
editor: yijichen
ms.assetid: 8855dbb9-8543-45b9-b4c6-aa743a04d547
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2016
ms.author: ilanr9;yijichen;garye
ms.openlocfilehash: 275e387878900154660d044b26ff5ac03a17a65a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Cortana Intelligence oplossing sjabloon Playbook voor het voorspellen van de vraag van energie
## <a name="executive-summary"></a>Managementsamenvatting
In de afgelopen jaren, hebben Internet der dingen (IoT), alternatieve energie en big data samengevoegde voor het maken van de enorme mogelijkheden in het hulpprogramma en de energie-domein. Op hetzelfde moment hebben het hulpprogramma en de volledige energiesector verbruik plat uit met betere methoden voor het beheren van hun gebruik van energie veeleisende consumenten gezien. Daarom zijn het hulpprogramma en de smartcard raster bedrijven nodig voor innoveren en vernieuwen van zichzelf. Bovendien steeds veel hulpprogramma's en energiebeheer rasters verouderde en kostbare onderhouden en beheren. Tijdens het afgelopen jaar werkt het team op een aantal betrokkenheid binnen de energie-domein. Er is veel gevallen waarin de hulpprogramma's of ISV's (Independent Software Vendors) hebben op zoek in voorspellen voor toekomstige energie vraag tijdens deze betrokkenheid. Deze prognoses speelt een belangrijke rol in het huidige en toekomstige bedrijf en de basis voor verschillende gebruiksvoorbeelden zijn geworden. Het gaat hierbij om korte en lange termijn power load prognose, handel, taakverdeling, raster optimalisatie, enzovoort. BIG data en geavanceerde analyses AA-methoden zoals Machine Learning (ML) zijn de belangrijkste onderdelen mogelijk te maken voor het produceren van nauwkeurige en betrouwbare prognoses.  

In deze playbook we samenstellen de zakelijke en analytische richtlijnen die nodig zijn voor een geslaagde ontwikkeling en implementatie van de vraag energie forecast oplossing. Deze voorgestelde richtlijnen kunnen u hulpprogramma's, gegevenswetenschappers en gegevens engineers volledig geoperationaliseerd, cloud-gebaseerde, voorspellen van de vraag oplossingen tot stand wordt gebracht. Voor de bedrijven die hun big data en geavanceerde analyses vervoer net beginnen, kan een dergelijke oplossing de initiële seed in hun langetermijnstrategie smart raster vertegenwoordigen.

> [!TIP]
> Zie voor het downloaden van een diagram waarin u een overzicht van de architectuur van deze sjabloon bevat [Cortana Intelligence oplossingssjabloon architectuur voor het voorspellen van de vraag van energie](cortana-analytics-architecture-demand-forecasting-energy.md).  
> 
> 

## <a name="overview"></a>Overzicht
Dit document bevat informatie over de zakelijke, gegevens en technische aspecten van het gebruik van Cortana Intelligence en in bepaalde Azure Machine Learning (AML) voor de implementatie en de implementatie van energie prognose oplossingen. Het document bestaat uit drie hoofdonderdelen:  

1. Inzicht in het bedrijf  
2. Understanding Data  
3. Technische implementatie

De **bedrijven inzicht** deel bevat een overzicht van het bedrijf aspect één moet om te begrijpen en overwegen voordat een beslissing investering. Hierin wordt uitgelegd hoe u het zakelijke probleem op dat moment om ervoor te zorgen dat predictive analytics en machine learning inderdaad effectieve en van toepassing zijn. Het document verder vindt u de basisbeginselen van machine learning en hoe deze wordt gebruikt voor het oplossen van problemen energie prognose. Het bevat een overzicht van de vereisten en de kwalificatiecriteria van een gebruiksvoorbeeld. Aantal voorbeelden gebruiken aanvragen en het bedrijfsscenario scenario's worden ook gegeven.

Gegevens zijn het hoofdbestanddeel voor een machine learning-oplossing. De **gegevens Understanding** deel van dit document bevat enkele belangrijke aspecten van de gegevens. Het bevat een overzicht van de soort gegevens die nodig is voor de prognose energie, data quality vereisten en welke gegevensbronnen bestaan doorgaans. Ook wordt uitgelegd hoe de onbewerkte gegevens wordt gebruikt voor het voorbereiden van de functies voor die daadwerkelijk het deel van het model.

Het derde onderdeel van het document bevat informatie over de **technische implementatie** aspect van een oplossing. Functie-engineering en modellering vormen de kern van het proces van de wetenschappelijke gegevens en worden daarom enkele gedetailleerd besproken. Ze omvatten het concept van webservices een belangrijk medium voor cloudimplementatie van predictive analytics-oplossingen zijn. We een typische architectuur van een operationalized end-to-end-oplossing ook een overzicht.

Het document bevat bovendien referentiemateriaal die u gebruiken kunt om te krijgen meer inzicht in het domein en de technologie.

Het is belangrijk te weten dat er niet van plan bent omvatten in dit document de meer gedetailleerde gegevens wetenschappelijke processen de wiskundige en technische aspecten. Deze gegevens vindt u in [documentatie voor Azure ML](http://azure.microsoft.com/services/machine-learning/) en [blogs](http://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Doelgroep
De doelgroep voor dit document is zowel zakelijke en technische medewerkers die u wilt leren kennen en kennis van Machine Learning gebaseerde oplossingen en hoe deze worden gebruikt specifiek binnen het domein energie prognose.

Gegevenswetenschappers kunnen ook profiteren van het lezen van dit document voor het verkrijgen van een beter inzicht te krijgen van het hoogste niveau proces dat de implementatie van een oplossing prognose energie-stations. In deze context kan ook worden gebruikt om een goede basis stand te brengen en beginpunt voor meer gedetailleerde en geavanceerde materiaal.

### <a name="industry-trends"></a>Trends van
In de afgelopen jaren hebben IoT, alternatieve energie en big data samengevoegde voor het maken van de enorme mogelijkheden in het hulpprogramma en de energie-ruimte. Op hetzelfde moment hebben het hulpprogramma en de volledige energiesectoren verbruik plat uit met betere methoden voor het beheren van hun gebruik van energie veeleisende consumenten gezien.

Veel hulpprogramma en slimme energiebedrijven is pioneering de [smart raster](https://en.wikipedia.org/wiki/Smart_grid) door het implementeren van een aantal gebruik aanvragen die maken gebruik van de gegevens die worden gegenereerd door het raster. Veel gebruiksvoorbeelden gebaseerd op de intrinsieke kenmerken van elektriciteitsproductie: kan niet worden verzameld of gereserveerd als inventarisatie worden opgeslagen. Ja, wat wordt geproduceerd moet worden gebruikt. Hulpprogramma's die u wilt weten hoe u efficiënter energieverbruik gewoon prognose nodig omdat die deze meer mogelijkheden om te krijgt **en aanbod in evenwicht**, waardoor wordt voorkomen dat energie-verliezen **handel gas uitstoot verminderen**, en beheerkosten.

Bij het bespreken van de kosten, is er een ander belangrijk aspect, de prijs is. Nieuwe mogelijkheden voor de handel stroom tussen de hulpprogramma's voor een goede nodig zijn om te hebben gebracht **voorspellen van toekomstige vraag en toekomstige prijs elektriciteit**. Hierdoor kunnen bedrijven hun omvang van de productie te bepalen.

Wanneer we het woord 'smart' gebruiken, verwijzen we daadwerkelijk naar een raster dat u kunt meer informatie over de en voorspellingen. Kan het seizoen wijzigingen in het verbruik van verwacht, evenals **voorzien van tijdelijke overbelasting situaties en automatisch aanpassen voor**. Door op afstand reguleren verbruik (met behulp van deze smartcard meters), kunnen gelokaliseerde overbelasting situaties worden verwerkt. **Door eerst voorspellen en vervolgens fungeert**, het raster kunt u zelf slimmer gedurende een bepaalde periode.

Voor de rest van dit document gaan we in op een specifieke reeks gebruiksvoorbeelden die betrekking hebben op voorspellen van toekomstige, korte termijn en op lange termijn energie-aanvraag. We werken al op deze gebieden een paar maanden en hebben opgedaan sommige kennis en vaardigheden dat wij bedrijfstak hoogwaardige resultaten opleveren. Andere toepassingen worden behandeld ook in het document in de nabije toekomst.

## <a name="business-understanding"></a>Inzicht in het bedrijf
### <a name="business-goals"></a>Zakelijke doelstellingen
De **energie Demo** doel is om aan te tonen een typische predictive analytics en machine learning-oplossing die kan worden geïmplementeerd in een korte periode. In het bijzonder is onze huidige focus over het inschakelen van energie vraag prognose oplossingen zodat de bedrijfswaarde kan snel worden gerealiseerd en worden gebruikt bij. De informatie in deze playbook kunt de klant uitvoeren van de volgende doelen:

* Oplossing op basis van een korte tijd op de waarde van machine learning
* Mogelijkheid om uit te breiden een proef gebruiksvoorbeeld aan andere toepassingen of een ruimere bereik op basis van hun zakelijke behoeften
* Snel krijgen van Cortana Intelligence Suite productkennis

Met deze drie doelen voor ogen, wordt deze playbook gericht op het leveren van de zakelijke en technische kennis die u helpt bij het bereiken van deze doelstellingen.

### <a name="power-load-and-demand-forecasting"></a>Power laden en het voorspellen van de vraag
Binnen de energiesector, kan er veel manieren besproken waarop vraag prognose kan helpen bedrijfsproblemen oplost. In feite voorspellen van de vraag kan worden beschouwd als de basis voor veel core gebruiksvoorbeelden in de branche. In het algemeen we twee soorten energie vraagprognoses beschouwen: korte en lange termijn. Elk criterium mag hebben een ander doel en gebruikmaken van een andere benadering. Het belangrijkste verschil tussen de twee is de prognoses horizon, wat betekent dat het bereik van de tijd in de toekomst waarvoor we zou prognose.

#### <a name="short-term-load-forecasting"></a>Voor de korte termijn Load prognose
Korte termijn laden prognose (STLF) wordt binnen de context van energie-aanvraag is gedefinieerd als de geaggregeerde belasting die wordt een prognose in de nabije toekomst op verschillende onderdelen van het raster (of het raster als geheel). Korte termijn wordt in deze context is gedefinieerd als periode binnen het bereik van 1 uur tot 24 uur. In sommige gevallen is er ook een periode van 48 uur mogelijk. Daarom is STLF heel gebruikelijk in een aanvraag operationeel gebruik van het raster. Hier volgen enkele voorbeelden van STLF aangestuurd gebruiksvoorbeelden:

* Vraag en aanbod Netwerktaakverdeling
* Handelspartners ondersteuning voor energiebeheer
* Markt maken (instelling power prijs)
* Raster operationele optimalisatie
* [Vraag antwoord](https://en.wikipedia.org/wiki/Demand_response)
* Pieksnelheden voorspellen van de vraag
* Vraag aan clientzijde management
* Taakverdeling en preventie van de overbelasting
* Lange termijn Load prognose
* Detectie van fouttolerantie en afwijkingsdetectie
* Piek inperking/herverdeling 

STLF model zijn voornamelijk gebaseerd op de bijna afgelopen (laatste dag of week) gegevens over het verbruik en gebruik prognose temperatuur als een belangrijke manier. Het verkrijgen van nauwkeurige temperatuur prognose voor het volgende uur en up tot 24 uur wordt steeds minder van een challenge nu dagen. Deze modellen zijn minder gevoelig voor seizoensgebonden patronen of trends voor lange termijn verbruik.

Er zijn ook SLTF oplossingen waarschijnlijk voor het genereren van grote hoeveelheden voorspelling aanroepen (serviceaanvragen) omdat ze zijn wordt aangeroepen op uurbasis en in sommige gevallen zelfs met een hogere frequentie. Het is ook heel gebruikelijk om te zien stadium van de innesteling waar elke afzonderlijke onderstation of transformator wordt weergegeven als een model met zelfstandige en zijn daarom het volume aan aanvragen van de voorspelling nog groter.

#### <a name="long-term-load-forecasting"></a>Lange termijn Load prognose
Het doel van de lange termijn Load prognose (LTLF) is te voorspellen power-aanvraag met een periode tussen 1 week tot meerdere maanden (en in sommige gevallen een aantal jaar). Dit bereik van de horizon is vooral van toepassing voor het plannen van investering gebruiksscenario's.

Voor de lange termijn scenario's is het belangrijk om gegevens van hoge kwaliteit die betrekking heeft op een reeks van meerdere jaren (minimaal 3 jaar). Deze modellen wordt doorgaans periodieke patronen extraheren uit de historische gegevens en maken gebruik van externe predicators zoals als weer en klimaat patronen.

Het is belangrijk om te verduidelijken dat hoe langer de prognoses horizon, hoe minder nauwkeurig de prognose kan zijn. Daarom is het belangrijk dat u sommige intervallen vertrouwen samen met de werkelijke prognose waarmee mensen rekening houden met de mogelijke variatie in hun planningsproces produceren.

Aangezien het verbruik scenario voor LTLF is voornamelijk planning, verwachten we veel lagere voorspelling volumes (in vergelijking met STLF). We deze voorspellingen in visualisatie's zoals Excel of Power BI embedded standaard wordt weergegeven en handmatig worden aangeroepen door de gebruiker.

### <a name="short-term-vs-long-term-prediction"></a>Korte termijn vs. Voorspelling van de lange termijn
De volgende tabel vergelijkt STLF en LTLF met betrekking tot de belangrijkste kenmerken:

| Kenmerk | Korte termijn Load Forecast | Lange termijn Load prognose |
| --- | --- | --- |
| Prognose Horizon |Vanaf 1 uur, 48 uur |Van 1 tot en met 6 maanden of meer |
| Samenvattingen van gegevens |Elk uur |Elk uur of dagelijks |
| Typische gebruiksvoorbeelden |<ul><li>Vraag/aanbod Netwerktaakverdeling</li><li>Kies uur prognose</li><li>Vraag antwoord</li></ul> |<ul><li>Lange termijn planning</li><li>Raster activa plannen</li><li>Resourceplanning</li></ul> |
| Typische variabelen |<ul><li>Dag of week</li><li>Uur van dag</li><li>Temperatuur per uur</li></ul> |<ul><li>Maand van het jaar</li><li>Dag van de maand</li><li>Lange termijn temperatuur- en klimaat</li></ul> |
| Historische gegevensbereik |Twee of drie jaar aan gegevens |5 tot 10 jaren aan gegevens |
| Gemiddelde nauwkeurigheid |MAPE * van 5% of minder |MAPE * van 25% of minder |
| Prognose frequentie |Elk uur of elke 24 uur geproduceerd |Geproduceerd zodra per maand, kwartaal of jaar |

\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – gemiddeld percentage fout betekent

Zoals blijkt uit deze tabel, is het erg belangrijk onderscheid maken tussen de korte en lange termijn prognose scenario's, omdat dit andere bedrijfsbehoeften vertegenwoordigen en mogelijk andere implementatie en het verbruik patronen.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Gebruiksvoorbeeld voorbeeld 1: eSmart systemen – overbelasting optimalisatie
Een belangrijke rol van een [smart raster](https://en.wikipedia.org/wiki/Smart_grid) moet dynamisch en voortdurend te optimaliseren en voor de veranderende verbruik patronen aanpassen. Energieverbruik kan worden beïnvloed door op korte termijn wijzigingen die hoofdzakelijk worden veroorzaakt door temperatuur schommelingen (*bijvoorbeeld*, meer stroom wordt gebruikt voor lucht voorwaarde of verwarming). Energieverbruik wordt op hetzelfde moment ook beïnvloed door trends op lange termijn. Deze kunnen seizoensgebonden effecten, feestdagen op lange termijn groei van de consumptie en zelfs economische factoren zoals consumer index olie en bbp bevatten.

In dit geval gebruik [eSmart](http://www.esmartsystems.com/) wilt implementeren van een cloud-gebaseerde oplossing waarmee het investeringsneiging van overbelasting van het raster op een gegeven onderstation voorspellen. In het bijzonder wilde eSmart onderstations die waarschijnlijk overbelasting binnen een uur, zodat een onmiddellijke actie kan worden ondernomen om te voorkomen of oplossen van die situatie identificeren.

Een nauwkeurige en snel uitvoeren voorspelling vereist de implementatie van de drie voorspellende modellen:

* Lange termijn model waarmee een prognose van het energieverbruik op elke onderstation tijdens de volgende enkele weken of maanden
* Korte termijn model waarmee voorspelling van overbelasting op een gegeven onderstation in het volgende uur
* Temperatuur model waarmee voorspellen van toekomstige temperatuur via meerdere scenario 's

Het doel van de lange termijn model is het rangschikken van de onderstations op basis van hun investeringsneiging van de overbelasting (gegeven hun energiecapaciteit transmission) tijdens de volgende week of maand. Hierdoor is het maken van een korte lijst met onderstations die als invoer voor de korte termijn voorspelling bedienen. Zoals temperatuur een belangrijke manier voor het model op lange termijn is, is het nodig voortdurend meerdere scenario temperatuur prognoses produceren en ze feed als invoer in het model op lange termijn. De korte termijn prognose wordt vervolgens om te voorspellen welke onderstation waarschijnlijk via het volgende uur van de overbelasting is aangeroepen.

De korte en lange termijn modellen worden afzonderlijk per elke onderstation geïmplementeerd. Daarom vereist de praktische uitvoering van deze modellen uitgebreide orchestration. Om toegang te krijgen hogere nauwkeurigheid op de korte termijn, is een meer gedetailleerd model als toegewezen voor elk uur van de dag. Alle deze modellen worden elk uur uitgevoerd en worden uitgevoerd binnen een paar minuten aan dat er voldoende tijd om te reageren en preventieve maatregelen indien nodig te voltooien. Deze verzameling van modellen wordt bijgewerkt door periodieke retraining de meest recente gegevens.

Meer informatie over deze gebruiksvoorbeeld vindt [hier](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Gebruik Case kwalificatiecriteria: vereisten
De belangrijkste sterkte van Cortana Intelligence is in de krachtige mogelijkheid om te implementeren en schalen van machine learning gericht oplossingen. Het is ontworpen voor ondersteuning van duizenden van voorspellingen die gelijktijdig worden uitgevoerd. Het kan automatisch schalen om te voldoen aan veranderende verbruik patroon. Een oplossing focus is daarom op de nauwkeurigheid en de computerprestaties. Bijvoorbeeld, is een bedrijf hulpprogramma geïnteresseerd in het opstellen van nauwkeurige energie-vraag prognose voor het volgende uur en voor elk uur van de dag. Aan de andere kant we minder geïnteresseerd bent in het beantwoorden van de vraag van waarom de aanvraag wordt voorspeld worden zoals deze is (het model zelf zorgt voor die).

Daarom rekening mee dat niet alle gebruiksvoorbeelden en zakelijke problemen effectief kunnen worden opgelost met behulp van machine learning.

Cortana Intelligence en machine learning mogelijk zeer effectieve bij het oplossen van een bepaalde zakelijke probleem wanneer de volgende criteria wordt voldaan:

* Het zakelijke probleem in de hand is **voorspellende** aard. Een voorbeeld van een Voorspellend gebruik case is een hulpprogramma bedrijf die u wilt voorspellen power belasting van een bepaalde onderstation in het volgende uur. Aan de andere kant analyseren en classificeren van stuurprogramma's van de historische vraag zou worden **beschrijvende** van aard en bijgevolg minder van toepassing.
* Er is een duidelijke **pad van de actie** moeten worden uitgevoerd zodra de voorspelling beschikbaar is. Bijvoorbeeld, het voorspellen van een overbelasting op een onderstation in het volgende uur een proactieve actie van de belasting die is gekoppeld aan die onderstation verminderen en zo mogelijk te voorkomen dat een overbelasting activeren.
* Het gebruiksvoorbeeld vertegenwoordigt een **typische type probleem** zo dat als opgelost deze kunt bereid u voor het oplossen van andere vergelijkbare gebruiksvoorbeelden.
* De klant kunt instellen **kwalitatieve en kwantitatieve doelstellingen** ter illustratie van de implementatie van een geslaagde oplossing. Een goede kwantitatief doel voor energie vraagprognose zou bijvoorbeeld de vereiste nauwkeurigheid drempelwaarde (*bijvoorbeeld*, maximaal 5% is een fout is toegestaan) of wanneer onderstation voorspellen van de overbelasting vervolgens de precisie (percentage van de waarde true positieven) en terughalen (mate van true positieven) moet hoger dan een opgegeven drempelwaarde. Deze doelstellingen moeten zijn afgeleid van de klant zakelijke doelstellingen.
* Er is een duidelijke **integratiescenario** met workflow binnen het bedrijf van het bedrijf. De onderstation load prognose kan bijvoorbeeld worden geïntegreerd in het raster beheercentrum om toe te staan activiteiten van overbelasting te voorkomen.
* De klant heeft, gebruiksklare **gegevens met voldoende kwaliteit** ter ondersteuning van het gebruiksvoorbeeld (Zie voor meer informatie in de volgende sectie **gegevenskwaliteit**, van deze playbook).
* De klant-architectuur hebben cloud gericht gegevens of **cloud-gebaseerde machine learning**, met inbegrip van Azure ML en andere onderdelen Cortana Intelligence Suite.
* De klant is tot stand brengen wil **een end-to-end-gegevensstroom** die faciliteiten de levering van gegevens naar de cloud voortdurend, en wil **operationeel** de oplossing.
* De klant is gereed om te **toe te wijzen aan resources** die zal worden actief ingeschakeld tijdens de eerste implementatie van de test zodat kennis en eigendom van de oplossing kunnen worden overgebracht naar de klant bij voltooiing zonder fouten.
* De bron van de klant moet een **ervaren gegevens professional**, bij voorkeur een wetenschappelijk gegevens.

Eis van een gebruiksvoorbeeld op basis van de bovenstaande criteria kan aanzienlijk verbeteren de succespercentages van een gebruiksvoorbeeld en tot stand brengen van een goede beachhead voor de uitvoering van aanvragen voor toekomstig gebruik.

### <a name="cloud-based-solutions"></a>Cloud-gebaseerde oplossingen
Cortana Intelligence Suite in Azure is een geïntegreerde omgeving die zich in de cloud bevindt. De implementatie van een oplossing voor geavanceerde analyses in een cloudomgeving bevat aanzienlijke voordelen voor bedrijven en op hetzelfde moment big wijziging voor bedrijven kunnen betekenen dat nog steeds on-premises gebruiken IT-oplossingen. Binnen de energiesector is er een duidelijke trend van geleidelijke migratie van bewerkingen in de cloud. Deze trend gaat gepaard samen met de ontwikkeling van de smartcard raster zoals beschreven in bovenstaande **Trends van**. Als deze playbook is gericht op een cloud-gebaseerde oplossing in het energie-domein, is het belangrijk om uit te leggen van de voordelen en andere overwegingen voor het implementeren van een cloud-gebaseerde oplossing.

Mogelijk is het grootste voordeel van een cloud-gebaseerde oplossing de kosten. Als een oplossing maakt gebruik van cloud geïmplementeerd onderdelen, er is geen opstartkosten of onderdeelkosten omzet (kosten van de omzet) gekoppeld. Dit betekent dat hoeft niet te investeren in de hardware, software en IT-onderhoud, dus er is een aanzienlijke vermindering van bedrijfsrisico.

Een ander belangrijk voordeel is de kostenstructuur betalen naar gebruik van cloud-gebaseerde oplossingen. Cloud-gebaseerde servers voor computing of de opslag kunnen worden geïmplementeerd en worden geschaald op basis van just-nodig. Hiermee wordt het voordeel van de efficiëntie kosten van een cloud-gebaseerde oplossing.

Er is ten slotte niet nodig voor het investeren in de IT-onderhoud of toekomstige infrastructuurontwikkeling, omdat deze deel uitmaakt van de aanbieding cloud-gebaseerde. Dat opzicht Cortana Intelligence Suite omvat het beste in klasse services en de routekaart houdt in ontwikkeling. Nieuwe functies, onderdelen en -mogelijkheden zijn voortdurend geïntroduceerd en ontwikkelen.

Voor een bedrijf dat de overgang naar de cloud nog wordt gestart, bevelen maximaal we voor een geleidelijke manier door het implementeren van een routekaart voor cloud-migratie. We zijn ervan overtuigd dat de toepassingen die worden beschreven in deze playbook voor hulpprogramma's en bedrijven in het energie-domein, een uitstekende gelegenheid voor het testen van predictive analytics-oplossingen in de cloud vertegenwoordigen.

#### <a name="business-case-justification-considerations"></a>Overwegingen voor zakelijke rechtvaardiging voor de aanvraag
In veel gevallen is de klant mogelijk geïnteresseerd zijn bij het maken van een zakelijke rechtvaardiging voor een bepaalde gebruiksvoorbeeld waarin een cloud-gebaseerde oplossing en Machine Learning belangrijke onderdelen zijn. In tegenstelling tot een on-premises-oplossing in het geval van een cloud-gebaseerde oplossing, is minimaal, het onderdeel kosten vooraf en de meeste van de kostenelementen zijn gekoppeld aan het werkelijke gebruik. Bij het implementeren van een oplossing op Cortana Intelligence Suite prognose energie, kunnen meerdere services kunnen worden geïntegreerd met een enkele algemene kostenstructuur. Bijvoorbeeld: databases (*bijvoorbeeld*, SQL Azure) kunnen worden gebruikt voor het opslaan van de onbewerkte gegevens en klik vervolgens voor de werkelijke prognoses Azure ML wordt gebruikt voor het hosten van de prognoses services. In dit voorbeeld kan de kostenstructuur opslag en transactionele onderdelen bevatten.

Aan de andere kant moet één een goed begrip van de zakelijke voordelen van het besturingssysteem van een energie-vraag prognose (korte of lange termijn) hebben. Het is in feite de zakelijke voordelen van elke bewerking prognose Houd er rekening mee. Bijvoorbeeld, nauwkeurig power load prognose voor de eerstvolgende 24 uur overproduction kunt voorkomen of overloads op het raster kunt voorkomen en dit kan worden bepaalde in termen van financiële besparingen dagelijks.

Een eenvoudige formule voor het berekenen van de financiële voordelen van de vraag forecast oplossing: ![Basic formule voor het berekenen van de financiële voordelen van de vraag forecast oplossing](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Aangezien Cortana Intelligence Suite een betalen naar gebruik prijsmodel biedt, hoeft niet voor een onderdeel van de vaste kosten op deze formule aangaan. Deze formule kan worden berekend op basis van dagelijks, maandelijks of jaarlijks.

Huidige Cortana Intelligence Suite en Azure ML prijsstelling vindt [hier](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Ontwikkelingsproces oplossing
De ontwikkelingscyclus van een energie-vraag prognose oplossing omvat doorgaans 4 fasen, die allemaal we maken gebruik van cloud-gebaseerde technologieën en services binnen de Cortana Intelligence Suite.

Dit wordt geïllustreerd in het volgende diagram:

![Smart raster cyclus](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

Het volgende lid beschrijft het proces van deze stap 4:

1. **Gegevensverzameling** – een geavanceerde gebaseerd analytics-oplossing is afhankelijk van de gegevens (Zie **gegevens Understanding**). In het bijzonder als het gaat om predictive analytics en prognoses, wij zijn afhankelijk van continue, dynamische stroom van gegevens. In het geval van energie vraag prognoses, deze gegevens kan afkomstig zijn direct vanaf smart meters of al op een on-premises-database worden geaggregeerd. Er is ook afhankelijk van andere externe gegevensbronnen zoals weer en temperatuur. Deze continue stroom van gegevens moet worden beheerd, gepland en opgeslagen. [Azure Data Factory](http://azure.microsoft.com/services/data-factory/) (ADF) is onze belangrijkste werkpaard voor het uitvoeren van deze taak.
2. **Modeling** – voor nauwkeurige en betrouwbare energie prognoses, moet een (train) ontwikkelen en te onderhouden van een geweldige model dat maken gebruik van de historische gegevens en extraheert de betekenis en voorspellende patronen in de gegevens. Het gebied van Machine Learning (ML) heeft groeien snel met meer geavanceerde algoritmen die regelmatig worden ontwikkeld. Azure ML Studio biedt een goede gebruikerservaring waarmee gebruikmaken van de meest geavanceerde algoritmen ML binnen een werkstroom voltooid. Werkstroom wordt weergegeven in een intuïtieve stroomdiagram en omvat het voorbereiden van gegevens, het ophalen van functies, modellering en model evaluatie. De gebruiker kunt ophalen in honderden verschillende modellen die zijn opgenomen in deze omgeving. Aan het einde van deze fase wordt een wetenschappelijk gegevens hebben een werkmodel die volledig geëvalueerd en klaar voor implementatie.
   
   Het volgende diagram wordt een afbeelding van een gangbare werkstroom:
   
   ![Modellering van een werkstroom](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)
3. **Implementatie** – met een werkmodel echter in de volgende stap is voor implementatie. Hier wordt het model geconverteerd naar een webservice die beschrijft een RESTful-API die gelijktijdig van verschillende verbruik clients via Internet kan worden aangeroepen. Azure ML biedt een eenvoudige manier voor het implementeren van een model rechtstreeks vanuit de Azure ML Studio met één klik een knop. Het volledige implementatieproces gebeurt achter de schermen. Deze oplossing kunt automatisch schalen om te voldoen aan de vereiste verbruik.
4. **Verbruik** : In deze fase, maken we daadwerkelijk gebruik van het prognosemodel voor het produceren van voorspellingen. Het verbruik van de toepassing van een gebruiker kan worden bepaald (*bijvoorbeeld*, dashboard) of rechtstreeks van een operationeel systeem zoals vraag/aanbod balancing systeem of een raster optimalisatie-oplossing. Meerdere gebruiksmogelijkheden kunnen uit één model worden aangestuurd.

## <a name="data-understanding"></a>Understanding Data
Na die betrekking hebben op de overwegingen voor bedrijven (Zie **bedrijven inzicht**) van een energie-vraag prognose oplossing, bent er nu klaar om het gegevensonderdeel worden behandeld. Een predictive analytics-oplossing is afhankelijk van betrouwbare gegevens. Voor het voorspellen van energie de vraag, afhankelijk wij zijn van historische verbruiksgegevens met verschillende detailniveaus. Historische gegevens wordt gebruikt als de grondstoffen. Deze worden onderworpen aan een zorgvuldige analyse waarin het gegevens-wetenschappelijk variabelen (ook wel onderdelen genoemd) die kunnen worden geplaatst in een model waarmee de vereiste prognoses uiteindelijk genereert wordt geïdentificeerd.

In de rest van deze sectie worden de verschillende stappen en overwegingen voor informatie over de gegevens en hoe u aan om aan een bruikbaar formulier worden beschreven.

### <a name="the-model-development-cycle"></a>De ontwikkelingscyclus Model
Het opstellen van goede prognose modellen vereist zorgvuldige voorbereiding en planning. De het modelleringsproces in meerdere stappen te analyseren en te focussen op één stap tegelijk kunnen het resultaat van het hele proces aanzienlijk te verbeteren.

Het volgende diagram illustreert hoe het modelleringsproces kan worden onderverdeeld in meerdere stappen:

![Model ontwikkelingscyclus](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Zoals te zien dat de cyclus bestaat uit zes stappen uitvoeren:

* Probleem formulering
* Gegevensopname en verkennen
* Voorbereiden van gegevens en functie-engineering
* Modelleren
* Model-evaluatie
* Ontwikkeling

In de rest van deze sectie worden de afzonderlijke stappen en rekening mee moet houden bij elke stap worden beschreven.

### <a name="problem-formulation"></a>Probleem formulering
We kunnen de formulering probleem als de meest kritieke stap één moet uitvoeren voorafgaand aan de implementatie van een predictive analytics-oplossing beschouwen. Hier wordt het zakelijke probleem zou transformeren en deze op bepaalde elementen die kunnen worden opgelost door middel van gegevens technieken afbreken. Het is raadzaam het probleem als een reeks vragen die wij willen graag beantwoorden formuleren. Hier volgen enkele mogelijke vragen die mogelijk van toepassing binnen het bereik van de prognose van energie-aanvraag:

* Wat is de verwachte belasting van een afzonderlijke onderstation in het volgende uur of dag?
* Op welk tijdstip van de dag wordt mijn raster pieken ervaren?
* Hoe waarschijnlijk is mijn raster ter ondersteuning van de verwachte piekbelasting?
* Hoeveel energie moet het station power genereren tijdens elk uur van de dag

Het opstellen van deze vragen kan wij gericht op de juiste gegevens ophalen en implementeren van een oplossing die volledig is afgestemd op het zakelijke probleem bij de hand. Daarnaast kunnen we enkele belangrijke metrische gegevens die kunnen we de prestaties van het model evalueren wordt ingesteld. Bijvoorbeeld, hoe nauwkeurig moet de prognose zijn en wat is het bereik van de fout die nog steeds aanvaardbaar door het bedrijf zijn?

### <a name="data-sources"></a>Gegevensbronnen
Het moderne smart raster verzamelt gegevens uit verschillende onderdelen en -onderdelen van het raster. Deze gegevens vertegenwoordigt verschillende aspecten van de bewerking en het gebruik van het raster power. Binnen het bereik van de energie-vraag prognose, zijn we de bespreking van de gegevensbronnen die overeenkomen met het werkelijke vraag verbruik te beperken. Een belangrijke bron van het energieverbruik zijn smart meters. Hulpprogramma's voor de hele wereld worden snel smart meters voor hun consumenten implementeert. Smart meters het werkelijke energieverbruik registreren en deze gegevens terug naar het hulpprogramma bedrijf voortdurend relay. Gegevens worden verzameld en verzonden terug op een vast interval, variërend van elke 5 minuten tot 1 uur. Meer geavanceerde smart meters kunnen op afstand beheren en het werkelijke verbruik binnen een huishouden in evenwicht worden geprogrammeerd. Smart meter gegevens is relatief betrouwbare en een tijdstempel bevat. Dat is het belangrijk ingrediënt voor vraag prognose. Gegevens van de meter kan worden geaggregeerd (getotaliseerde) op verschillende niveaus binnen de topologie raster: transformator, onderstation, regio, *enzovoort*. We kunnen het aggregatieniveau van de vereiste voor het bouwen van een prognosemodel voor vervolgens kiezen. Bijvoorbeeld, als het bedrijf hulpprogramma wilt voorspellen van toekomstige belasting voor elk van de rasterlijnen onderstations vervolgens alle meters gegevens kunnen worden samengevoegd voor elke afzonderlijke onderstation en gebruikt als invoer voor het prognosemodel. We noemen smart meters een interne gegevensbron.

Een aanvraag betrouwbare energie prognose vertrouwt ook op andere externe gegevensbronnen. Een belangrijke factor die invloed heeft op energieverbruik is het weer of nauwkeuriger de temperatuur. Historische gegevens vindt u nauwe correlatie tussen externe temperatuur- en energieverbruik. Hot zomer dagen, de consument maken gebruik van hun airconditioning en tijdens de verwarming winter inschakelen. Een betrouwbare bron van historische temperaturen op de rasterlocatie is daarom sleutel. Bovendien afhankelijk we ook van nauwkeurige voorspelling van temperatuur energieverbruik te voorspellen.

Andere externe gegevensbronnen kunnen ook helpen bij het bouwen van prognosemodellen energie-aanvraag. Deze kunnen wijzigingen in de lange termijn klimaat, voordelige indexen bevatten (*bijvoorbeeld*, bbp), en andere. In dit document nemen niet we deze andere gegevensbronnen.

### <a name="data-structure"></a>De structuur van gegevens
Nadat u de vereiste gegevensbronnen, graag willen we om ervoor te zorgen dat onbewerkte gegevens die zijn verzameld, de functies van de juiste gegevens bevat. Als u wilt maken van een betrouwbare vraag prognosemodel, moet we om ervoor te zorgen dat de verzamelde gegevens gegevenselementen die kunnen helpen bij het voorspellen van toekomstige vraag bevat. Hier volgen enkele basisvereisten inzake de gegevensstructuur (schema) van de onbewerkte gegevens.

De onbewerkte gegevens bestaat uit rijen en kolommen. Elke meting wordt weergegeven als één rij met gegevens. Elke rij gegevens bevat meerdere kolommen (ook aangeduid als functies of velden).

1. **Tijdstempel** – de tijdstempelveld vertegenwoordigt de werkelijke tijd waarop de meting is geregistreerd. Het moet voldoen aan een van de algemene datum/tijd-indelingen. Zowel de datum en tijd onderdelen moeten worden opgenomen. In de meeste gevallen is er niet nodig voor de tijd moeten tot het tweede niveau van granulatie worden vastgelegd. Het is belangrijk om op te geven van de tijdzone waarin de gegevens worden geregistreerd.
2. **ID meten** -dit veld geeft de meter of het apparaat van de meting. Dit kan is een categorische variabele en een combinatie van cijfers en tekens.
3. **Waarde van de consumptie** – dit is het werkelijke verbruik op een gegeven datum/tijd. Het verbruik kan worden gemeten in kWh (kilowatt-hour) of een andere gewenste eenheden. Het is belangrijk te weten dat de maateenheid in alle metingen in de gegevens consistent moet blijven. In sommige gevallen kan verbruik meer dan 3 power fasen worden opgegeven. In dat geval moet er voor het verzamelen van alle fasen van de onafhankelijke verbruik.
4. **Temperatuur** – de temperatuur doorgaans worden verzameld van een onafhankelijke bron. Er moet echter compatibel is met de gegevens over het verbruik. Neemt een tijdstempel die ervoor zorgen dat deze worden gesynchroniseerd met de werkelijke verbruiksgegevens zoals hierboven is beschreven. De waarde van de temperatuur kan worden opgegeven in graden c of Fahrenheit, maar moet blijven consistent op alle metingen.
5. **Locatie –** het locatieveld is meestal gekoppeld aan de plaats waar de temperatuur-gegevens zijn verzameld. Worden kan weergegeven als een zip-code number of breedtegraad/lengtegraad (lat/lang)-indeling.

De volgende tabellen ziet u voorbeelden van een goede gebruiks- en temperatuur gegevensindeling:

| **Datum** | **Tijd** | **ID van de meter** | **Fase 1** | **Fase 2** | **Fase 3** |
| --- | --- | --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |ABC1234 |7.0 |2.1 |5.3 |
| 7/1/2015 |10:00:01 |ABC1234 |7.1 |2.2 |4.3 |
| 7/1/2015 |10:00:02 |ABC1234 |6.0 |2.1 |4.0 |

| **Datum** | **Tijd** | **Locatie** | **Temperatuur** |
| --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |11242 |24.4 |
| 7/1/2015 |10:00:01 |11242 |24.4 |
| 7/1/2015 |10:00:02 |11242 |24.5 |

Zoals hierboven kan worden afgeleid, worden in dit voorbeeld 3 verschillende waarden voor verbruik die zijn gekoppeld aan 3 power fasen bevat. Houd er ook rekening mee dat de datum en tijd velden worden gescheiden, maar ze kunnen ook worden gecombineerd in één kolom. In dit geval wordt de kolom locatie weergegeven in een 5-cijferige postcode-indeling en de temperatuur in een zekere mate c-indeling.

### <a name="data-format"></a>Indeling van gegevens
De meest voorkomende gegevensindelingen zoals CSV, TSV JSON, kan worden ondersteund door Cortana Intelligence Suite *enzovoort*. Het is belangrijk dat de indeling van de gegevens consistent voor de volledige levenscyclus van het project blijft.

### <a name="data-ingestion"></a>Gegevensopname
Aangezien energie vraagprognose wordt voortdurend en vaak voorspeld, moeten we ervoor zorgen dat de onbewerkte gegevens door middel van een solide en betrouwbare opname proces stroomt. Het proces opname moet garanderen dat de onbewerkte gegevens beschikbaar voor het prognosemodel proces op het moment dat vereist is. Die impliceert dat de frequentie van de opname gegevens groter dan de frequentie van het prognosemodel moet.

Bijvoorbeeld: als onze oplossing prognose aanvraag een nieuwe prognose om 8:00 AM dagelijks genereert moet we om ervoor te zorgen dat alle gegevens die zijn verzameld tijdens de afgelopen 24 uur is het volledig is ingenomen tot dat moment en moet ook het afgelopen uur aan gegevens bevatten.

Cortana Intelligence Suite biedt hiervoor, verschillende manieren ter ondersteuning van een betrouwbare gegevens opname-proces. Dit wordt nader besproken in de **implementatie** gedeelte van dit document.

### <a name="data-quality"></a>Gegevenskwaliteit
De bron van onbewerkte gegevens die nodig is voor het uitvoeren van de aanvraag voor betrouwbare en nauwkeurige prognose moet voldoen aan de criteria van een aantal basisgegevens kwaliteit. Hoewel geavanceerde statistische methoden kunnen worden gebruikt om te compenseren voor een aantal mogelijke gegevens kwaliteit probleem, moet er nog steeds om ervoor te zorgen dat we een aantal basisgegevens kwaliteit drempelwaarde overschrijden bij het ophalen van nieuwe gegevens. Hier volgen enkele overwegingen betreffende de kwaliteit van de onbewerkte gegevens:

* **Ontbrekende waarde** – dit verwijst naar de situatie wanneer bepaalde meting is niet verzameld. Hier het basic vereiste is dat de ontbrekende waarde snelheid niet groter dan 10% voor een bepaalde periode zijn moet. In geval een enkele waarde is het ontbrekende moet worden aangegeven met een vooraf gedefinieerde waarde (bijvoorbeeld: '9999') en niet '0', die een geldige meting kan worden.
* **Nauwkeurigheid van de meting** : de werkelijke waarde van verbruik of temperatuur nauwkeurig moet worden geregistreerd. Onjuiste metingen levert onnauwkeurige prognoses. Normaal gesproken moet de meting fout lager zijn dan 1% ten opzichte van de waarde ' True '.
* **Tijdstip van de meting** – dit is vereist dat de werkelijke tijdstempel van de gegevens verzameld wordt niet meer dan 10 seconden ten opzichte van de waarde true tijd van de werkelijke meting afwijken.
* **Synchronisatie** – wanneer meerdere gegevensbronnen worden gebruikt (*bijvoorbeeld*, gebruiks- en temperatuur) we moet ervoor zorgen dat er geen tijdsynchronisatie problemen ertussen. Dit betekent dat het tijdsverschil tussen de tijdstempel die worden verzameld van twee onafhankelijke gegevensbronnen mag niet langer zijn dan meer dan 10 seconden.
* **Latentie** - zoals beschreven in bovenstaande **gegevensopname**, wij zijn afhankelijk van een betrouwbare gegevens stroom en opnamesnelheid proces. Om te bepalen die moeten we ervoor zorgen dat we de latentie van de gegevens bepalen. Dit is opgegeven als het tijdsverschil tussen de tijd die de werkelijke maat is gemaakt en het tijdstip waarop deze is geladen in de opslag van de Cortana Intelligence Suite en is klaar voor gebruik. Voor de korte termijn load prognose van de totale latentie mag niet langer dan 30 minuten. Voor de lange termijn load prognose van de totale latentie niet moet groter dan 1 dag.

### <a name="data-preparation-and-feature-engineering"></a>Voorbereiden van gegevens en functie-Engineering
Zodra de onbewerkte gegevens zijn geconsumeerd (Zie **gegevensopname**) en is veilig opgeslagen, is het gereed om te worden verwerkt. De gegevens voorbereidingsfase is in feite duurt de onbewerkte gegevens en converteren (transformeert, opnieuw) in een formulier voor het modelleren-fase. Eventueel die eenvoudige handelingen zoals het gebruik van de onbewerkte gegevenskolom is met de werkelijke waarde van de gemeten, gestandaardiseerde waarden, complexere-bewerkingen zoals [tijd achtergebleven](https://en.wikipedia.org/wiki/Lag_operator), en andere. De zojuist gemaakte gegevenskolommen worden aangeduid als gegevensfuncties en het proces voor het genereren van deze aangeduid als de functie-engineering. Aan het einde van dit proces moet er een nieuwe gegevensset die is is afgeleid van de onbewerkte gegevens en kan worden gebruikt voor het model. Bovendien moet de gegevens voorbereidingsfase zorgt voor de ontbrekende waarden (Zie **gegevenskwaliteit**) en compenseren voor hen. In sommige gevallen moet er ook normaliseren van de gegevens om ervoor te zorgen dat alle waarden in dezelfde schaal worden weergegeven.

In deze sectie die wordt een overzicht van de functies van algemene gegevens die zijn opgenomen in de energie prognosemodellen vraag.

**Tijd gebaseerde functies:** deze functies zijn afgeleid van de gegevens van de datum/tijdstempel. Deze zijn geëxtraheerd en geconverteerd naar categorische functies, zoals:

* Tijd van de dag: dit is de uur van de dag waarmee waarden tussen 0 en 23
* Dag van week – dit vertegenwoordigt de dag van de week en waarden van 1 (zondag) tot en met 7 (zaterdag)
* Dag van de maand – dit staat voor de huidige datum en kan waarden tussen 1 en 31
* Maand van het jaar – dit vertegenwoordigt de maand en waarden van 1 (januari) en 12 (December)
* Weekend – dit is een functie binaire waarde waarmee de waarden van 0 voor weekdagen of 1 voor het weekend
* Vakantiedag - dit is een binaire waarde-functie die de waarden van 0 voor een normale dag of 1 voordat een feestdag duurt
* Voorwaarden Fourier – de Fourier voorwaarden zijn gewichten die zijn afgeleid van het tijdstempel en worden gebruikt om vast te leggen van de seizoensgebonden (cycli) in de gegevens. Aangezien we meerdere seizoen in onze gegevens hebben mogelijk moeten we meerdere Fourier voorwaarden. Waarden van de aanvraag kunnen bijvoorbeeld jaarlijkse wekelijks en dagelijks seizoen/cycli dat in 3 Fourier voorwaarden resulteert.

**Onafhankelijke meting functies:** onafhankelijke biedt onder meer de gegevenselementen die we willen graag gebruiken als variabelen in het model. Hier wordt het afhankelijke onderdeel dat we moet te voorspellen uitsluiten.

* De functie lag – dit tijd verschoven zijn waarden van de werkelijke vraag. De functies lag 1 wordt bijvoorbeeld de waarde van de aanvraag houdt in het vorige uur ten opzichte van de huidig timestamp (ervan uitgaande dat per uur). Op deze manier we toevoegen lag 2, 3, lag *enzovoort*. De werkelijke combinatie van lag functies die worden gebruikt tijdens de fase modellering bepaald door de evaluatie van de resultaten van het model.
* Lange termijn trends – deze functie vertegenwoordigt de lineaire groei van de vraag tussen jaar.

**Afhankelijke functie:** het afhankelijke onderdeel is de kolom met gegevens die willen we graag onze model om te voorspellen. Met [onder supervisie machine learning](https://en.wikipedia.org/wiki/Supervised_learning), moeten we het model met behulp van de afhankelijke functies (die ook aangeduid als labels) eerst te trainen. Hierdoor kan het model voor meer informatie over de patronen in de gegevens die zijn gekoppeld aan het afhankelijke onderdeel. In energie vraag prognose doorgaans willen we de werkelijke vraag voorspellen en daarom wordt het zou gebruiken als het afhankelijke onderdeel.

**Verwerking van de ontbrekende waarden:** tijdens de voorbereidingsfase van gegevens moet we om te bepalen van de beste strategie voor het afhandelen van de ontbrekende waarden. Dit gebeurt meestal met behulp van de verschillende statistische [begrip Gegevensmethoden](https://en.wikipedia.org/wiki/Imputation_\(statistics\)). In het geval van energie vraag prognoses rekenen we ontbrekende waarden meestal met behulp van zwevend gemiddelde van de vorige beschikbare gegevenspunten.

**Gegevensnormalisatie:** gegevensnormalisatie is een ander type transformatie die wordt gebruikt voor alle numerieke gegevens zoals vraag prognose overbrengen naar een soortgelijke schaal. Dit meestal verbetert de nauwkeurigheid model en de precisie. We gewoonlijk doet dit door de werkelijke waarde delen door het bereik van de gegevens.
Hiermee wordt de oorspronkelijke waarde omlaag schalen in een kleiner bereik, doorgaans tussen 1 en 1 liggen.

## <a name="modeling"></a>Modelleren
De model-fase is waar de omzetting van de gegevens in een model plaatsvindt. In de kern van dit proces er zijn geavanceerde algoritmen die bij het scannen van de historische gegevens (trainingsgegevens), patronen ophalen en een model bouwen. Dit model kan later worden gebruikt om te voorspellen op nieuwe gegevens die niet is gebruikt voor het bouwen van het model.

Nadat we hebben een betrouwbare werkmodel we kan vervolgens worden gebruikt voor de beoordeling van nieuwe gegevens die is geordend op zodanig dat de vereiste onderdelen (X). Het scoreprofiel proces maakt gebruik van de persistente model (object van de fase training) en de doelvariabele die wordt aangeduid met Ŷ voorspellen.

### <a name="demand-forecasting-modeling-techniques"></a>Prognosemodel Modelleringstechnieken vraag
In het geval van een aanvraag prognose maken we gebruik van historische gegevens op die is besteld op tijd. We meestal verwijzen naar gegevens die de tijddimensie als omvat [time series](https://en.wikipedia.org/wiki/Time_series). Het doel in de reeks modellering van tijd is om te zoeken tijd gerelateerde trends, seizoensgebonden, auto-correlatie (correlatie na verloop van tijd) en die in een model te formuleren.

In de afgelopen jaren zijn geavanceerde algoritmen om time series prognose onder te brengen en prognoses nauwkeuriger ontwikkeld. We beschreven enkele van deze hier kort.

> [!NOTE]
> Deze sectie is niet bedoeld om te worden gebruikt als een machine learning en overzicht prognoses maar in plaats daarvan als een kort overzicht van modelleren technieken die vaak worden gebruikt voor het voorspellen van de vraag. Voor meer informatie en educatieve materiaal over tijd reeks prognose wordt ten zeerste aanbevolen de on line handleiding [prognose: beginselen en praktijken](https://www.otexts.org/book/fpp).
> 
> 

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**MA (zwevend gemiddelde)**](https://www.otexts.org/fpp/6/2)
Zwevend gemiddelde is een van de eerste analytische technieken die is gebruikt voor het voorspellen van de reeks tijd en is nog steeds een van de meest gebruikte technieken vanaf vandaag. Het is ook de basis voor meer geavanceerde prognose technieken. Met zwevend gemiddelde zijn we het volgende gegevenspunt gemiddelde via de meest recente punten K, waarbij K de volgorde van de zwevend gemiddelde geeft van de prognose.

De zwevend gemiddelde techniek heeft het effect van het vloeiend maken van de prognose en daarom kan niet verwerken en grote volatiliteit in de gegevens.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**ETS (exponentiële demping)**](https://www.otexts.org/fpp/7/5)
Exponentiële vloeiend maken (ETS) is een serie van verschillende methoden die gewogen gemiddelde van recente gegevenspunten gebruiken om te voorspellen van de volgende gegevenspunt. Het idee is hoger gewichten toewijzen aan meer recente waarden en dit gewicht voor oudere meetwaarden geleidelijk te verlagen. Er zijn een aantal verschillende manieren met deze familie, zijn enkele van deze verwerking van de seizoensgebonden in de gegevens, zoals [Holt Winters seizoensgebonden methode](https://www.otexts.org/fpp/7/5).

Sommige van deze methoden factor ook in de seizoensgebonden van de gegevens.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**ARIMA (automatische regressie geïntegreerd zwevend gemiddelde)**](https://www.otexts.org/fpp/8)
Automatische regressie geïntegreerde zwevend gemiddelde (ARIMA) is een ander gezin van methoden die wordt meestal gebruikt voor de prognose tijdreeks. Het combineert automatisch regressie methoden vrijwel met zwevend gemiddelde. Automatische regressie methoden maken gebruik van regressie modellen door middel van eerdere tijd reekswaarden om te kunnen berekenen van de volgende datum-punt. ARIMA methoden differentiërende methodes waaronder berekening van het verschil tussen de gegevenspunten en het gebruik van die in plaats van de oorspronkelijke waarde van de gemeten ook van toepassing. Ten slotte ARIMA ook maakt gebruik van het zwevende gemiddelde technieken die hierboven worden beschreven. De combinatie van al deze methoden op verschillende manieren is wat de familie van ARIMA methoden vormt.

ETS en ARIMA worden zijn veel vandaag gebruikt voor het voorspellen van de energie-vraag en veel andere prognoses problemen. In veel gevallen deze gecombineerd gecombineerd om zeer nauwkeurige resultaten.

**Algemene meerdere regressies** regressie modellen kunnen worden de belangrijkste modellering aanpak binnen het domein van de machine learning en statistieken. In de context van de tijdreeks gebruiken we regressie te voorspellen van toekomstige waarden (*bijvoorbeeld*, van de vraag). In regressie we nemen van een lineaire combinatie van de variabelen en meer informatie over het gewicht (ook wel coëfficiënten genoemd) van deze variabelen tijdens de training. Het doel is voor het produceren van een lijn die onze voorspelde waarde wordt verwacht. Regressie methoden zijn geschikt wanneer de doelvariabele numerieke en daarom ook time series prognose past. Er is een breed scala aan regressie methoden inclusief zeer eenvoudig regressie modellen zoals [lineaire regressie](https://en.wikipedia.org/wiki/Linear_regression) en meer geavanceerde toepassingsgroepen zoals beslissingsstructuren [willekeurige Forests](https://en.wikipedia.org/wiki/Random_forest), [Neural Networks](https://en.wikipedia.org/wiki/Artificial_neural_network), en Boosted Decision Trees.

Energie-aanvraag prognose als een probleem regressie construeren ons een grote flexibiliteit bij het selecteren van de gegevensfuncties van onze die kunnen worden gecombineerd van de werkelijke vraag time series gegevens en de externe factoren zoals temperatuur biedt. Meer informatie over de geselecteerde onderdelen worden beschreven in de functie-Engineering (Zie **voorbereiden van gegevens en functie-Engineering**) sectie van dit playbook.

Uit onze ervaring met implementatie- en implementatie van energie vraag prognoses test, er is vastgesteld dat de geavanceerde regressie modellen die beschikbaar in Azure ML zijn meestal de beste resultaten produceren en we maken gebruik ervan.

## <a name="model-evaluation"></a>Model-evaluatie
Evaluatie van het model is een belangrijke rol in de **Model ontwikkelingscyclus**. Bij deze stap kijken we in het model en de prestaties met gegevens van de praktijk worden gevalideerd. Tijdens de stap modellering we een deel van de beschikbare gegevens gebruiken voor het model te trainen. Tijdens de evaluatiefase nemen we de rest van de gegevens voor het testen van het model. Vrijwel betekent dit dat we de nieuwe modelgegevens die heeft is geherstructureerd en bevat de functies van de gegevensset training zijn voeding. Echter tijdens het validatieproces gebruiken we het model te voorspellen van de doelvariabele in plaats van de beschikbare doelservers-variabele opgeven. Vaak is dit proces als het score model. We zou vervolgens de doelwaarden true gebruiken en deze te vergelijken met de voorspelde waarden. Dit dient om te meten en beperken van de voorspelling fout, wat betekent dat het verschil tussen de voorspellingen en de waarde ' True '. De meting fout kwantificeren is sleutel Aangezien we willen graag aanpassen van het model en valideren of de fout daadwerkelijk afneemt. Aan te passen van het model kan worden gedaan door het wijzigen van Modelparameters waarmee het leerproces of door toe te voegen of te verwijderen van functies (aangeduid als [parameters vegen](https://channel9.msdn.com/Blogs/Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Dat betekent vrijwel dat we mogelijk herhalen tussen de functie-engineering, modelleren, en het model evaluatie fasen totdat we kunnen de fout op het vereiste niveau verlagen meerdere keren.

Het is belangrijk dat de fout voorspelling nooit nul als worden kan nadruk is nooit een model dat elke uitkomst perfect kan voorspellen. Er is echter een bepaalde grootte van de fout die aanvaardbaar is door het bedrijf. Tijdens het validatieproces willen we graag om ervoor te zorgen dat onze model prediction-fout op het niveau is of hoger is dan het tolerantieniveau van bedrijven. Het is daarom belangrijk om in te stellen van het niveau van de toegestane fout aan het begin van de cyclus tijdens de **probleem formulering** fase.

### <a name="typical-evaluation-techniques"></a>Typische evaluatietechnieken
Er zijn verschillende manieren in welke voorspelling fout kan worden gemeten en bepaalde. In deze sectie gaan we de bespreking van de relevante op tijdreeks en in specifieke voor energie vraag prognose evaluatietechnieken.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE staat voor de gemiddelde Absolute Percentage fout. Met MAPE we het verschil tussen zijn computing elk punt en de werkelijke waarde van dat punt prognose. We vervolgens kwantificeren de fout per punt door te berekenen van het deel van het verschil ten opzichte van de werkelijke waarde. Op de laatste stap gemiddelde we deze waarden. De wiskundige formule voor MAPE is het volgende:

![De formule MAPE](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*waar A<sub>t</sub> de werkelijke waarde, F<sub>t</sub> is de voorspelde waarde en n staat voor de prognose horizon.*

## <a name="deployment"></a>Implementatie
We hebben, kunt u de fase modelleren en kan de model-prestaties die we klaar voor gebruik in de implementatiefase zijn gevalideerd. In deze context betekent implementatie inschakelen van de klant gebruiken voor het model door uitgevoerd werkelijke voorspellingen op grote schaal. Het concept van implementatie is de sleutel in Azure ML omdat onze belangrijkste doel is om aan te roepen voortdurend voorspellingen in plaats van alleen het inzicht verkrijgen van de gegevens. De implementatiefase is het gedeelte waar we het model om te worden verbruikt op grote schaal kunnen inschakelen.

Binnen de context van energie vraagprognose is ons doel om aan te roepen continue en periodieke prognoses terwijl u ervoor zorgt dat nieuwe gegevens beschikbaar voor het model is en dat de voorspelde gegevens worden verzonden naar de client in beslag neemt.

### <a name="web-services-deployment"></a>Implementatie van Web-Services
De belangrijkste implementeerbare bouwsteen in Azure ML is de webservice. Dit is de meest effectieve manier om het verbruik van een Voorspellend model in de cloud inschakelen. De webservice ingekapseld van het model en wordt afgerond met een [RESTful](http://www.restapitutorial.com/) API (Application Programming Interface). De API kan worden gebruikt als onderdeel van een clientcode zoals geïllustreerd in het onderstaande diagram.

![We Service-implementatie en het verbruik](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Zoals u kunt zien, wordt de web-service wordt geïmplementeerd in de cloud Cortana Intelligence Suite en via het blootgestelde REST-API-eindpunt kan worden aangeroepen. Ander type van clients in verschillende domeinen kunt tegelijkertijd de service via de Web-API aanroepen. De webservice kan ook worden geschaald ter ondersteuning van duizenden gelijktijdige aanroepen.

### <a name="a-typical-solution-architecture"></a>Een typische oplossingsarchitectuur
Bij het implementeren van een oplossing prognose energie-vraag bent we geïnteresseerd in een end-to-end-oplossing die zich verder uitstrekt dan de webservice voor de voorspelling en vereenvoudigt de hele gegevensstroom implementeren. Op het moment dat we een nieuwe prognose aanroepen, moet we om ervoor te zorgen dat het model met de functies van de meest recente gegevens worden ingevoerd. Dat betekent dat de zojuist verzamelde onbewerkte gegevens is voortdurend ingenomen, verwerkt en in de vereiste functieset omgezet in het model is opgebouwd. Op hetzelfde moment willen we ervoor dat de verwachte gegevens beschikbaar zijn voor het end gebruiken van clients. Een voorbeeld van de gegevens stroom cyclus (of gegevens pijplijn) wordt weergegeven in het diagram hieronder:

![Prognose complete gegevensstroom van de energie-vraag](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Dit zijn de stappen die uitgevoerd als onderdeel van de prognose energiecyclus voor aanvraag worden:

1. Miljoenen meters geïmplementeerde gegevens genereren voortdurend energieverbruiksgegevens in realtime.
2. Deze gegevens worden verzameld en geüpload naar een cloud-opslagplaats (*bijvoorbeeld*, Azure Blob).
3. De onbewerkte gegevens worden samengevoegd tot een onderstation of regionaal niveau voordat het wordt verwerkt, zoals gedefinieerd door het bedrijf.
4. De verwerking van de functie (Zie **voorbereiden van gegevens en verwerken van de functie**) vervolgens plaatsvindt en geeft de gegevens die vereist is voor training of score model: de functie setgegevens worden opgeslagen in een database (*bijvoorbeeld*, SQL Azure).
5. De service opnieuw training wordt opgeroepen opnieuw trainen van het prognosemodel – dat de nieuwe versie van het model wordt bewaard, zodat deze kan worden gebruikt door de webservice score.
6. De score-webservice wordt aangeroepen op een planning die geschikt is voor de vereiste frequentie van de prognose.
7. De verwachte gegevens worden opgeslagen in een database die toegankelijk zijn voor de end-verbruik-client.
8. De client verbruik haalt de prognoses, past deze weer in het raster en in overeenstemming met het vereiste gebruiksvoorbeeld worden verbruikt.

Het is belangrijk te weten dat deze volledige cyclus is volledig geautomatiseerd en volgens een planning wordt uitgevoerd. De volledige orchestration van deze gegevenscyclus kunt doen met behulp van hulpprogramma's zoals [Azure Data Factory](http://azure.microsoft.com/services/data-factory/).

### <a name="end-to-end-deployment-architecture"></a>Architectuur van de end-to-End-implementatie
Om een prognose energie demand-oplossing op Cortana Intelligence vrijwel implementeert, moeten we ervoor dat de vereiste onderdelen zijn gemaakt en correct geconfigureerd.

Het volgende diagram wordt een typische Cortana Intelligence gebaseerd architectuur die wordt geïmplementeerd en stuurt de cyclus voor het stroom van gegevens die hierboven beschreven:

![Architectuur van de end-to-End-implementatie](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Raadpleeg voor meer informatie over elk van de onderdelen en de hele architectuur de energie-oplossingssjabloon.

