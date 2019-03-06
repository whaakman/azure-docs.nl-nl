---
title: Sjabloon Playbook van Cortana Intelligence-oplossing voor vraagprognose van energie
description: Een oplossingssjabloon met Microsoft Cortana Intelligence waarmee de prognose van vraag voor een energiebedrijf-hulpprogramma.
services: machine-learning
author: ilanr9
manager: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/24/2016
ms.author: garye
ms.openlocfilehash: bb5ef610e55495c372a47ff78e3252c9d8ec7055
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57435913"
---
# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Sjabloon Playbook van Cortana Intelligence-oplossing voor vraagprognose van energie
## <a name="executive-summary"></a>Managementsamenvatting
Internet of Things (IoT), alternatieve energiebronnen en big data in de afgelopen jaren hebben samengevoegd voor het maken van grote verkoopkansen in het hulpprogramma en de energie-domein. Op hetzelfde moment, hebben het hulpprogramma en de volledige energiesector verbruik plat maken om met klanten betere manieren voor het beheren van hun gebruik van energie veeleisende gezien. Daarom kan zijn het hulpprogramma en slimme grid bedrijven nodig voor het vernieuwen en vernieuwen zelf. Bovendien worden veel hulpprogramma's en energiebeheer rasters vaker het verouderde en zeer kostbaar zijn te onderhouden en beheren. Tijdens het afgelopen jaar werkt het team aan een aantal engagements binnen het domein energie. Tijdens deze betrokkenheid, Hiermee hebben we veel gevallen waarin de hulpprogramma's of ISV's (Independent Software Vendors) hebt op zoek was bij de prognoses voor toekomstige energievraag aangetroffen. Deze prognoses speelt een belangrijke rol in hun huidige en toekomstige activiteiten en de basis vormt voor verschillende gebruiksvoorbeelden zijn geworden. Het gaat hierbij om korte en lange termijn power load prognose, handel, taakverdeling, raster optimalisatie, enzovoort. BIG data en geavanceerde analyse (AA) methoden, zoals Machine Learning (ML) zijn de belangrijkste virtualisatielaag voor het produceren van nauwkeurige en betrouwbare prognoses.

In deze playbook we samengesteld de zakelijke en analytische richtlijnen die nodig zijn voor een geslaagde ontwikkeling en implementatie van de vraag naar energie prognose-oplossing. Deze voorgestelde richtlijnen kunnen u hulpprogramma's, gegevenswetenschappers en data-engineers bij het opstellen van volledig ingezette, cloudgebaseerde en on-demand prognose-oplossingen. Voor bedrijven die hun big data en geavanceerde analyse reis net begint, kan een dergelijke oplossing de basiswaarde in hun langetermijnstrategie slimme grid vertegenwoordigen.

> [!TIP]
> Zie voor het downloaden van een diagram een overzicht van de architectuur van deze sjabloon biedt [architectuur van de Cortana Intelligence-oplossingssjabloon voor on-demand prognose van energie](cortana-analytics-architecture-demand-forecasting-energy.md).
>
>

## <a name="overview"></a>Overzicht
In dit document bevat informatie over het bedrijf, gegevens en technische aspecten van het gebruik van Cortana Intelligence en in bepaalde Azure Machine Learning (AML) voor de implementatie- en implementatie van oplossingen voor energie voorspellen. Het document bestaat uit drie belangrijke onderdelen:

1. Inzicht in het bedrijf
2. Inzicht in gegevens
3. Technische implementatie

De **inzicht in het bedrijf** onderdeel bevat een overzicht van het bedrijf aspect nodig zijn om te begrijpen en overwegen voordat u een beslissing investering. Hierin wordt uitgelegd hoe u het zakelijke probleem bij de hand om ervoor te zorgen dat predictive analytics en machine learning inderdaad effectief en van toepassing zijn. Het document verder vindt u de basisbeginselen van machine learning en hoe deze wordt gebruikt voor het voorspellen van energieverbruik problemen te verhelpen. Het bevat een overzicht van de vereisten en de kwalificatiecriteria van een use case. Een aantal voorbeelden use cases en het bedrijfsscenario scenario's zijn ook beschikbaar.

Gegevens zijn de belangrijkste onderdeel toe voor elke machine learning-oplossing. De **gegevens wat** onderdeel van dit document bevat informatie over enkele belangrijke aspecten van de gegevens. Het bevat een overzicht van het type gegevens dat is vereist voor het voorspellen van energieverbruik, kwaliteitsvereisten gegevens en welke gegevensbronnen bestaan doorgaans. Ook wordt uitgelegd hoe de onbewerkte gegevens om voor te bereiden gegevensfuncties die daadwerkelijk het deel van het model wordt gebruikt.

Het derde deel van het document bevat informatie over de **technische implementatie** aspect van een oplossing. Feature-engineering en modellen zijn in de kern van het data science process en daarom in bepaalde mate van detail worden besproken. Hierin wordt het concept van webservices een belangrijk vehicle voor cloudimplementatie van predictive analytics-oplossingen zijn. We een typische architectuur van een end-to-end ingezette oplossing ook een overzicht.

Het document bevat bovendien referentiemateriaal dat beschikbaar is die u gebruiken kunt om te krijgen meer inzicht in het domein en de technologie.

