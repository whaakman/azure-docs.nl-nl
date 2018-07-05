---
title: bestand opnemen
description: bestand opnemen
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 0eaf1115dc6ed5a7dc9e7354340d5eb529116c88
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37780652"
---
# <a name="internet-of-things-security-from-the-ground-up"></a>Beveiliging van Internet of Things vanaf de grond

Het Internet of Things (IoT) vormt de unieke uitdagingen voor de beveiliging, privacy en naleving voor bedrijven over de hele wereld. In tegenstelling tot traditionele cyber-technologie waar deze problemen draaien om software en hoe deze is geïmplementeerd, IoT heeft betrekking op wat er gebeurt wanneer de cyberaanvallen en de fysieke werelden geconvergeerd. IoT-oplossingen beveiligen, moet ervoor te zorgen dat beveiligde inrichting van apparaten, veilige connectiviteit tussen deze apparaten en de cloud en de bescherming van de beveiligde gegevens in de cloud tijdens verwerking en opslag. Op basis van deze functionaliteit werkt, zijn echter apparaten resource beperkt, geografische verdeling van implementaties en een groot aantal apparaten in een oplossing.

In dit artikel gaat in op hoe de IoT-oplossingsversnellers bieden een veilige en persoonlijke Internet of Things-cloudoplossing. De oplossingsversnellers bieden een volledige end-to-end-oplossing, met beveiliging die is ingebouwd in elke fase van het begin te beginnen. Bij Microsoft, het ontwikkelen van veilige software maakt deel uit van de software-engineering-praktijken, die verankerd ligt in tientallen jaren van Microsoft lange ervaring van het ontwikkelen van veilige software. Om te controleren of dit, is Security Development Lifecycle (SDL) de fundamentele ontwikkelingsmethodologie, in combinatie met een groot aantal infrastructuur beveiligingsniveau services zoals Operational Security Assurance (OSA) en de Microsoft Digital Crimes Unit, Microsoft Security Response Center en Microsoft Malware Protection Center.

De oplossingsversnellers bieden een unieke functies die moet inrichten, verbinding maken met en opslaan van gegevens van IoT-apparaten, eenvoudige en transparante en de meeste van alle, beveiligen. In dit artikel onderzoekt de beveiligingsfuncties van Azure IoT-oplossing accelerators en implementatiestrategieën voor om ervoor te zorgen voor beveiliging, privacy en naleving uitdagingen vallen.

## <a name="introduction"></a>Inleiding

Het Internet of Things (IoT) is de groep van de toekomst, biedt bedrijven direct en echte mogelijkheden om kosten te verlagen, de opbrengst te verhogen en transformeren van hun bedrijf. Veel bedrijven zijn echter liever IoT implementeren in hun organisaties vanwege problemen met beveiliging, privacy en naleving. Er is een belangrijke punt van zorg afkomstig van de uniekheid van de IoT-infrastructuur, die worden samengevoegd de cyberaanvallen en de fysieke werelden, afzonderlijke intrinsieke risico's van deze twee werelden wordt samengesteld. Beveiliging van IoT heeft betrekking op het ervoor te zorgen dat de integriteit van code die wordt uitgevoerd op apparaten, apparaat- en -verificatie, definiëren wissen eigendom van apparaten (evenals gegevens die zijn gegenereerd door deze apparaten) en wordt het bestand is tegen cyberaanvallen en fysieke aanvallen.

Er wordt vervolgens het probleem van privacy. Bedrijven willen transparantie over gegevens te verzamelen, zoals in wat wordt verzameld en waarom, die het kan zien, die verantwoordelijk is voor toegang, enzovoort. Er zijn ten slotte algemene veiligheid van de apparatuur, samen met de mensen die ze en problemen van het onderhouden van industrienormen van naleving.

Gezien de beveiliging, privacy, transparantie en naleving opmerkingen, blijft de juiste provider van de IoT-oplossing kiezen een uitdaging. Samenvoegen samen afzonderlijke stukjes IoT-software en services die door een groot aantal leveranciers introduceert hiaten in de beveiliging, privacy, transparantie en naleving, die mogelijk moeilijk te detecteren, laat staan los. De keuze van de juiste IoT-software en service provider is gebaseerd op het vinden van providers waarvoor uitgebreide ervaring met services, die zakelijke dienstverlening en landen bestrijkt, maar kunnen ook worden gebruikt om te schalen op een veilige en transparante manier. Op deze manier is het nuttig voor de geselecteerde provider hebben tientallen jaren ervaring met het ontwikkelen van veilige software die wordt uitgevoerd op miljarden machines over de hele wereld en de mogelijkheid om op te waarderen van het landschap van cyberveiligheidsbedreigingen die door deze nieuwe wereld van het Internet of Things.

