---
title: Azure IoT Hub hoge beschikbaarheid en herstel na noodgevallen | Microsoft Docs
description: Beschrijft de functies voor Azure en IoT-Hub die u helpen bij het ontwikkelen van maximaal beschikbare Azure-IoT-oplossingen met disaster recovery-mogelijkheden.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: rkmanda
ms.openlocfilehash: 22c53a85d46cbcb1ed9592d7373ea044e8910297
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248295"
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT Hub hoge beschikbaarheid en herstel na noodgevallen
Als eerste stap voor het implementeren van een robuuste IoT moeten-oplossing, architecten, ontwikkelaars en eigenaren van bedrijven definiëren de actieve tijdsduur van de doelstellingen voor de oplossingen die ze bouwen Deze doelstellingen kunnen voornamelijk op basis van specifieke zakelijke doelstellingen voor elk scenario worden gedefinieerd. In deze context, het artikel [technische richtlijnen voor Azure Business Continuity]( https://docs.microsoft.com/azure/architecture/resiliency/) beschrijft een algemene framework waarmee u kunt denken over zakelijke continuïteit en herstel na noodgevallen. De [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](_https://msdn.microsoft.com/library/dn251004.aspx) document bevat architectuurrichtlijnen voor strategieën voor Azure-toepassingen om hoge beschikbaarheid (HA) en herstel na noodgevallen (DR) te realiseren. 

Dit artikel worden de HA en DR-functies die specifiek door de IoT Hub-service worden aangeboden. De brede gebieden die worden beschreven in dit artikel zijn 

- HA intra-regio
- Regio-overkoepelende DR
- Regio-overkoepelende HA bereiken

Afhankelijk van de doelstellingen van uw actieve tijdsduur u voor uw IoT-oplossingen definieert, moet u bepalen welke van de opties worden hieronder beschreven aanbevolen gesorteerde uw bedrijfsdoelstellingen. Een van deze alternatieven HA-/ DR opnemen in uw IoT-oplossing vereist een zorgvuldige evaluatie van de wisselwerking tussen de:
- Tolerantieniveau die u nodig hebt 
- Implementatie en onderhoud complexiteit
- COGS impact


## <a name="intra-region-ha"></a>HA intra-regio
De IoT Hub-service biedt intra-regio HA door het implementeren van redundantie in bijna alle lagen van de service. De [SLA gepubliceerd door de IoT Hub-service](_https://azure.microsoft.com/support/legal/sla/iot-hub) wordt bereikt door gebruik van deze redundantie. Er zijn geen extra werk is vereist voor de ontwikkelaars van een IoT-oplossing om te profiteren van deze HA-functies. Hoewel IoT-Hub een redelijk beschikbaarheidsgarantie biedt, kunnen nog steeds tijdelijke fouten worden verwacht net als bij elk platform voor gedistribueerde computing. Als u net begint met het migreren van uw oplossingen naar de cloud van een on-premises-oplossing, uw focus moet wilt verschuiven van optimaliseren "gemiddelde tijd tussen storingen" 'gemiddelde tijd om te herstellen'. Met andere woorden, zijn tijdelijke fouten normale worden overwogen bij het werken met de cloud in de samenstelling. Juiste [beleid voor opnieuw proberen](_https://channel9.msdn.com/Shows/Internet-of-Things-Show/Retry-logic-in-device-SDKs-for-Azure-IoT-Hub) moet zijn ingebouwd in de onderdelen die interactie met een cloudtoepassing te bekommeren om tijdelijke fouten. 

> [!NOTE]
> Sommige Azure-services biedt ook extra beveiligingslagen beschikbaarheid binnen een regio door te integreren met [Beschikbaarheidszones (AZs)](_https://docs.microsoft.com/azure/availability-zones/az-overview). AZs worden momenteel niet ondersteund door de IoT Hub-service.

## <a name="cross-region-dr"></a>Regio-overkoepelende DR
Er zijn mogelijk sommige zeldzame gevallen wanneer een datacenter optreedt in de uitgebreide onderbrekingen vanwege stroomstoringen of andere fouten met betrekking tot fysieke activa. Dergelijke gebeurtenissen zijn zeldzaam tijdens die de intra-regio HA-mogelijkheden die hierboven worden beschreven mogelijk niet altijd helpen. IoT Hub biedt meerdere oplossingen voor het herstellen van deze uitgebreide onderbrekingen. De opties voor herstel beschikbaar voor klanten in een dergelijke situatie zijn "Microsoft gestart failover" en 'handmatige failover'. De fundamentele verschil tussen de twee is dat Microsoft de voormalige Start en de gebruiker de laatste. Handmatige failover biedt ook een lagere beoogde hersteltijd (RTO) in vergelijking met de failover-optie van Microsoft gestart. De specifieke RTO's die bij elke optie worden aangeboden worden in de onderstaande secties besproken. Wanneer een van deze opties om uit te voeren van failover van een IoT-hub van de primaire regio wordt uitgevoerd, de hub volledig functioneel in de bijbehorende wordt [Azure geokoppeling regio](_https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 


Beide opties voor deze failover bieden de volgende herstelpuntdoelen (RPO's):

| Gegevenstype | Herstelpuntdoelen (RPO) |
| --- | --- |
| ID-register |0-5 minuten gegevensverlies |
| Dubbele apparaatgegevens |0-5 minuten gegevensverlies |
| Cloud-naar-apparaat berichten ** |0-5 minuten gegevensverlies |
| Bovenliggende ** en apparaat-taken |0-5 minuten gegevensverlies |
| Apparaat-naar-cloud-berichten |Alle ongelezen berichten gaan verloren |
| Bewerkingen controleren van berichten |Alle ongelezen berichten gaan verloren |
| Van cloud-naar-apparaat-Feedbackberichten |Alle ongelezen berichten gaan verloren |

Nadat de failoverbewerking voor de IoT hub is voltooid, worden alle bewerkingen van het apparaat en back-end-toepassingen verwacht blijven werken zonder een handmatige tussenkomst.

> [!CAUTION]
> - De Event Hub-compatibele naam en het eindpunt van de IoT Hub-eindpunt voor ingebouwde gebeurtenissen wijzigen na een failover. Wanneer er berichten over telemetrie ontvangen van het geïntegreerde eindpunt met behulp van de event hub-client of de event processor host, moet u [de IoT hub-verbindingsreeks gebruiken](_https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-read-builtin#read-from-the-built-in-endpoint) om de verbinding te maken. Dit zorgt ervoor dat uw back endtoepassingen werken blijven zonder handmatige tussenkomst na failover. Als u de Event Hub-compatibele naam en het eindpunt in uw back endtoepassing rechtstreeks gebruiken, moet u opnieuw configureren van uw toepassing door [ophalen van de nieuwe Event Hub-compatibele naam en het eindpunt](_https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-read-builtin#read-from-the-built-in-endpoint) na een failover om door te gaan bewerkingen. 
>
> - Na een failover, kunnen de gebeurtenissen verzonden via Event Grid worden gebruikt via de dezelfde abonnementen eerder hebt geconfigureerd, zolang deze Event Grid-abonnementen beschikbaar blijven.
>
> - ** Cloud-naar-apparaat-berichten en bovenliggende taken kunnen niet ophalen hersteld als onderdeel van handmatige failover in de preview-product van deze functie.

### <a name="microsoft-initiated-failover"></a>Failover van de Microsoft gestart
Microsoft gestart failover wordt uitgevoerd door Microsoft in zeer uitzonderlijke gevallen voor failover van alle IoT hubs vanaf een getroffen regio naar de bijbehorende geokoppeling regio. Dit is een standaardoptie (geen enkele manier waarop gebruikers zich afmelden) en zonder tussenkomst van de gebruiker vereist. Microsoft behoudt zich het recht om een van de te bepalen wanneer deze optie zal worden uitgeoefend. Dit mechanisme heeft geen betrekking op een toestemming van de gebruiker voordat de failover van de hub van de gebruiker wordt uitgevoerd. Microsoft gestart failover heeft een beoogde hersteltijd (RTO) van 2-26 uur. De grote RTO is, omdat Microsoft moet de failoverbewerking namens de betrokken klanten in die regio uitvoeren. Als u een minder belangrijke IoT-oplossing die het bestand is tegen een uitvaltijd van ongeveer een dag worden uitgevoerd, is het geschikt voor u een afhankelijkheid op deze optie om te voldoen aan de doelstellingen van de herstel na noodgevallen voor uw IoT-oplossing. De totale tijd voor runtime bewerkingen om te worden volledig operationeel zodra dit proces wordt geactiveerd, wordt beschreven in de sectie 'Tijd hebt om te herstellen'. 

### <a name="manual-failover-preview"></a>Handmatige failover (preview)

Als uw bedrijfsdoelen uptime niet wordt voldaan aan door de RTO bepaalt biedt die failover Microsoft geïnitieerd, moet u overwegen handmatige failover voor het activeren van het failoverproces. De RTO bepaalt met deze optie kan overal worden tussen 10 minuten een paar uur. De RTO is momenteel een functie van het aantal apparaten die zijn geregistreerd op basis van de IoT hub-instantie waarvoor een failover is. U kunt verwachten dat de RTO bepaalt voor een hub die als host fungeert ongeveer 100.000 apparaten zich in de ballpark van 15 minuten. De totale tijd voor runtime bewerkingen om te worden volledig operationeel zodra dit proces wordt geactiveerd, wordt beschreven in de sectie 'Tijd hebt om te herstellen'.

De handmatige failover-optie is altijd beschikbaar voor gebruik, ongeacht of de primaire regio uitvaltijd of niet ondervindt. Deze optie kan daarom mogelijk worden gebruikt om geplande failovers uitvoeren. Een voorbeeld van het gebruik van geplande failovers is periodieke failover oefeningen uitvoeren. Waarschuwing al is dat u de resultaten van de bewerking van een geplande failover in een uitvaltijd voor de hub voor de periode die is gedefinieerd door de RTO bepaalt voor deze optie, en ook resulteert in een verlies van gegevens, zoals gedefinieerd door de bovenstaande RPO-tabel. Instellen van een test van IoT hub-instantie uit te oefenen van de geplande failover-optie regelmatig te vertrouwen winnen in uw vermogen om uw end-to-end-oplossingen en wordt uitgevoerd als een echte noodgeval gebeurt, kunt u ook. 

> [!IMPORTANT]
> - Test oefeningen moeten niet worden uitgevoerd op IoT-hubs die worden gebruikt in uw productieomgeving. 
>
> - Handmatige failover moet niet worden gebruikt als een mechanisme voor het migreren van permanent uw hub tussen de Azure geo gekoppelde regio's. In dat geval zou leiden tot een langere latentie voor de bewerkingen worden uitgevoerd op de hub van apparaten homed in de oude primaire regio. 
>
> - Handmatige failover is momenteel in preview en is niet beschikbaar in de volgende Azure-regio's. VS-Oost, VS-West, Noord-Europa, West-Europa, Brazilië-Zuid, VS Zuid-centraal.

### <a name="failback"></a>Failback

Failover wordt teruggestuurd naar de oude primaire regio kan worden bereikt door het activeren van de failover-actie een later tijdstip. Als de oorspronkelijke failoverbewerking is uitgevoerd om te herstellen na een uitgebreide storing in de oorspronkelijke primaire regio, wordt u aangeraden dat de hub moet niet terug naar de oorspronkelijke locatie zodra deze locatie is hersteld van de serviceonderbreking situatie. 

> [!IMPORTANT]
> - Gebruikers mogen alleen uit te voeren 2 failover is uitgevoerd en 2 geslaagde failbackbewerkingen per dag.
>
> - Back-back-failover en failback-bewerkingen zijn niet toegestaan. Gebruikers hebben na afloop van 1 uur tussen deze bewerkingen.

### <a name="time-to-recover"></a>Tijd om te herstellen

Terwijl de FQDN-naam (en dus de connection string) van de IoT hub-instantie de dezelfde na failover blijft, verandert het onderliggende IP-adres. De totale tijd voor de runtime-bewerkingen worden uitgevoerd op uw IoT hub-instantie om te worden volledig operationeel nadat de failoverproces wordt geactiveerd kan daarom worden uitgedrukt met behulp van de volgende functie.

Tijd om te herstellen RTO = [10 min - 2 uur voor handmatige failover | 2-26 uur voor Microsoft failover geïnitieerd] + DNS-doorgifte vertraging + tijd die nodig is door de clienttoepassing verifiëren.%12%0 een in de cache opgeslagen IoT Hub-IP-adres. 

> [!IMPORTANT]
> Het IP-adres van de IoT-hub niet in cache opslaan de IoT-SDK's. Het is raadzaam dat het IP-adres van de IoT-hub niet in de gebruikerscode communicatie met de SDK's moet cache. 

## <a name="achieve-cross-region-ha"></a>Interregionaal HA bereiken
Als uw bedrijfsdoelen uptime niet wordt voldaan aan door de RTO bepaalt dat Microsoft failover gestart of handmatige failover opties bieden, moet u rekening houden met een per apparaat regio-overkoepelende automatische failover-mechanisme implementeren. Een volledige verwerking van de implementatietopologieën in IoT-oplossingen is buiten het bereik van dit artikel. Het artikel behandelt de *regionale failover* implementatiemodel ten behoeve van hoge beschikbaarheid en herstel na noodgevallen.

In een regionale failover-model, de oplossing back-end wordt voornamelijk in één datacenterlocatie. Een secundaire IoT-hub en de back-end zijn geïmplementeerd in een ander datacenterlocatie. Als de IoT-hub in de primaire regio een storing heeft of de netwerkverbinding van het apparaat naar de primaire regio wordt onderbroken, apparaten een secundaire service-eindpunt gebruiken. U kunt de oplossing beschikbaarheid verbeteren door het implementeren van een model interregionale failover in plaats van binnen één regio blijft. 

Op een hoog niveau voor het implementeren van een model regionale failover met IoT Hub, moet u de volgende stappen uit:

* **Een secundaire IoT-hub en apparaat routering logische**: als de service in de primaire regio wordt onderbroken, apparaten verbinding maken met uw secundaire regio moeten beginnen. Gezien de statusbewust aard van de meeste services die betrokken zijn, is het gebruikelijk voor beheerders van de oplossing voor het activeren van het failoverproces tussen regio's. De beste manier om te communiceren van het nieuwe eindpunt naar apparaten, terwijl het beheer van het proces is dat ze regelmatig controleren op een *concierge* service voor het huidige actieve eindpunt. De conciërge-service kan een webtoepassing die wordt gerepliceerd en opgeslagen bereikbaar zijn met behulp van DNS-omleiding technieken (bijvoorbeeld met behulp van [Azure Traffic Manager][Azure Traffic Manager]).

   > [!NOTE]
   > IoT hub-service is niet een ondersteunde eindpunt van het type in Azure Traffic Manager. De aanbeveling is de voorgestelde concierge-service integreren met Azure traffic manager door het implementeren van de eindpunt-statustest API.

* **Replicatie voor Identity-registry**: om te worden gebruikt, de secundaire IoT-hub alle apparaat-id's die u verbinding met de oplossing maken kunnen moet bevatten. De oplossing moet bewaren van back-ups via geo-replicatie van apparaat-id's en upload deze naar de secundaire IoT-hub voordat u overschakelt van de actieve eindpunt voor de apparaten. De exportfunctie van apparaat-id van IoT-Hub is handig in deze context. Zie voor meer informatie, [het Ontwikkelaarshandleiding voor IoT Hub - identiteitsregister][IoT Hub developer guide - identity registry].
* **Samenvoegen van logische**: wanneer de primaire regio weer beschikbaar is, alle de status en gegevens die zijn gemaakt op de secundaire site moeten worden gemigreerd terug naar de primaire regio. Deze status en gegevens, voornamelijk betrekking hebben op apparaat-id's en metagegevens van de toepassing, die moet worden samengevoegd met de primaire IoT-hub en andere toepassingsspecifieke winkels in de primaire regio. Ter vereenvoudiging van deze stap, moet u idempotente bewerkingen. Idempotente bewerkingen Minimaliseer de neveneffecten van de uiteindelijke consistente distributie van gebeurtenissen en van duplicaten of out volgorde bezorging van gebeurtenissen. Bovendien moet de toepassingslogica worden ontworpen te tolereren mogelijke inconsistenties of iets verouderd staat. Deze situatie kan optreden vanwege de extra tijd die nodig is voor het systeem herstel knooppuntservice op basis van de doelstellingen voor herstelpunten (RPO).

## <a name="choose-the-right-hadr-option"></a>Kies de juiste HA-/ DR-optie
Heres een samenvatting van de HA-/ DR-opties die zijn gepresenteerd in dit artikel die kan worden gebruikt als een het referentiekader van de juiste optie die geschikt is voor uw oplossing kiezen

| HA-/ DR-optie | RTO | RPO | Handmatige interventie nodig is? | Complexiteit van de implementatie | Impact van de extra kosten|
| --- | --- | --- | --- | --- | --- | --- |
| Failover van de Microsoft gestart |2 - 26 uur|Raadpleeg de bovenstaande RPO-tabel|Nee|Geen|Geen
| Handmatige failover |10 minuten - 2 uur|Raadpleeg de bovenstaande RPO-tabel|Ja|Zeer laag. U hoeft alleen te activeren met deze bewerking via de portal.|Geen
| Cross-regio HA |< 1 minuut|Afhankelijk van de replicatiefrequentie van uw aangepaste HA-oplossing|Nee|Hoog|> 1 x de kosten van 1 IoT-hub 

## <a name="next-steps"></a>Volgende stappen
Volg deze koppelingen voor meer informatie over Azure IoT Hub:

* [Aan de slag met IoT-Hubs (zelfstudie)][lnk-get-started]
* [Wat is Azure IoT Hub?][What is Azure IoT Hub?]

[Disaster recovery and high availability for Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub developer guide - identity registry]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: quickstart-send-telemetry-dotnet.md
[What is Azure IoT Hub?]: about-iot-hub.md