Het is belangrijk te weten dat we niet van plan bent om te kunnen krijgen in dit document het diepere data science process de wiskundige en technische aspecten. Deze informatie kunnen u vinden in [documentatie voor Azure Machine Learning-service](https://azure.microsoft.com/services/machine-learning/) en [blogs](https://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Doelgroep
De doelgroep voor dit document is zowel zakelijke als technische medewerkers die graag willen kennis en inzicht in Machine Learning op basis van oplossingen en hoe deze worden gebruikt om precies binnen het domein energie voorspellen.

Gegevenswetenschappers kunnen ook profiteren van dit document om te krijgen een beter begrip van het proces op hoog niveau die de implementatie van een prognose-oplossing energie-stations. In deze context het kan ook worden gebruikt voor een goede basislijn en beginpunt voor meer gedetailleerde en geavanceerde materiaal.

### <a name="industry-trends"></a>Trends in de branche
IoT, alternatieve energiebronnen en big data in de afgelopen jaren hebben voor het maken van enorme mogelijkheden in de ruimte hulpprogramma en de energie samengevoegd. Op hetzelfde moment, hebben het hulpprogramma en de volledige energiesectoren verbruik plat maken om met klanten betere manieren voor het beheren van hun gebruik van energie veeleisende gezien.

Veel hulpprogramma en slimme energiebedrijven hebben is beveiligingsexperts de [slimme grid](https://en.wikipedia.org/wiki/Smart_grid) door het implementeren van een aantal gebruik gevallen die gebruik van de gegevens die worden gegenereerd door het raster. Veel van use cases omvangsfase heeft te maken rond de inherente kenmerken van elektriciteitswinning: kan niet worden samengevoegd of gereserveerd als inventarisatie worden opgeslagen. Dus, wat wordt geproduceerd moet worden gebruikt. Hulpprogramma's die u wilt weten hoe u efficiënter nodig hebt om te voorspellen van energieverbruik gewoon omdat dit ze meer mogelijkheden om te geeft **vraag en aanbod in balans brengen**, waardoor wordt voorkomen dat energie verliezen, **handel gas beperken emissie**, en de kosten worden beheerd.

Bij het bespreken van de kosten, is er een ander belangrijk aspect, de prijs is. Nieuwe mogelijkheden voor energiebeheer tussen hulpprogramma's in een geweldige hoeft te hebben gebracht **voorspellen van toekomstige vraag en toekomstige prijsstijgingen van elektriciteit**. Dit kan helpen bedrijven hun omvang van de productie te bepalen.

Als we het woord 'slimme' gebruiken, verwijzen we eigenlijk naar een raster dat kan leren en vervolgens voorspellingen te doen. Deze seizoensgebonden wijzigingen in het verbruik van kan voorspellen, evenals **voorzien van tijdelijke overbelasting en automatisch aanpassen voor**. Door op afstand reguleren verbruik (aan de hand van deze slimme meters), kunnen gelokaliseerde overbelasting worden verwerkt. **Door eerst voorspellen en vervolgens handelen**, het raster kunt u zelf slimmer na verloop van tijd.

Voor de rest van dit document ligt de nadruk op een specifieke reeks van use cases die betrekking hebben op voorspellen van toekomstige, korte termijn en op lange termijn energievraag te voldoen. We werken al op deze gebieden een paar maanden en hebben opgedaan met enkele kennis en vaardigheden die we kunnen branche geavanceerde resultaten opleveren. Andere gevallen wordt gedekt ook in het document in de nabije toekomst.

## <a name="business-understanding"></a>Inzicht in het bedrijf
### <a name="business-goals"></a>Zakelijke doelstellingen
De **energie Demo** doel is om te demonstreren een typische predictive analytics en machine learning-oplossing die kan worden geïmplementeerd in een zeer korte periode. Onze huidige focus is met name over het inschakelen van energie-demandoplossingen prognose zodat de waarde voor het bedrijf kan snel worden gerealiseerd en worden gebruikt bij het. De informatie in deze playbook kan helpen de klant uitvoeren van de volgende doelen:

* Korte tijd op de waarde van machine learning op basis van de oplossing
* Mogelijkheid om uit te breiden van een leider use-case naar andere use-cases of naar een breder bereik op basis van hun zakelijke behoeften
* Productkennis van Cortana Intelligence Suite en snel inzicht

Met deze drie doelen voor ogen, deze playbook is erop gericht op het leveren van de zakelijke en technische kennis die u helpt bij het bereiken van deze doelstellingen.

### <a name="power-load-and-demand-forecasting"></a>Power belasting en on-Demand prognose
Binnen de energiesector, kunnen er veel manieren waarop vraag voorspellen kunt kritieke zakelijke problemen kunt oplossen. In feite vraagprognose kan worden beschouwd als de basis voor veel core use-cases in de branche. In het algemeen wij beschouwen als twee typen energie vraagprognoses: korte en lange termijn. Elk mogelijk een ander doel dienen en gebruikmaken van een andere benadering. Het belangrijkste verschil tussen de twee is de prognoses horizon, wat betekent dat het bereik van de tijd in de toekomst waarvoor we zouden prognose.

#### <a name="short-term-load-forecasting"></a>Korte termijn Load prognoses
Korte termijn laden prognose (STLF) wordt in de context van de vraag naar energie gedefinieerd als de cumulatieve load die in de nabije toekomst op verschillende onderdelen van het raster (of het raster als geheel) wordt een prognose. In deze context, wordt de korte termijn gedefinieerd als periode binnen het bereik van 1 uur tot 24 uur. In sommige gevallen kan is een periode van 48 uur ook mogelijk. Daarom is STLF heel gebruikelijk in een operationele use case van het raster. Hier volgen enkele voorbeelden van STLF gestuurde gebruiksvoorbeelden:

* Vraag en aanbod Netwerktaakverdeling
* Trading ondersteuning voor energiebeheer
* Handel maken (instelling power prijs)
* Raster operationele optimalisatie
* [Vraag-antwoord](https://en.wikipedia.org/wiki/Demand_response)
* Pieken vraagprognose
* Vraag aan clientzijde management
* Taakverdeling en preventie overbelasten
* Lange termijn Load prognoses
* Fouttolerantie en anomaliedetectie
* Piek inperking/herverdeling

STLF model zijn voornamelijk gebaseerd op het dichtstbijzijnde verleden (afgelopen dag of week) gegevens over het verbruik en gebruik temperatuur als een belangrijke voorspelde prognose. Het verkrijgen van nauwkeurige temperatuur prognose voor het volgende uur en van 24 uur wordt steeds minder van een uitdaging nu dagen. Deze modellen zijn minder gevoelig zijn voor seizoensgebonden patronen of verbruikstrends van op de lange termijn.

SLTF oplossingen waarschijnlijk ook voor het genereren van groot aantal aanroepen van de productie (serviceaanvragen) omdat ze wordt op uurbasis berekend en in sommige gevallen zelfs met een hogere frequentie aangeroepen. Het is ook heel gebruikelijk om te zien stadium van de innesteling waar elke afzonderlijke onderstation of transformator wordt weergegeven als een zelfstandige model en het aantal aanvragen voor voorspelling zijn daarom nog groter.

#### <a name="long-term-load-forecasting"></a>Lange termijn Load prognoses
Het doel van de lange termijn Load prognose (LTLF) is prognose van vraag met een periode, variërend van 1 week tot meerdere maanden (en in sommige gevallen voor een aantal jaar). Dit bereik van de horizon is voornamelijk van toepassing voor het plannen van en investering gebruiksvoorbeelden.

Voor scenario's op de lange termijn is het belangrijk dat u hebt gegevens van hoge kwaliteit die betrekking heeft op een reeks van meerdere jaren (ten minste 3 jaar). Deze modellen wordt gewoonlijk seizoensgebondenheid patronen onttrekken aan de historische gegevens en maakt gebruik van externe predicators zoals als weer en klimaat patronen.

Het is belangrijk om te verduidelijken dat hoe langer de prognoses horizon is, hoe minder nauwkeurig de prognose kan zijn. Daarom is het belangrijk dat u bepaalde intervallen vertrouwen, samen met de werkelijke prognose waarmee mensen rekening met de mogelijke variatie in hun planningsproces zou produceren.

Aangezien het verbruik scenario voor LTLF is voornamelijk plan, kunnen we verwachten dat veel lager voorspelling volumes (in vergelijking met STLF). We meestal wordt deze voorspellingen in visualisatie-hulpprogramma's zoals Excel of Power BI embedded en handmatig worden aangeroepen door de gebruiker.

### <a name="short-term-vs-long-term-prediction"></a>Korte termijn vs. Voorspelling van de lange termijn
De volgende tabel vergelijkt STLF en LTLF met betrekking tot de belangrijkste kenmerken:

| Kenmerk | Prognose van de korte termijn laden | Lange termijn Load prognose |
| --- | --- | --- |
| Prognose Horizon |Van 1 uur tot 48 uur |Van 1 tot en met 6 maanden of meer |
| Gegevens granulariteit |Elk uur |Elk uur of dagelijks |
| Typische gebruiksvoorbeelden |<ul><li>Vraag/aanbod Netwerktaakverdeling</li><li>Kies uur prognoses</li><li>Vraag-antwoord</li></ul> |<ul><li>Lange termijn planning</li><li>Raster activa plannen</li><li>Resourceplanning</li></ul> |
| Typische voorspellingsfactoren |<ul><li>Dag of week</li><li>uur van de dag</li><li>Per uur temperatuur</li></ul> |<ul><li>Maand van jaar</li><li>Dag van maand</li><li>Lange termijn temperatuur en klimaat</li></ul> |
| Bereik van historische gegevens |Twee of drie jaar aan gegevens |Vijf tot en met 10 jaar aan gegevens |
| Typische nauwkeurigheid |MAPE * van 5% of minder |MAPE * van 25% of minder |
| Prognose frequentie |Elk uur of elke 24 uur |Geproduceerd eenmaal per maand, kwartaal of jaar |

\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – gemiddeld percentage fout betekent

Als uit deze tabel kan worden gezien, is het heel belangrijk dat u onderscheid maken tussen de korte en lange termijn voorspellen scenario's zoals deze staan voor verschillende zakelijke behoeften en mogelijk verschillende implementatie- en gebruikspatronen voor databasebronnen.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Voorbeeld van de Use Case 1: eSmart systemen – overbelasting optimalisatie
Een belangrijke rol van een [slimme grid](https://en.wikipedia.org/wiki/Smart_grid) is dynamisch en voortdurend te optimaliseren en aanpassen aan de veranderende gebruikspatronen voor databasebronnen. Energieverbruik kan worden beïnvloed door die voornamelijk worden veroorzaakt door temperatuur fluctuaties (*bijvoorbeeld*, meer stroom wordt gebruikt voor lucht voorwaarde of verwarming). Energieverbruik wordt tegelijkertijd, ook beïnvloed door op de lange termijn trends. Deze kunnen seizoensgebondenheid effecten, feestdagen, op de lange termijn de groei van consumptie en zelfs de economische factoren zoals consumenten index, de olieprijs en bbp bevatten.

In dit geval gebruik [eSmart](https://www.esmartsystems.com/) wilde implementeren van een cloud-gebaseerde oplossing waarmee de neiging zullen zijn van overbelasting van het raster op een bepaalde onderstation voorspellen. In het bijzonder wilde eSmart substations die waarschijnlijk overbelasten binnen een uur, zodat een onmiddellijke actie kan worden ondernomen om te voorkomen of oplossen van die situatie identificeren.

Een nauwkeurige en het uitvoeren van snelle voorspelling vereist implementatie van drie voorspellende modellen:

* Lange termijn model waarmee voorspellen van energieverbruik op elke onderstation tijdens de volgende paar weken of maanden
* Korte termijn model waarmee voorspelling van overbelasting op een bepaalde onderstation in het volgende uur
* Temperatuur-model dat u kunt voorspellen van toekomstige temperatuur via meerdere scenario 's

Het doel van de lange termijn model is het de substations rangschikken op basis van hun neiging zullen zijn om te overbelasten (gegeven hun energiecapaciteit verzending) tijdens de volgende week of maand. Hiermee wordt het maken van een korte lijst substations die als invoer voor de korte termijn voorspelling dienen kan. Zoals temperatuur hoger een belangrijk voorspelde voor de lange termijn model is, is er een nodig voortdurend meerdere scenario temperatuur prognoses maken en ze als invoer in voor het model op de lange termijn. De prognose van de korte termijn wordt vervolgens aangeroepen om te voorspellen welke onderstation dreigt te overbelasten in het volgende uur.

De modellen voor zowel kortdurende als langdurige worden afzonderlijk per elke onderstation geïmplementeerd. Daarom vereist de praktische uitvoering van deze modellen uitgebreide indeling. Om te krijgen hogere nauwkeurigheid op de korte termijn, wordt een meer gedetailleerd model toegewezen voor elk uur van de dag. Alle deze modellen worden elk uur uitgevoerd en uitgevoerd binnen een paar minuten aan dat er voldoende tijd om te reageren en preventieve acties uitvoeren, indien nodig. Deze verzameling van modellen wordt up-to-date gehouden door periodieke retraining met behulp van de meest recente gegevens.

Meer informatie over deze use case vindt [hier](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Gebruik Case kwalificatiecriteria-vereisten
De belangrijkste sterkte van Cortana Intelligence is in de krachtige mogelijkheid voor het implementeren en schalen van machine learning gebaseerde oplossingen. Het is ontworpen voor ondersteuning van duizenden van voorspellingen die gelijktijdig worden uitgevoerd. Deze kan automatisch worden geschaald om te voldoen aan een wisselende verbruik-patroon. De focus van een oplossing is daarom op nauwkeurigheid en verwerkingsprestaties. Bijvoorbeeld, is een energiebedrijf geïnteresseerd in productie nauwkeurige energievraag prognose voor het volgende uur en voor elk uur van de dag. Aan de andere kant zijn we minder geïnteresseerd in het beantwoorden van de vraag van waarom de aanvraag wordt voorspeld zijn als deze is (het model zelf zorgt dat).

Het is dus houd er rekening mee dat niet alle use-cases en zakelijke problemen effectief kunnen worden opgelost met behulp van machine learning.

Cortana Intelligence en machine learning kunnen worden zeer effectief zijn bij het oplossen van een bepaalde zakelijke probleem wanneer de volgende criteria wordt voldaan:

* Het zakelijke probleem in voorraad is **voorspellende** van aard. Een voorbeeld van een voorspellende use case is een hulpprogramma voor bedrijf die graag power belasting van een bepaalde onderstation in het volgende uur te voorspellen. Aan de andere kant, analyseren en classificeren van stuurprogramma's van de historische vraag zou worden **beschrijvende** van aard en daarom minder van toepassing.
* Er is een duidelijke **pad van de actie** moeten worden uitgevoerd zodra de voorspelling beschikbaar is. Bijvoorbeeld, het voorspellen van een overbelasting op een onderstation in het volgende uur een proactieve actie van de load die is gekoppeld aan deze onderstation verminderen en zo mogelijk te voorkomen dat een overbelasting activeren.
* Hiermee geeft u de use-case een **typische type probleem dat** zo dat als opgelost deze kunt bereid u voor de manier waarop voor het oplossen van andere vergelijkbare gebruiksvoorbeelden.
* De klant kunt instellen **kwalitatieve en kwantitatieve doelstellingen** ter illustratie van de implementatie van een succesvolle oplossingen. Een goede kwantitatief doel voor vraagprognose energie zou bijvoorbeeld, de nauwkeurigheid van de vereiste drempelwaarde (*bijvoorbeeld*, maximaal 5% fout is toegestaan) of wanneer het voorspellen van onderstation overbelasten vervolgens de precisie (percentage van echt positieven) en het intrekken (mate van echt positieven) moet hoger dan een opgegeven drempelwaarde. Deze doelstellingen moeten zijn afgeleid van de klant zakelijke doelstellingen.
* Er is een duidelijke **integratiescenario** met bedrijfswerkstroom uit van het bedrijf. Bijvoorbeeld, kan de prognose van de load onderstation in het raster control center om toe te staan van overbelasting preventie activiteiten worden geïntegreerd.
* De klant heeft, gebruiksklare **gegevens van kwaliteit voldoende** ter ondersteuning van de use-case (Zie voor meer informatie in de volgende sectie **gegevenskwaliteit**, van deze playbook).
* De klant-architectuur is gebaseerd op met cloud gericht gegevens of **cloud op basis van machine learning**, met inbegrip van Azure ML en andere onderdelen van de Cortana Intelligence Suite.
* De klant is uitgeroepen tot stand brengen **een end-to-gegevensstroom** die faciliteiten de levering van gegevens in de cloud op een vaste regelmatig uitgevoerd, en wil **operationeel** de oplossing.
* De klant is gereed om te **toe te wijzen aan resources** die zal worden actief Bezig tijdens de initiële implementatie van de pilot zodat kennis en het eigendom van de oplossing kunnen worden overgedragen aan de klant na voltooiing.
* De klant-resource moet een **ervaren gegevens professional**, bij voorkeur een gegevenswetenschapper.

Kwalificatie van een use case op basis van de bovenstaande criteria kan aanzienlijk verbeteren de succespercentages van een use-case en tot stand brengen van een goede beachhead voor de implementatie van de gevallen voor toekomstig gebruik.

### <a name="cloud-based-solutions"></a>Cloud-gebaseerde oplossingen
Cortana Intelligence Suite op Azure is een geïntegreerde omgeving die zich in de cloud bevindt. De implementatie van een geavanceerde analyseoplossing in een cloudomgeving bevat aanzienlijke voordelen voor bedrijven en op hetzelfde moment big wijzigen voor bedrijven kunnen betekenen dat nog steeds gebruik on-premises IT-oplossingen. Binnen de energiesector is er een duidelijke trend van geleidelijke migratie van bewerkingen in de cloud. Deze trend gaat hand in hand samen met de ontwikkeling van het raster slimme zoals hierboven, in **Trends in de branche**. Als deze playbook is gericht op een cloud-gebaseerde oplossing in het energie-domein, is het belangrijk om uit te leggen van de voordelen en andere overwegingen van de implementatie van een cloud-gebaseerde oplossing.

Het is wellicht het grootste voordeel van een cloud-gebaseerde oplossing de kosten. Als een oplossing maakt gebruik van de onderdelen van cloud zijn geïmplementeerd, is er geen kosten vooraf of onderdeelkosten van COGS (kosten van verkochte goederen) gekoppeld. Dit betekent dat er geen hoeven te investeren in hardware, software en IT-onderhoud, en er daarom een aanzienlijke vermindering van zakelijk risico is.

Een ander belangrijk voordeel is de kostenstructuur voor betalen per gebruik van cloud-gebaseerde oplossingen. Cloud-gebaseerde servers voor computing of de opslag kunnen worden geïmplementeerd en geschaald op basis van net zo nodig. Hiermee wordt het voordeel van de kosten voor efficiëntie van een cloud-gebaseerde oplossing.

Er is ten slotte niet nodig voor het investeren in de IT-onderhoud of toekomstige infrastructuurontwikkeling als al deze deel uit van de aanbieding op basis van een cloud maakt. Dat opzicht Cortana Intelligence Suite bevat het beste in klasse services en de roadmap houdt in ontwikkeling. Nieuwe functies, onderdelen en functies worden voortdurend wordt geïntroduceerd en ontwikkelen.

Voor een bedrijf dat de overgang naar de cloud nog wordt gestart, bevelen we zeer voor een geleidelijke manier door het implementeren van een overzicht van de cloud migreren. Wij geloven dat de use-cases die worden beschreven in dit playbook voor hulpprogramma's en bedrijven in de energie-domein, een uitstekende gelegenheid voor predictive analytics-oplossingen in de cloud valideert vertegenwoordigen.

#### <a name="business-case-justification-considerations"></a>Overwegingen voor zakelijke reden voor aanvraag
In veel gevallen de klant mogelijk geïnteresseerd zijn bij het maken van een zakelijke rechtvaardiging voor een bepaalde use-case belangrijke onderdelen waarin een cloud-gebaseerde oplossing en Machine Learning zijn. In tegenstelling tot een on-premises-oplossing in het geval van een cloud-gebaseerde oplossing is minimaal, het onderdeel kosten vooraf en de meeste van de kostenelementen zijn gekoppeld aan het werkelijke gebruik. Als het gaat om een prognose-oplossing op Cortana Intelligence Suite energie implementeren, kunnen meerdere services worden geïntegreerd met een enkele algemene kostenstructuur. Bijvoorbeeld,-databases (*bijvoorbeeld*, SQL Azure) kan worden gebruikt voor het opslaan van de onbewerkte gegevens en vervolgens voor de werkelijke geeft een prognose van Azure ML wordt gebruikt voor het hosten van de prognoses services. In dit voorbeeld kan de kostenstructuur voor opslag en transactionele onderdelen bevatten.

Aan de andere kant moet één hebben een goed begrip van de bedrijfswaarde van het besturingssysteem van een vraag naar energie voorspellen (korte of lange termijn). In feite is het belangrijk om te profiteren van de bedrijfswaarde van elke bewerking prognose. Bijvoorbeeld, nauwkeurig power load prognose voor de volgende 24 uur overproduction kunt voorkomen of overloads in het raster kunt voorkomen en dit kan worden gekwantificeerd in termen van financiële besparing op dagelijks.

Een eenvoudige formule voor het berekenen van de financiële voordelen van de vraag prognose-oplossing: ![Eenvoudige formule voor het berekenen van de financiële voordelen van de vraag prognose-oplossing](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Omdat Cortana Intelligence Suite voorziet in een prijsmodel voor betalen per gebruik, is er geen behoefte waarbij extra kosten voor een vaste prijs onderdeel op deze formule. Deze formule kan worden berekend op basis van een dagelijks, maandelijks of jaarlijks.

Huidige Cortana Intelligence Suite en Azure ML-prijzen en abonnementen vindt [hier](https://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Ontwikkelingsproces van oplossing
Het ontwikkelingsproces van een vraag naar energie prognose-oplossing worden doorgaans 4 fasen, die allemaal we maken gebruik van cloud-gebaseerde technologieën en services in de Cortana Intelligence Suite.

Dit wordt geïllustreerd in het volgende diagram:

![Slimme Grid cyclus](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

De volgende alinea beschrijving van dit proces stap 4:

1. **Het verzamelen van gegevens** : een geavanceerde op basis van analyseoplossing is gebaseerd op gegevens (Zie **gegevens wat**). Met name als het gaat om predictive analytics en prognoses, we zijn afhankelijk van continue, dynamische stroom van gegevens. In het geval van energie vraagprognose, deze gegevens kan worden opgevraagd rechtstreeks van slimme meters of al op een on-premises-database worden samengesteld. We zijn ook afhankelijk van andere externe gegevensbronnen, zoals weer en temperatuur. Deze continue stroom van gegevens moet worden georganiseerd, gepland en opgeslagen. [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) (ADF) is onze belangrijkste werkpaard voor het uitvoeren van deze taak.
2. **Modellering** – voor nauwkeurige en betrouwbare energie prognoses, moet een (train) ontwikkelen en onderhouden van een geweldige model dat maakt gebruik van de historische gegevens en de betekenis en voorspellende patronen in de gegevens worden geëxtraheerd. Het gebied van Machine Learning (ML) heeft snel groeien met meer geavanceerde algoritmen die regelmatig worden ontwikkeld. Azure ML Studio biedt een optimale ervaring waarmee u kunt gebruikmaken van de meest geavanceerde algoritmen binnen een volledige werkstroom. Werkstroom wordt weergegeven in een diagram van een intuïtieve stroom en de gegevens voor te bereiden, het ophalen van functies, modelleren en model evaluatie bevat. De gebruiker kan op te halen in honderden verschillende modellen die zijn opgenomen in deze omgeving. Aan het einde van deze fase heeft een gegevenswetenschapper een werkende-model dat zich volledig geëvalueerd en klaar voor implementatie.

   Het volgende diagram wordt een afbeelding van een werkstroom:

   ![Modellering van een werkstroom](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)
3. **Implementatie** – met een model werkt echter in de volgende stap is voor implementatie. Hier wordt het model omgezet in een webservice die een RESTful API die gelijktijdig kunnen worden aangeroepen van verschillende verbruik clients via Internet beschikbaar stelt. Azure ML biedt een eenvoudige manier van het implementeren van een model rechtstreeks vanuit de Azure ML Studio met één klik een knop. Het proces voor de hele gebeurt achter de schermen. Deze oplossing kan automatisch worden geschaald om te voldoen aan de vereiste verbruik.
4. **Verbruik** : In deze fase, maken we daadwerkelijk gebruik van het prognosemodel voor het produceren van voorspellingen. Het verbruik worden bepaald vanuit de gebruikerstoepassing van een (*bijvoorbeeld*, dashboard) of rechtstreeks vanuit een operationele systeem zoals vraag/aanbod balancing systeem of een raster optimalisatie-oplossing. Meerdere gebruiksmogelijkheden worden bepaald vanuit één model.

## <a name="data-understanding"></a>Inzicht in gegevens
Na die betrekking hebben op de overwegingen voor het bedrijf (Zie **inzicht in het bedrijf**) van een vraag naar energie prognose-oplossing, zijn we nu klaar om te bespreken het gegevensonderdeel. Een predictive analytics-oplossing is afhankelijk van betrouwbare gegevens. Voor energie vraagprognose, zijn we afhankelijk van historische gegevens met verschillende detailniveaus. Historische gegevens wordt gebruikt als de grondstoffen. Deze worden onderworpen aan een zorgvuldige analyse waarin de gegevenswetenschapper voorspellingsfactoren (ook wel functies genoemd) die kunnen worden geplaatst in een model dat wordt uiteindelijk de vereiste prognoses gegenereerd wordt geïdentificeerd.

In de rest van deze sectie wordt we de verschillende stappen en overwegingen voor het begrijpen van de gegevens en over te brengen naar een bruikbaar formulier beschreven.

### <a name="the-model-development-cycle"></a>De ontwikkelingscyclus Model
Produceert goede prognoses modellen vereist zorgvuldige voorbereiding en planning. Behalve dat het modelleringsproces in meerdere stappen en gericht op één stap tegelijk kunnen aanzienlijk te verbeteren de uitkomst van het gehele proces.

Het volgende diagram illustreert hoe het modelleringsproces kan worden onderverdeeld in meerdere stappen:

![Model ontwikkelingscyclus](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Zoals u kunt zien dat de cyclus bestaat uit zes stappen uit:

* Aanmaak van probleem
* Gegevensopname en -gegevens verkennen
* Gegevens voor te bereiden en feature-engineering
* Modelleren
* Model-evaluatie
* Ontwikkeling

In de rest van deze sectie wordt we de afzonderlijke stappen en items om te overwegen bij elke stap beschreven.

### <a name="problem-formulation"></a>Aanmaak van probleem
Wij kunnen beschouwen als de formulering probleem als de meest kritieke stap een moet uitvoeren voorafgaand aan de implementatie van een predictive analytics-oplossing. We zouden hier het zakelijke probleem transformeren en deze op bepaalde elementen die kunnen worden opgelost door middel van gegevens technieken afbreken. Het is raadzaam om te formuleren van het probleem als een reeks vragen die we graag willen beantwoorden. Hier volgen enkele mogelijke vragen die mogelijk van toepassing is binnen het bereik van energie vraagprognose:

* Wat is de verwachte belasting van een afzonderlijke onderstation in het volgende uur of de dag?
* Op welk moment van de dag wordt mijn raster vraag ondervinden?
* Hoe waarschijnlijk is mijn raster te accepteren van de verwachte piekbelasting?
* Hoeveel energie moet het station power genereren tijdens elk uur van de dag?

Opstellen van deze vragen kan we ons concentreren op de juiste gegevens ophalen en implementeren van een oplossing die volledig is afgestemd op het zakelijke probleem bij de hand. Bovendien kunnen we enkele belangrijke metrische gegevens die we kunnen de prestaties van het model evalueren wordt ingesteld. Bijvoorbeeld, hoe nauwkeurig de prognose moeten en wat is het bereik van de fout die nog steeds acceptabel door het bedrijf?

### <a name="data-sources"></a>Gegevensbronnen
Het moderne slimme raster verzamelt gegevens uit verschillende onderdelen en onderdelen van het raster. Deze gegevens vertegenwoordigt verschillende aspecten van de bewerking en het gebruik van het raster power. Binnen het bereik van de prognose van vraag naar energie, zijn we de discussie over de gegevensbronnen die overeenkomen met de vraag verbruik te beperken. Een belangrijke bron van het energieverbruik zijn slimme meters. Hulpprogramma's over de hele wereld implementeert snel slimme meters voor hun gebruikers. Slimme meters het werkelijke energieverbruik vastleggen en deze gegevens terug naar het hulpprogramma bedrijf voortdurend relay. Gegevens worden verzameld en verzonden op een vast interval, variërend van elke 5 minuten tot 1 uur weer. Meer geavanceerde slimme meters kunnen op afstand te beheren en het werkelijke verbruik binnen een huishouden in evenwicht worden geprogrammeerd. Smart meter gegevens relatief betrouwbaar is en een tijdstempel bevat. Daardoor is het belangrijk onderdeel toe voor de prognose van vraag. Meter-gegevens kunnen worden geaggregeerd (getotaliseerde) op verschillende niveaus in de topologie grid: transformator, onderstation, regio, *enzovoort*. We kunnen vervolgens het aggregatieniveau van de vereiste op het bouwen van een Voorspellend model voor het kiezen. Bijvoorbeeld, als het bedrijf hulpprogramma wilt voorspellen van toekomstige belasting van elk van de grid substations kunnen vervolgens alle meters gegevens worden samengevoegd voor elke afzonderlijke onderstation en gebruikt als invoer voor het prognosemodel. We verwijzen naar slimme meters als een interne gegevensbron.

Een betrouwbare energie vraagprognose rekenen ook op andere externe gegevensbronnen. Een belangrijke factor die invloed heeft op energieverbruik is het weer, of nauwkeuriger de temperatuur. Historische gegevens vindt u nauwe correlatie tussen externe temperatuur- en energieverbruik. Hot zomer dagen, de consument te maken van hun airconditioning en tijdens de kracht winter op verwarming gebruiken. Een betrouwbare bron van historische temperaturen op de rasterlocatie is daarom sleutel. Bovendien we ook zijn afhankelijk van nauwkeurige prognose van de temperatuur te voorspellen van energieverbruik.

Externe gegevens van andere bronnen kunnen ook helpen bij het bouwen van prognosemodellen energie-aanvraag. Deze tips omvatten bijvoorbeeld wijzigingen in de lange termijn klimaat, voordelige indexen (*bijvoorbeeld*, bbp), en anderen. In dit document nemen niet we deze gegevens van andere bronnen.

### <a name="data-structure"></a>Gegevensstructuur
Na het identificeren van de vereiste gegevens-bronnen, graag willen we om ervoor te zorgen dat de onbewerkte gegevens die zijn verzameld de functies van de juiste gegevens bevat. Voor het bouwen van een betrouwbare aanvraag prognosemodel moet we om ervoor te zorgen dat de verzamelde gegevens gegevenselementen waarmee de toekomstige vraag voorspellen bevat. Hier volgen enkele basisvereisten inzake de gegevensstructuur (schema) van de onbewerkte gegevens.

De onbewerkte gegevens bestaat uit rijen en kolommen. Elke meting wordt weergegeven als één rij met gegevens. Elke rij gegevens bestaat uit meerdere kolommen (ook wel functies of velden genoemd).

1. **Tijdstempel** – het tijdstempelveld geeft de werkelijke tijd waarop de meting is geregistreerd. Het moet voldoen aan een van de algemene datum/tijd-indelingen. Datum- en tijdgegevens onderdelen moeten worden opgenomen. In de meeste gevallen is er niet nodig voor de tijd om te worden geregistreerd totdat het tweede niveau van de granulatie. Het is belangrijk om op te geven van de tijdzone waarin de gegevens worden geregistreerd.
2. **ID meten** -dit veld geeft de meter of het apparaat voor de meting. Het is een categorische variabele en kan bestaan uit een combinatie van cijfers en tekens.
3. **Verbruik waarde** : dit is het werkelijke verbruik op een bepaalde datum/tijd. Het gebruik kan worden gemeten in kWh (kilowatt-hour) of een andere aanbevolen eenheden. Het is belangrijk te weten dat de maateenheid voor alle metingen in de gegevens consistent moet blijven. In sommige gevallen kan meer dan 3 power fasen op verbruik worden opgegeven. In dat geval moet er voor het verzamelen van alle fasen van de onafhankelijke verbruik.
4. **Temperatuur** – de temperatuur doorgaans worden verzameld van een onafhankelijke bron. Het moet echter zijn compatibel met de verbruiksgegevens. Hierbij moet een tijdstempel, zoals hierboven beschreven, zodat deze kunnen worden gesynchroniseerd met de werkelijke verbruiksgegevens. De temperatuurwaarde in graden Celsius of Fahrenheit kan worden opgegeven maar moet blijven consistent op alle metingen.
5. **Locatie –** het locatieveld is doorgaans gekoppeld aan de plaats waar de temperatuurgegevens zijn verzameld. Het kan worden weergegeven als een zip-code number of in breedtegraad/lengtegraad (lat/lang)-indeling.

De volgende tabellen ziet u voorbeelden van een goede verbruik en temperatuur gegevensindeling:

| **datum** | **tijd** | **Meter-ID** | **Fase 1** | **Fase 2** | **Fase 3** |
| --- | --- | --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |ABC1234 |7.0 |2.1 |5.3 |
| 7/1/2015 |10:00:01 |ABC1234 |7.1 |2.2 |4.3 |
| 7/1/2015 |10:00:02 |ABC1234 |6.0 |2.1 |4.0 |

| **datum** | **tijd** | **Locatie** | **temperatuur** |
| --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |11242 |24.4 |
| 7/1/2015 |10:00:01 |11242 |24.4 |
| 7/1/2015 |10:00:02 |11242 |24.5 |

Zoals hierboven kunnen worden weergegeven, wordt in dit voorbeeld 3 verschillende waarden voor verbruik dat is gekoppeld aan 3 power fasen bevat. Houd er ook rekening mee dat de datum en tijd velden worden gescheiden, maar ze kunnen ook worden gecombineerd in één kolom. In dit geval wordt de locatiekolom weergegeven in de indeling van een 5-cijferige zip-code en de temperatuur in een indeling graden Celsius.

### <a name="data-format"></a>Gegevensindeling
Cortana Intelligence Suite kunt ondersteuning voor de meest voorkomende opmaak van gegevens, zoals CSV, TSV, JSON, *enzovoort*. Het is belangrijk dat de indeling van de gegevens consistent voor de volledige levenscyclus van het project blijft.

### <a name="data-ingestion"></a>Gegevensopname
Omdat vraagprognose energie wordt voortdurend en vaak voorspeld, moeten we ervoor zorgen dat de onbewerkte gegevens met behulp van een solide en betrouwbare opname-proces worden doorgestuurd. De opname-proces moet garanderen dat de onbewerkte gegevens beschikbaar voor de prognoses proces op de tijd die nodig is is. Dat betekent dat de gegevensfrequentie van opname groter zijn dan de prognoses frequentie moet.

Bijvoorbeeld: Als onze oplossing voor vraagprognose een nieuwe prognose om 8:00 uur dagelijks genereert vervolgens moeten we ervoor zorgen dat alle gegevens die zijn verzameld tijdens de afgelopen 24 uur volledig zijn opgenomen dat moment tot en om op te nemen zelfs het afgelopen uur gegevens heeft.

Cortana Intelligence Suite biedt hiervoor, verschillende manieren voor ondersteuning van een betrouwbare opname-proces. Dit wordt nader besproken in de **implementatie** sectie van dit document.

### <a name="data-quality"></a>Gegevenskwaliteit
De onbewerkte gegevensbron die is vereist voor het uitvoeren van betrouwbare en nauwkeurige vraagprognose moet voldoen aan bepaalde kwaliteitscriteria basisgegevens. Hoewel geavanceerde statistische methoden kunnen worden gebruikt om te compenseren voor een probleem mogelijk gegevens van kwaliteit opgetreden, moet er nog steeds om ervoor te zorgen dat we enkele basisgegevens kwaliteit drempelwaarde overschrijden bij het ophalen van nieuwe gegevens. Hier volgen enkele overwegingen betreffende de kwaliteit van de onbewerkte gegevens:

* **Ontbrekende waarde** – dit verwijst naar de situatie wanneer bepaalde meting is niet verzameld. De algemene vereiste hier is dat de ontbrekende waarde snelheid niet groter zijn dan 10% voor een bepaalde periode moet. In geval dat een enkele waarde het ontbreekt, moet worden aangegeven met behulp van een vooraf gedefinieerde waarde (bijvoorbeeld: '9999') en niet '0', wat erop kan een geldige waarde.
* **De nauwkeurigheid van de meting** : de werkelijke waarde van de verbruiks- of temperatuur nauwkeurig moet worden geregistreerd. Onjuiste metingen levert onnauwkeurige prognoses. Normaal gesproken moet de meting-fout lager zijn dan 1% ten opzichte van de waarde true.
* **Tijdstip van de meting** : dit is vereist dat de werkelijke tijdstempel van de gegevens die worden verzameld wordt niet meer dan 10 seconden ten opzichte van de waarde true tijd van de werkelijke meting afwijken.
* **Synchronisatie** – wanneer meerdere gegevensbronnen worden gebruikt (*bijvoorbeeld*, verbruik en temperatuur) we moet ervoor zorgen dat er geen tijdsynchronisatie zijn problemen daartussen. Dit betekent dat het tijdsverschil tussen de verzamelde-timestamp van twee onafhankelijke gegevensbronnen mag niet groter zijn dan meer dan 10 seconden.
* **Latentie** - zoals hierboven, in **gegevensopname**, we zijn afhankelijk van een betrouwbare gegevensstroom en opnamesnelheid proces. Om te bepalen die moeten we ervoor zorgen dat we invloed hebben op de gegevenslatentie. Dit is opgegeven als het tijdsverschil tussen de tijd die de werkelijke meting is uitgevoerd en het tijdstip waarop deze is geladen in de Cortana Intelligence Suite-opslag en gereed is voor gebruik. Voor de korte termijn load prognose van de totale latentie niet moet groter zijn dan 30 minuten. Voor de lange termijn load prognose van de totale latentie niet moet groter zijn dan 1 dag.

### <a name="data-preparation-and-feature-engineering"></a>Gegevens voor te bereiden en Feature-Engineering
Nadat de onbewerkte gegevens heeft die is opgenomen (Zie **gegevensopname**) en is veilig opgeslagen, is het gereed om te worden verwerkt. De voorbereidingsfase gegevens is in feite houdend met de onbewerkte gegevens en (transformeren, omvormen) converteren naar een formulier voor het maken van modellering-fase. Die eenvoudige bewerkingen zoals het gebruik van de onbewerkte gegevenskolom is met de werkelijke waarde van de gemeten, gestandaardiseerde waarden, meer gecompliceerde bewerkingen zoals kan bevatten [tijd achtergebleven](https://en.wikipedia.org/wiki/Lag_operator), enzovoort. De zojuist gemaakte gegevenskolommen worden aangeduid als functies en het proces voor het genereren van deze wordt aangeduid als feature-engineering. Aan het einde van dit proces hebben we een nieuwe gegevensset die is is afgeleid van de onbewerkte gegevens en kan worden gebruikt voor modellen. Daarnaast de voorbereidingsfase gegevens moet voorzichtig zijn met ontbrekende waarden (Zie **gegevenskwaliteit**) en voor hen compenseren. In sommige gevallen moet we ook de gegevens om ervoor te zorgen dat alle waarden worden weergegeven in dezelfde schaal normaliseren.

In deze sectie die wordt een overzicht van de algemene functies voor gegevens die zijn opgenomen in de energie vraagprognose modellen.

**Tijd gestuurde functies:** Deze functies zijn afgeleid van de gegevens van de datum/tijdstempel. Deze zijn geëxtraheerd en geconverteerd naar categorische functies zoals:

* Tijd van de dag – dit het uur van de dag waarbij waarden tussen 0 en 23 is
* Dag van week – dit staat voor de dag van de week en waarden van 1 (zondag) tot en met 7 (zaterdag)
* Dag van maand – dit de werkelijke datum weergeeft en kan duren voordat de waarden van 1 tot en met 31
* Maand van jaar – dit staat voor de maand en waarden van 1 (januari) en 12 (December)
* Weekend: dit is een binaire waarde-functie die de waarden van 0 voor doordeweekse dagen of 1 voor weekend
* Vakantiedag - dit is een binaire waarde-functie die de waarden van 0 voor een normale dag of 1 voor een feestdag
* Fourier voorwaarden: de voorwaarden Fourier zijn gewichten die zijn afgeleid van de tijdstempel en worden gebruikt om vast te leggen van de periodieke variatie (cycli) in de gegevens. Omdat we meerdere seizoenen in onze gegevens hebben mogelijk moet er mogelijk meerdere Fourier voorwaarden. Waarden van de aanvraag kunnen bijvoorbeeld jaarlijkse wekelijks en dagelijks seizoenen /-cycli die tot 3 Fourier voorwaarden leidt.

**Onafhankelijke meting functies:** De onafhankelijke functies omvatten alle gegevenselementen die we graag gebruiken als variabelen in het model. Hier wordt de afhankelijke functie die we om te voorspellen moet uitsluiten.

* De functie lag: dit zijn tijd verschoven waarden van de vraag. De functies lag 1 bevatten bijvoorbeeld de waarde van de aanvraag in het vorige uur (ervan uitgaande dat de gegevens per uur) ten opzichte van de huidige tijdstempel. Op deze manier kunnen we lag 2 toevoegen, 3, vertraging *enzovoort*. De werkelijke combinatie van lag-functies die worden gebruikt worden tijdens de fase modellering is bepaald door de evaluatie van de modelresultaten.
* Lange termijn trends: deze functie vertegenwoordigt de lineaire groei in de vraag tussen jaar.

**Afhankelijke functie:** Het afhankelijke onderdeel is de kolom met gegevens die we graag onze model om te voorspellen. Met [onder supervisie van machine learning](https://en.wikipedia.org/wiki/Supervised_learning), moeten we eerst het model met behulp van de afhankelijke onderdelen (dit wordt ook wel labels) te trainen. Hiermee wordt het model voor meer informatie over de patronen in de gegevens die zijn gekoppeld aan het afhankelijke onderdeel. In de vraag naar energie voorspellen willen we meestal om te voorspellen van de vraag en daarom gebruiken we dit als het afhankelijke onderdeel.

**Verwerking van de ontbrekende waarden:** Tijdens de voorbereidingsfase gegevens moet we de beste strategie voor het afhandelen van ontbrekende waarden te bepalen. Dit gebeurt meestal met behulp van de verschillende statistische [toerekening metody](https://en.wikipedia.org/wiki/Imputation_\(statistics\)). In het geval van energie-on-demand prognose rekenen we meestal ontbrekende waarden met behulp van zwevend gemiddelde van de vorige punten van de beschikbare gegevens.

**Normalisering van gegevens:** Gegevensnormalisatie is een ander type transformatie die wordt gebruikt voor alle numerieke gegevens, zoals vraagprognose overzetten naar een soortgelijke schaal. Dit doorgaans verbetert de nauwkeurigheid van model en de precisie. We zouden dit meestal doen door de werkelijke waarde te delen door het bereik van de gegevens.
Hiermee wordt de oorspronkelijke waarde omlaag schalen in een kleiner bereik, meestal tussen 1 en 1.

## <a name="modeling"></a>Modelleren
De fase modellering is waar de conversie van de gegevens in een model plaatsvindt. In de kern van dit proces er zijn geavanceerde algoritmen die de historische gegevens (trainingsgegevens) scannen, patronen ophalen en een model bouwen. Dit model kan later worden gebruikt om te voorspellen op nieuwe gegevens die niet is gebruikt om het model te bouwen.

Zodra er een betrouwbare werkmodel we kan vervolgens worden gebruikt om te scoren van nieuwe gegevens die is opgebouwd om op te nemen van de vereiste onderdelen (X). Het scoring-proces maakt gebruik van de persistente model (object van de fase training) en het voorspellen van de doelvariabele die wordt aangeduid met Ŷ.

### <a name="demand-forecasting-modeling-techniques"></a>Vraagprognose technieken modelleren
In het geval van de vraag voorspelt maken we gebruik van historische gegevens die zijn geordend op basis van tijd. We in het algemeen verwijst naar gegevens die met de tijddimensie als [time series](https://en.wikipedia.org/wiki/Time_series). Het doel in time series modellering is te vinden wanneer gerelateerde trends, seizoensgebondenheid, auto-correlatie (correlatie na verloop van tijd), en die in een model te formuleren.

In de afgelopen jaren zijn geavanceerde algoritmen ontwikkeld voor time series-prognoses en de nauwkeurigheid van prognoses verbeteren. We beschreven enkele van deze hier kort.

> [!NOTE]
> In deze sectie is niet bedoeld om te worden gebruikt als een machine learning en voorspeld overzicht, maar in plaats van als een korte enquête van het modelleren van technieken die vaak worden gebruikt voor de prognose van vraag. Voor meer informatie en educatieve materiaal over time series-prognoses, is het raadzaam het online boek [Forecasting: principes en praktijken](https://www.otexts.org/).
>
>

#### <a name="ma-moving-average"></a>**MA (zwevend gemiddelde)**
Zwevend gemiddelde is een van de eerste analytische technieken die is gebruikt voor time series-prognoses en is nog steeds een van de meest gangbare technieken vanaf vandaag. Het is ook de basis voor meer geavanceerde technieken prognose. Met het zwevend gemiddelde zijn we het volgende gegevenspunt gemiddelde over de meest recente punten K, waarbij K de volgorde van de zwevend gemiddelde geeft van de prognose.

De zwevend gemiddelde techniek is het effect van de prognose-aanpassing en daarom kan niet worden verwerkt en grote volatiliteit van de gegevens.

#### <a name="ets-exponential-smoothing"></a>**ETS (exponentieel vloeiend maken)**
Exponentieel vloeiend maken (ETS) is een productreeks met verschillende methoden die gewogen gemiddelde van recente gegevenspunten gebruiken om te voorspellen van de volgende gegevenspunt. Het idee is hoger gewicht toewijzen aan meer recente waarden en dit gewicht voor oudere meetwaarden geleidelijk te verlagen. Er zijn een aantal verschillende manieren met deze serie, enkele van deze verwerking van de periodieke variatie zijn in de gegevens, zoals seizoensgebonden methode Holt-Winters.

Sommige van deze methoden factor ook in de periodieke variatie van de gegevens.

#### <a name="arima-auto-regression-integrated-moving-average"></a>**ARIMA (automatisch regressie Integrated Moving Average)**
Automatisch regressie geïntegreerde zwevend gemiddelde (ARIMA) is een andere serie van methoden die wordt doorgaans gebruikt voor het maken van prognoses tijdreeksen. Het automatisch regressie methoden voor het praktisch combineert met zwevend gemiddelde. Automatisch regressie methoden maken gebruik van regressiemodellen door vorige tijdwaarden van de serie als u wilt de volgende datum punt compute. ARIMA methoden differentiërende methoden, zoals het verschil tussen de gegevenspunten berekenen en u deze in plaats van de oorspronkelijke gemeten waarde ook van toepassing. Ten slotte ARIMA ook maakt gebruik van de zwevend gemiddelde technieken die hierboven worden beschreven. De combinatie van al deze methoden op verschillende manieren is wat de familie van ARIMA methoden bouwt.

ETS en ARIMA zijn tegenwoordig gebruikte voor vraagprognose energie en tal van andere prognoses problemen. In veel gevallen deze worden gecombineerd samen aan het leveren van zeer nauwkeurige resultaten.

**Algemene meerdere regressies** regressiemodellen kunnen worden de belangrijkste benadering van modellering binnen het domein van machine learning en statistieken. In de context van de tijdreeks we regressie gebruiken om de toekomstige waarden te voorspellen (*bijvoorbeeld*, van aanvraag). In de regressie we een lineair combinatie van de voorspellingsfactoren en informatie over het gewicht (ook wel coëfficiënten genoemd) van de voorspellingsfactoren tijdens de training. Het doel is voor het produceren van een lijn die de waarde voor de voorspelde wordt verwacht. Regressie methoden zijn geschikt als de doelvariabele numeriek moet zijn en dus ook geschikt is voor time series-prognoses. Er is een breed scala aan regressie methoden, met inbegrip van zeer eenvoudig regressiemodellen zoals [lineaire regressie](https://en.wikipedia.org/wiki/Linear_regression) en meer resources, zoals beslisbomen, geavanceerde [willekeurige Forests](https://en.wikipedia.org/wiki/Random_forest), [Neural Netwerken](https://en.wikipedia.org/wiki/Artificial_neural_network), en heeft de Klantenopbrengst Beslisbomen.

Maken van de vraag naar energie voorspellen als regressieprobleem biedt ons een grote flexibiliteit bij het selecteren van onze functies die kunnen worden gecombineerd in de vraag time series-gegevens en externe factoren zoals temperatuur. Meer informatie over de geselecteerde onderdelen worden beschreven in de functie-Engineering (Zie **gegevens voor te bereiden en functie-Engineering**) sectie van dit playbook.

Uit onze ervaring met het implementeren en implementatie van energie aanvraag prognoses pilot, we hebben ontdekt dat dat de geavanceerde regressiemodellen die beschikbaar in Azure ML zijn doorgaans de beste resultaten opleveren en we maken gebruik van deze.

## <a name="model-evaluation"></a>Model-evaluatie
Evaluatie van het model is een belangrijke rol binnen de **Model ontwikkelingscyclus**. Bij deze stap kijken we het model en de prestaties met echte leven gegevens valideren. Tijdens het maken van modellering stap gebruiken we een deel van de beschikbare gegevens voor het model te trainen. Tijdens de evaluatiefase nemen we de rest van de gegevens voor het testen van het model. Vrijwel, betekent dit dat we deze uit het model nieuwe gegevens die is is geherstructureerd en bevat de dezelfde functies als de gegevensset training. Echter tijdens het validatieproces gebruiken we het model om te voorspellen van de doelvariabele in plaats van de beschikbare doelvariabele bieden. Vaak verwijzen we naar dit proces als het model scoren. We zouden gebruikt u de doelwaarden waar en deze te vergelijken met de voorspelde waarden. Het doel hier is om te meten en Minimaliseer de voorspelling fout, wat betekent dat het verschil tussen de voorspellingen en de waarde true. Kwantificeren de meting van de fout is erg belangrijk omdat we willen het model afstemmen en valideren of de fout daadwerkelijk afneemt. Bezig met het aanpassen van het model kan worden gedaan door het Modelparameters waarmee het leerproces te wijzigen of toevoegen of verwijderen van functies voor (aangeduid als [parameters vegen](https://channel9.msdn.com/Blogs/Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Vrijwel dat betekent dat we moet mogelijk herhalen tussen de feature-engineering, modellerings- en evaluatie fasen model meerdere keren totdat we kunnen om te beperken van de fout op het vereiste niveau.

Het is belangrijk om te benadrukken dat de fout voorspelling kan niet nul als er wordt nooit een model dat perfect elke resultaten kan voorspellen. Er is echter een bepaalde grootte van de fout die aanvaardbaar is door het bedrijf. Tijdens het validatieproces, willen we graag om ervoor te zorgen dat onze model prediction fout op het niveau of hoger is dan het tolerantieniveau business. Het is daarom belangrijk om in te stellen van het niveau van de toegestane fout aan het begin van de cyclus tijdens de **probleem formulering** fase.

### <a name="typical-evaluation-techniques"></a>Typische evaluatietechnieken
Er zijn verschillende manieren in welke voorspelling fout kan worden gemeten en (gekwantificeerde). In deze sectie gaan we de discussie over de evaluatietechnieken relevante tijdreeksen en met specifieke voor vraag naar energie voorspellen.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE staat voor Mean Absolute Error voor Percentage. Met MAPE zijn we het verschil tussen cloudcomputing elk punt en de werkelijke waarde van dat moment prognose. We vervolgens kwantificeren de fout per punt door het berekenen van de verhouding van het verschil ten opzichte van de werkelijke waarde. Tijdens de laatste stap gemiddelde we deze waarden. De rekenkundige formule gebruikt voor MAPE is het volgende:

![MAPE formule](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*waar een<sub>t</sub> is de werkelijke waarde, F<sub>t</sub> is de voorspelde waarde en n de prognose horizon.*

## <a name="deployment"></a>Implementatie
Nadat we hebben, kunt u de model-fase en de modelprestaties die we staan klaar om te gaan naar de implementatiefase gevalideerd. In deze context betekent implementatie inschakelen van de klant gebruiken voor het model door de werkelijke voorspellingen die erop worden uitgevoerd op grote schaal. Het concept van de implementatie is de sleutel in Azure ML sinds onze belangrijkste doel is om aan te roepen voortdurend voorspellingen in plaats van alleen het verkrijgen van het inzicht van de gegevens. De implementatiefase is het gedeelte waar we in staat stellen het model om te worden verbruikt op grote schaal.

In de context van energie-vraagprognose is ons doel om aan te roepen doorlopende en periodieke prognoses terwijl ervoor te zorgen dat er nieuwe gegevens beschikbaar zijn voor het model is en dat de verwachte gegevens worden verzonden naar de client in beslag nemen.

### <a name="web-services-deployment"></a>Implementatie van Web Services
De belangrijkste implementeerbare bouwsteen in Azure ML is de web-service. Dit is de meest efficiënte manier om in te schakelen verbruik van een Voorspellend model in de cloud. De webservice bevat het model en afgerond met een [RESTful](https://www.restapitutorial.com/) API (Application Programming Interface). De API kan worden gebruikt als onderdeel van een clientcode, zoals wordt geïllustreerd in het onderstaande diagram.

![We-Service-implementatie en het verbruik](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Zoals u kunt zien, wordt de web-service wordt geïmplementeerd in de cloud voor Cortana Intelligence Suite en kan worden aangeroepen via de beschikbaar gemaakte REST-API-eindpunt. Ander type clients in verschillende domeinen kunt tegelijkertijd de service via de Web-API aanroepen. De webservice kunt ook schalen ter ondersteuning van duizenden gelijktijdige aanroepen.

### <a name="a-typical-solution-architecture"></a>Een typische oplossing-architectuur
Wanneer u een vraag naar energie prognose-oplossing implementeert, zijn we geïnteresseerd in de implementatie van een end-to-end-oplossing die gaat verder dan de voorspellingswebservice en vereenvoudigt het uitvoeren van de volledige gegevensstroom. Op het moment dat we een nieuwe prognose aanroepen, zouden we nodig om ervoor te zorgen dat het model wordt ingevoerd met de functies van de meest recente gegevens. Dat betekent dat de zojuist verzamelde onbewerkte gegevens is voortdurend die zijn opgenomen, verwerkt en in de vereiste functieset omgezet in het model is gebouwd. Op hetzelfde moment graag willen we om de verwachte gegevens beschikbaar voor het end clients verbruikt. Een voorbeeld van de gegevens stroom cyclus (of pijplijn) wordt weergegeven in het onderstaande diagram:

![Vraag naar energie voorspellen End-to-gegevensstroom](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Dit zijn de stappen die uitgevoerd als onderdeel van de prognose energiecyclus voor aanvraag worden:

1. Miljoenen geïmplementeerde meters genereren energieverbruiksgegevens voortdurend in realtime.
2. Deze gegevens worden verzameld en geüpload naar een cloud-opslagplaats (*bijvoorbeeld*, Azure Blob).
3. De onbewerkte gegevens worden samengevoegd tot een onderstation of regionaal niveau voordat deze wordt verwerkt, zoals gedefinieerd door het bedrijf.
4. De functie-verwerking (Zie **gegevens voor te bereiden en verwerken van de functie**) vervolgens plaatsvindt produceert de gegevens die vereist is voor het model en training of scoring – de functie setgegevens worden opgeslagen in een database (*bijvoorbeeld*, SQL Azure).
5. De opnieuw training-service wordt aangeroepen met het opnieuw trainen van het prognosemodel – dat de nieuwe versie van het model worden bewaard, zodat deze kan worden gebruikt door de scoring-webservice.
6. De scoring-webservice wordt aangeroepen op een planning die geschikt is voor de vereiste frequentie van de prognose.
7. De verwachte gegevens worden opgeslagen in een database die kan worden geopend door de end-verbruik-client.
8. De client verbruik worden opgehaald van de prognoses, past deze weer in het raster en in overeenstemming met de vereiste use-case worden verbruikt.

Het is belangrijk te weten dat deze volledige cyclus is volledig geautomatiseerd en volgens een schema wordt uitgevoerd. De volledige indeling van de cyclus van deze gegevens kan worden gedaan met behulp van hulpprogramma's zoals [Azure Data Factory](https://azure.microsoft.com/services/data-factory/).

### <a name="end-to-end-deployment-architecture"></a>Implementatie van de end-to-architectuur
We moeten nagenoeg implementeert een energie demand prognose-oplossing op Cortana Intelligence, zorg ervoor dat de vereiste onderdelen zijn vastgesteld en correct geconfigureerd.

Het volgende diagram illustreert een typische architectuur voor Cortana Intelligence op basis van die worden geïmplementeerd en stuurt de stroom-cyclus voor gegevens die hierboven beschreven:

![Implementatie van de end-to-architectuur](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Raadpleeg de energie-oplossingssjabloon voor meer informatie over elk van de onderdelen en de hele architectuur.