## <a name="secure-infrastructure-from-the-ground-up"></a>Veilige infrastructuur implementeren vanaf het begin te beginnen

De [Microsoft Cloud](https://azure.microsoft.com) infrastructuur ondersteunt meer dan een miljard klanten verspreid over 127 landen. Tekenen van Microsofts tientallen jaren ervaring bij het bouwen van zakelijke software en uitvoering van een van de grootste online services in de hele wereld, biedt de Microsoft-Cloud hogere niveaus van verbeterde beveiliging, privacy, naleving en bedreigingen Risicobeperking voor dan de meeste klanten op hun eigen kunnen realiseren.

De [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/) biedt een verplichte bedrijfsbrede ontwikkelingsproces waarbij beveiligingsvereisten in de gehele levenscyclus van software integreert. Om ervoor te zorgen dat operationele activiteiten dezelfde mate van beveiligingsprocedures volgen, maakt gebruik van SDL krachtige beveiligingsrichtlijnen lay-out van Microsoft Operational Security Assurance (OSA) verwerkt. Microsoft werkt ook met de derde partij auditbedrijven om voortdurend te verifiëren dat het voldoet aan de verplichtingen en praat met Microsoft in breed beveiligingsinspanningen via het maken van de datacenters van hoogwaardige zakelijke prestaties, met inbegrip van de Microsoft Digital Crimes Unit Microsoft Security Response Center en Microsoft Malware Protection Center.

## <a name="microsoft-azure---secure-iot-infrastructure-for-your-business"></a>Microsoft Azure - veilige IoT-infrastructuur voor uw bedrijf

Microsoft Azure biedt een complete cloudoplossing die een voortdurend groeiende verzameling geïntegreerde cloudservices combineert, analytics, machine learning, opslag, beveiliging, netwerken en web, met een toonaangevende rol in de beveiliging en de privacy van uw gegevens. Microsofts [uitgaan van inbreuk](https://azure.microsoft.com/blog/red-teaming-using-cutting-edge-threat-simulation-to-harden-the-microsoft-enterprise-cloud/) strategie maakt gebruik van een toegewezen *rood team* van beveiligingsexperts software die aanvallen simuleren, test de mogelijkheid van Azure te detecteren en bescherming tegen nieuwe bedreigingen en herstellen van schendingen. Microsofts [wereldwijd reageren op incidenten](https://www.microsoft.com/en-us/TrustCenter/Security/DesignOpSecurity) team werkt nacht aan de effecten van aanvallen en schadelijke activiteiten. Het team volgt tot stand gebrachte procedures voor het beheer van incidenten, communicatie en herstel, en maakt gebruik van kunnen worden gedetecteerd en voorspelbare interfaces met interne en externe partners.

De Microsoft-systemen bieden continue inbraakdetectie en preventie, aanvalbeveiliging service, regelmatige indringingstests en forensische hulpprogramma's die helpen bij het identificeren en bedreigingen te verhelpen. [Meervoudige verificatie](../articles/active-directory/authentication/multi-factor-authentication.md) biedt een extra beveiligingslaag voor eindgebruikers toegang hebben tot het netwerk. En voor de toepassing en de host-provider, Microsoft biedt toegangsbeheer, bewaking, anti-malware, scannen op beveiligingsproblemen, patches en Configuratiebeheer.

De oplossingsversnellers te profiteren van de beveiliging en privacy is ingebouwd in het Azure-platform, samen met de processen van SDL en OSA-code voor veilige ontwikkeling en het gebruik van alle Microsoft-software. Deze procedures bieden beveiliging van de infrastructuur, netwerkbeveiliging en identiteit- en beheerfuncties cruciaal belang voor de beveiliging van een oplossing.

De [Azure IoT Hub](../articles/iot-hub/iot-hub-what-is-iot-hub.md) binnen de [IoT-oplossingsversnellers](../articles/iot-accelerators/iot-accelerators-what-is-azure-iot.md) biedt een volledig beheerde service die stabiele en veilige bidirectionele communicatie tussen IoT-apparaten en Azure-services zoals [Azure Machine Learning](../articles/machine-learning/studio/what-is-machine-learning.md) en [Azure Stream Analytics](../articles/stream-analytics/stream-analytics-introduction.md) met behulp van beveiligingsreferenties per apparaat- en toegangsbeheer.

Om te communiceren beste beveiliging en privacy-functies die zijn ingebouwd in de Azure IoT-oplossingsversnellers, wordt in dit artikel het pakket in de drie beveiligingsgebieden van de primaire.

![Azure IoT-oplossingsversnellers](media/iot-security-ground-up/securing-iot-ground-up-fig3.png)

### <a name="secure-device-provisioning-and-authentication"></a>Beveiligde apparaten inrichten en verificatie

De oplossingsversnellers beveiligde apparaten terwijl ze worden uit in het veld door op te geven van een unieke id-sleutel voor elk apparaat, die kan worden gebruikt door de IoT-infrastructuur om te communiceren met het apparaat tijdens deze bewerking. Het proces is snel en gemakkelijk te installeren. De gegenereerde sleutel met een gebruiker geselecteerde apparaat-ID vormt de basis van een token dat wordt gebruikt in alle communicatie tussen het apparaat en de Azure IoT Hub.

Apparaat-id's kunnen worden gekoppeld aan een apparaat tijdens productie (die in een module van de vertrouwensrelatie hardware is, geknipperd) of een bestaande vaste identiteit als een proxy (bijvoorbeeld CPU-serienummers) kunnen gebruiken. Omdat het wijzigen van deze identificatiegegevens in het apparaat is niet eenvoudig, is het belangrijk om te introduceren logische apparaat-id's in het geval de onderliggende apparaat hardware hebt aangebracht, maar het logische apparaat blijft hetzelfde. In sommige gevallen kan de koppeling van een apparaat-id kan gebeuren tijdens de implementatie apparaat (bijvoorbeeld, een technicus geverifieerde veld fysiek configureert u een nieuw apparaat tijdens het communiceren met de oplossing back-end). De [Azure IoT Hub-identiteitsregister](../articles/iot-hub/iot-hub-devguide.md) beveiligde opslag van apparaat-id's en sleutels voor een oplossing biedt. Persoon of groepen van apparaat-id's kunnen worden toegevoegd aan een lijst met toegestane of een lijst met geblokkeerde websites volledige controle over toegang tot het apparaat inschakelen.

Azure IoT Hub toegangscontrolebeleid in de cloud inschakelen activering en uitschakelen van een apparaat-id, bieden een manier om het loskoppelen van een apparaat van een IoT-implementatie indien nodig. Deze koppeling en loskoppelen van apparaten is gebaseerd op elk apparaat-id.

Beveiliging van aanvullende apparaatfuncties zijn onder andere:

* Apparaten accepteren geen ongevraagde netwerkverbindingen. Ze stellen alle verbindingen en routes op een manier alleen uitgaand verkeer. Voor een apparaat een opdracht van de back-end ontvangt, moet het apparaat opnieuw verbinding maken om te controleren of er nog in behandeling opdrachten om te verwerken. Zodra een verbinding tussen het apparaat en IoT-Hub veilig is gemaakt, op het apparaat-berichten vanuit de cloud en apparaat naar de cloud kan transparant worden verzonden.
* Apparaten alleen verbinding maken met of routes naar bekende services waarmee ze aan elkaar zijn gekoppeld, zoals een Azure IoT Hub tot stand brengen.
* Niveau van het bestandssysteem autorisatie en verificatie per apparaat-id's, waardoor referenties voor toegang en machtigingen in de buurt gebruiken-onmiddellijk worden ingetrokken.

### <a name="secure-connectivity"></a>Beveiligde verbindingen

Duurzaamheid van berichten is een belangrijk onderdeel van een IoT-oplossing. De noodzaak tot blijvend opdrachten leveren en/of gegevens van apparaten ontvangen wordt door het feit dat de IoT-apparaten zijn verbonden via het Internet of andere vergelijkbare netwerken die kunnen worden onbetrouwbare onderstreept. Azure IoT Hub biedt duurzaamheid van de uitwisseling van berichten tussen cloud en apparaten via een systeem van bevestigingen in reactie op berichten. Extra duurzaamheid voor berichten wordt bereikt door het in cache plaatsen van berichten in de IoT-Hub voor maximaal zeven dagen voor Telemetrie en twee dagen voor opdrachten.

Efficiëntie is het belangrijk om ervoor te zorgen, behoud van resources en de bewerking in een omgeving met beperkte capaciteit. HTTPS (HTTP Secure), de standaard-beveiligde versie van het populaire http-protocol wordt ondersteund door Azure IoT Hub, waardoor efficiënte communicatie. Geavanceerde Message Queuing Protocol (AMQP) en Message Queuing-telemetrie Transport (MQTT), ondersteund door Azure IoT Hub, zijn ontworpen niet alleen voor wat betreft het gebruik van bronnen, maar ook betrouwbare bezorging van berichten-efficiëntie. 

Schaalbaarheid vereist de mogelijkheid om veilig samenwerken met een breed scala aan apparaten. Azure IoT hub kunt beveiligde verbinding op apparaten met zowel IP is ingeschakeld en niet IP is ingeschakeld. IP-compatibele apparaten kunnen rechtstreeks verbinding maken en communiceren met de IoT-Hub via een beveiligde verbinding. Niet-IP-compatibele apparaten zijn resource-beperking en maak verbinding over korte afstand communicatieprotocollen, zoals Zwave ZigBee en Bluetooth. Een veldgateway is die wordt gebruikt om deze apparaten en voert protocolconversie uit om in te schakelen veilige bidirectionele communicatie met de cloud.

Beveiligingsfuncties voor extra verbinding zijn onder andere:

* Het communicatiepad tussen apparaten en Azure IoT Hub, of tussen gateways en Azure IoT Hub is beveiligd met behulp van industriestandaard Transport Layer Security (TLS) met Azure IoT Hub is geverifieerd met x.509-protocol.
* Als u wilt voorkomen dat apparaten ongevraagde binnenkomende verbindingen, Azure IoT Hub een verbinding met het apparaat niet geopend. Het apparaat wordt alle verbindingen.
* Azure IoT Hub blijvend berichten voor apparaten worden opgeslagen en wacht tot het apparaat verbinding maakt. Deze opdrachten worden opgeslagen voor twee dagen inschakelen van apparaten die verbinding maken sporadisch, vanwege de stroom of de connectiviteit problemen, om deze opdrachten te ontvangen. Azure IoT Hub onderhoudt een wachtrij per apparaat voor elk apparaat.

### <a name="secure-processing-and-storage-in-the-cloud"></a>Verwerking en opslag in de cloud beveiligen

Van versleutelde berichten te verwerken van gegevens in de cloud helpen de oplossingsversnellers gegevens te beveiligen. Dit biedt flexibiliteit voor het implementeren van aanvullende versleuteling en het beheer van sleutels.

Met behulp van Azure Active Directory (AAD) voor verificatie en autorisatie, krijgt Azure IoT-oplossingsversnellers u een autorisatiemodel op basis van beleid voor gegevens in de cloud, zodat u eenvoudig toegang kunt beheren die kan worden gecontroleerd en beoordeeld. Dit model kan ook een vrijwel directe intrekken van toegang tot gegevens in de cloud, en apparaten die zijn verbonden met de Azure IoT-oplossingsversnellers.

Zodra de gegevens zich bevinden in de cloud, kan worden verwerkt en opgeslagen in een door de gebruiker gedefinieerde werkstroom. Toegang tot elk onderdeel van de gegevens wordt beheerd met Azure Active Directory, afhankelijk van de storage-service gebruikt.

Alle sleutels die worden gebruikt door de IoT-infrastructuur zijn opgeslagen in de cloud in veilige opslag met de mogelijkheid om te vernieuwen als sleutels moeten worden ingericht. Gegevens kunnen worden opgeslagen [Azure Cosmos DB](../articles/cosmos-db/introduction.md) of in [SQL-databases](../articles/sql-database/sql-database-faq.md), het inschakelen van de definitie van het gewenste beveiligingsniveau. Daarnaast biedt Azure een manier om te bewaken en analyseren van alle toegang tot uw gegevens om te waarschuwen u van een indringing of onbevoegde toegang.

## <a name="conclusion"></a>Conclusie

Het Internet of Things begint met uw dingen, de dingen die het belangrijkst zijn voor bedrijven. IoT kunt geweldige waarde leveren voor een bedrijf kosten te verlagen, omzet te vergroten en business transformeren. Succes van deze transformatie is grotendeels afhankelijk is van het kiezen van de juiste IoT-software en service provider. Dit betekent dat zoeken naar een provider die niet alleen catalyzes deze transformatie door informatie over zakelijke behoeften en vereisten, maar ook biedt services en software die zijn gebouwd met beveiliging, privacy, transparantie en naleving als belangrijke ontwerpoverwegingen. Microsoft heeft uitgebreide ervaring met het ontwikkelen en implementeren van veilige software en services en blijft een leider is geselecteerd in deze nieuwe manier van Internet of Things.

De oplossingsversnellers ontwikkelen in beveiligingsmaatregelen standaard inschakelen van beveiligde bewaking van de activa voor het verbeteren van de efficiëntie, operationele prestaties station innovatie mogelijk en geavanceerde gegevensanalyse gebruiken om te zetten bedrijven. Met de gelaagde benadering voor beveiliging, meerdere beveiligingsfuncties en ontwerppatronen helpen de oplossingsversnellers implementeren van een infrastructuur die vertrouwd worden kan voor elk bedrijf te transformeren.

## <a name="additional-information"></a>Aanvullende informatie

Elke oplossingsverbetering maakt exemplaren van Azure-services, zoals:

* [**Azure IoT Hub**](https://azure.microsoft.com/services/iot-hub/): de gateway die de cloud met apparaten verbindt. U kunt schalen naar miljoenen verbindingen per hub en verwerken grote hoeveelheden gegevens met ondersteuning voor verificatie per apparaat helpt beveiligen van uw oplossing.
* [**Azure Cosmos DB**](https://azure.microsoft.com/services/cosmos-db/): een schaalbare, volledig geïndexeerd databaseservice voor de semi-gestructureerde gegevens die worden beheerd metagegevens voor de apparaten die u inricht, zoals kenmerken, configuratie en de eigenschappen voor beveiliging. Azure Cosmos DB biedt hoge prestaties en hoge doorvoer verwerkt, schema-agnostische indexering van gegevens en een geavanceerde SQL-QueryInterface.
* [**Azure Stream Analytics**](https://azure.microsoft.com/services/stream-analytics/): realtime-verwerking in de cloud waarmee u snel ontwikkelen en implementeren van een goedkope analyseoplossing realtime inzichten kunt van apparaten, sensoren, infrastructuur en toepassingen . De gegevens uit deze volledig beheerde service kan worden geschaald naar een volume behoud van hoge doorvoer, lage latentie en tolerantie.
* [**Azure App Services**](https://azure.microsoft.com/services/app-service/): een cloudplatform voor het bouwen van krachtige web- en mobiele apps die verbinding met gegevens waar dan ook; in de cloud of on-premises maken. Aansprekende mobiele apps bouwen voor iOS, Android en Windows. Integreren met uw Software as a Service (SaaS) en zakelijke toepassingen met out-of-the-box-connectiviteit met tientallen cloudgebaseerde services en zakelijke toepassingen. Code in uw favoriete taal en IDE – .NET, Node.js, PHP, Python of Java: sneller dan ooit webtoepassingen en API's bouwen.
* [**Logic Apps**](https://azure.microsoft.com/services/app-service/logic/): Logic Apps-functie van Azure App Service helpt uw IoT-oplossing met uw bestaande line-of-business-systemen integreren en automatiseren van werkstroomprocessen worden uitgevoerd. Logic Apps kan ontwikkelaars werkstromen ontwerpen die na een trigger worden gestart en voer vervolgens een reeks stappen, regels en acties die gebruikmaken van krachtige connectors om te integreren in uw bedrijfsprocessen. Logic Apps biedt out-of-the-box verbinding met een uitgebreid ecosysteem van SaaS, op basis van cloud en on-premises toepassingen.
* [**Azure blob-opslag**](https://azure.microsoft.com/services/storage/): betrouwbare, betaalbare cloudopslag voor de gegevens die uw apparaten naar de cloud verzenden.