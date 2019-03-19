---
title: bestand opnemen
description: bestand opnemen
services: iot-fundamentals
author: robinsh
ms.service: iot-fundamentals
ms.topic: include
ms.date: 08/07/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: f3e05f213821b053f8cf6abbbc50a14e9ea62295
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58125098"
---
# <a name="internet-of-things-iot-security-architecture"></a>Internet of Things (IoT)-beveiligingsarchitectuur

Bij het ontwerpen van een systeem, is het belangrijk dat u de potentiële bedreigingen voor dat systeem te begrijpen en dienovereenkomstig, juiste beveiliging toevoegen wanneer het systeem worden ontworpen en ontworpen. Het is belangrijk om te ontwerpen van het product vanaf het begin rekening met beveiliging omdat informatie over hoe een aanvaller kan mogelijk een systeem kunt u ervoor dat de juiste oplossingen worden in plaats van het begin.

## <a name="security-starts-with-a-threat-model"></a>Beveiliging begint met een risicomodel

Microsoft heeft lang bedreigingsmodellen gebruikt voor de producten en van het bedrijf threat modeling proces openbaar beschikbaar heeft gesteld. De ervaring van het bedrijf laat zien dat het model heeft onverwachte voordelen boven het direct inzicht in wat bedreigingen de meeste zijn over. Bijvoorbeeld, maakt het ook een avenue voor een discussie over de openen met anderen buiten het ontwikkelingsteam en wat tot nieuwe ideeën en verbeteringen in het product leiden kan.

Het doel van risicomodel is om te begrijpen hoe een aanvaller kan een systeem en controleer vervolgens of de juiste oplossingen zijn aanwezig kan zijn. Threat modellering Hiermee wordt het ontwerpteam om oplossingen te overwegen als het systeem is ontworpen in plaats van nadat het systeem wordt geïmplementeerd. Dit is van cruciaal belang, omdat ook onrendabel zijn beveiligingen aan een groot aantal apparaten in het veld onbruikbare is, is dit foutgevoelig en laat klanten op risico.

Veel ontwikkelteams werkzaamheden een uitstekende vastleggen van de functionele vereisten voor het systeem waarmee klanten profiteren. Identificeren van niet-duidelijk manieren dat iemand het systeem mogelijk misbruik is echter moeilijker. Risicomodel kunt ontwikkelteams begrijpen wat een aanvaller kan doen en waarom. Risicomodel is een gestructureerde proces waarmee een discussie over de beveiliging ontwerpbeslissingen maakt in het systeem, evenals wijzigingen in het ontwerp dat weg zijn aangebracht die gevolgen-beveiliging. Terwijl een risicomodel gewoon een document is, wordt in deze documentatie ook een ideale manier om ervoor te zorgen voor bedrijfscontinuïteit te waarborgen van de kennis, behoud van de lessen hebt geleerd en help nieuw team snel vrijgeven vertegenwoordigt. Ten slotte is het resultaat van risicomodel waarmee u rekening houden met andere aspecten van beveiliging, zoals welke beveiligingsverplichtingen die u wilt bieden aan uw klanten. Deze verplichtingen in combinatie met risicomodel kennis en station testen van uw Internet of Things (IoT)-oplossing.

### <a name="when-to-do-threat-modeling"></a>Wanneer u moet modellering van bedreigingen

[Risicomodel](https://www.microsoft.com/en-us/sdl/adopt/threatmodeling.aspx) biedt de grootste waarde wanneer u deze in de ontwerpfase opnemen. Wanneer u ontwerpt, hebt u de grootste flexibiliteit om wijzigingen aanbrengen in de bedreigingen te elimineren. Verwijderen van dreigingen standaard is het gewenste resultaat. Het is veel eenvoudiger dan oplossingen toevoegen, testen en ervoor te zorgen dat ze actueel blijven en bovendien die verwijdering is niet altijd mogelijk. Er wordt het moeilijker om te voorkomen van bedreigingen zoals een product meer volwassen wordt en uiteindelijk op zijn beurt vereist meer werk en vaak veel moeilijker compromissen dan threat modeling vroeg in de ontwikkeling.

### <a name="what-to-consider-for-threat-modeling"></a>Wat u moet overwegen voor risicomodel

U ziet op de oplossing als een geheel en ook zijn gericht op de volgende gebieden:

* De beveiliging en privacy-functies
* De functies waarvoor fouten beveiliging relevante zijn
* De functies die grens van een vertrouwensrelatie touch

### <a name="who-performs-threat-modeling"></a>Wie risicomodel uitvoert

Risicomodel is een proces zoals elk ander. Het is een goed idee om het document threat model, zoals elk ander onderdeel van de oplossing worden behandeld en te valideren. Veel ontwikkelteams werkzaamheden een uitstekende vastleggen van de functionele vereisten voor het systeem waarmee klanten profiteren. Identificeren van niet-duidelijk manieren dat iemand het systeem mogelijk misbruik is echter moeilijker. Risicomodel kunt ontwikkelteams begrijpen wat een aanvaller kan doen en waarom.

### <a name="how-to-perform-threat-modeling"></a>Het uitvoeren van risicomodel

De threat modeling proces bestaat uit vier stappen; de stappen zijn:

* De toepassing modelleren
* Het inventariseren van bedreigingen
* Voorkom bedreigingen
* Valideren van de oplossingen

#### <a name="the-process-steps"></a>De stappen

Drie vuistregels waarmee u rekening moet houden bij het bouwen van een risicomodel:

1. Diagram van een buiten-referentiearchitectuur maken.

2. Reikwijdte mobiliteit beginnen. Bekijk een overzicht en inzicht in het systeem als geheel, deep wilt voordat. Deze aanpak zorgt ervoor dat u nader bekeken in de juiste plaatsen worden uitgevoerd.

3. Station van het proces, niet toestaan dat het proces beslissingen. Als u een probleem in de fase modelleren gevonden en wilt verkennen, gaat u voor deze! Hoeft dat u moet deze stappen slavishly.

#### <a name="threats"></a>Bedreigingen

De vier belangrijkste elementen van een risicomodel zijn:

* Processen zoals webservices, Win32-services, en * nix daemons. Sommige complexe entiteiten (bijvoorbeeld veldgateways en sensoren) kunnen worden geabstraheerd als een proces wanneer een technische Inzoomen op deze gebieden niet mogelijk is.

* Gegevensarchieven (overal gegevens worden opgeslagen, zoals een configuratiebestand of database)

* Gegevensoverdracht (waarbij gegevens worden verplaatst tussen de andere elementen in de toepassing)

* Externe entiteiten (Alles die communiceert met het systeem, maar is niet onder het beheer van de toepassing, zijn bijvoorbeeld gebruikers en satelliet feeds)

Alle elementen in het architectuurdiagram zijn afhankelijk van verschillende bedreigingen; in dit artikel de verkorte STRIDE weergave. Lezen [Threat Modeling opnieuw, STRIDE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) voor meer informatie over de STRIDE-elementen.

Er zijn verschillende elementen van een diagram van de toepassing afhankelijk van bepaalde STRIDE bedreigingen:

* Processen zijn afhankelijk van STRIDE
* Gegevensoverdrachten zijn afhankelijk van TID
* Gegevensarchieven zijn onderworpen aan TID en soms R, wanneer de gegevensarchieven logboekbestanden zijn.
* Externe entiteiten zijn afhankelijk van SRD

## <a name="security-in-iot"></a>Beveiliging in IoT

Verbonden apparaten voor speciale doeleinden hebben een groot aantal potentiële interactie surface gebieden en interactie patronen, die allemaal van belang om een framework voor het beveiligen van digitale toegang tot deze apparaten. De term 'digitale toegang' wordt hier gebruikt om te onderscheiden van bewerkingen die worden uitgevoerd via directe apparaat interactie waarbij beveiliging wordt geboden via fysieke toegangsbeheer. Bijvoorbeeld, als het apparaat in een kamer met een vergrendeling op de deur van de. Hoewel fysieke toegang kan niet worden geweigerd met behulp van software en hardware, kunnen maatregelen worden getroffen om te voorkomen dat fysieke toegang ertoe leidt dat voor systeem storing.

Als u de interactie patronen ontdekken, kijken "apparaat controle" en "apparaat" met hetzelfde niveau liggen. 'Apparaatbeheer ' kan worden geclassificeerd als alle gegevens die met het doel van wijzigen of het gedrag van het naar de status of de status van de omgeving invloed op een apparaat wordt geleverd door een partij. 'Apparaatgegevens' kunnen worden geclassificeerd als alle informatie op die een apparaat naar een andere partij over de status en de waargenomen status van de omgeving verzendt.

Als u wilt optimaliseren best practices voor beveiliging, wordt het aanbevolen dat een typische IoT-architectuur is onderverdeeld in meerdere onderdeel/zones als onderdeel van de threat modeling oefening. Deze zones volledig in deze sectie worden beschreven, en omvatten:

* Apparaat
* Veldgateway,
* Gateways, in de cloud en
* Services.

Zones zijn breed aan segment van een oplossing. elke zone heeft vaak een eigen gegevens en verificatie en autorisatie-vereisten. Zones kunnen ook worden gebruikt voor isolatie schade en beperken de gevolgen van lage vertrouwensrelatie zones op hoger vertrouwen zones.

Elke zone worden gescheiden door een grens vertrouwen, die wordt vermeld als de rode stippellijn in het volgende diagram. Vertegenwoordigt een overgang van de gegevens/van een bron naar een andere. Tijdens deze overgang worden de gegevens mogelijk onderhevig aan Spoofing, Tampering, Repudiation, Information Disclosure, denial of Service en kan leiden tot misbruik van bevoegdheden (STRIDE).

![Zones voor IoT-beveiliging](media/iot-security-architecture/iot-security-architecture-fig1.png) 

De onderdelen binnen elke grens die ook worden onderworpen aan STRIDE, waardoor een volledige 360 threat modeling weergave van de oplossing. De volgende secties lichten op elk van de onderdelen en specifieke beveiligingsproblemen en oplossingen die moeten worden geplaatst in plaats.

De volgende secties worden besproken standaardonderdelen gewoonlijk in deze zones.

### <a name="the-device-zone"></a>De apparaat-zone

De apparaat-omgeving is de onmiddellijke fysieke ruimte rond het apparaat, waar fysieke toegang en/of 'lokale netwerk' digitale peer-to-peer-toegang tot het apparaat haalbaar is. Een 'lokale netwerk' wordt ervan uitgegaan dat een netwerk dat is uniek en geïsoleerd – maar een beperkt bereik draadloze radio-technologie waarmee peer-to-peer-communicatie van apparaten mogelijk naar het openbare Internet overbrugd. Dit gebeurt *niet* bevatten netwerk-virtualisatietechnologie die het maken van de illusie van een lokaal netwerk en het bevat ook geen openbare operator netwerken waarvoor twee apparaten om te communiceren via een openbaar netwerk ruimte als ze zijn om in te voeren van de relatie van een peer-to-peer-communicatie.

### <a name="the-field-gateway-zone"></a>De zone van de gateway veld

Veldgateway is een apparaat/apparaat of bepaalde computersoftware voor algemeen gebruik-server die als factor voor communicatie en mogelijk, als een apparaat besturingssysteem en een apparaat gegevensverwerking hub fungeert. De zone van de gateway veld bevat het veldgateway zelf en alle apparaten die zijn gekoppeld aan deze. Als de naam al aangeeft, veldgateways buiten toegewezen gegevensverwerking faciliteiten fungeren, zijn meestal afhankelijk van locatie, zijn mogelijk onderworpen aan fysieke indringing en beperkte operationele redundantie. Alle als u wilt bijvoorbeeld een veldgateway is meestal een ding kan een touch en sabotage en wat de functie is.

Een veldgateway wijkt af van een router louter verkeer in dat het een actieve rol bij het beheren van toegang heeft gehad en informatiestroom, wat betekent dat het een toepassing entiteit en de netwerkverbinding of terminal-sessie opgelost. Een NAT-apparaat of een firewall, daarentegen, niet in aanmerking komt als veldgateways omdat ze geen expliciete verbinding of sessie terminals, maar in plaats van een route (of blok)-verbindingen of via deze sessies. De veldgateway heeft twee verschillende aspecten van de surface. Een gezichten van de apparaten die zijn gekoppeld aan deze en binnen de zone vertegenwoordigt, en de andere gezichten van alle externe partijen en de rand van de zone is.

### <a name="the-cloud-gateway-zone"></a>De zone van de gateway cloud

Cloudgateway is een systeem dat externe communicatie van en naar apparaten of veldgateways van verschillende sites in openbaar netwerk ruimte, meestal op een besturingselement cloud-gebaseerde en analysesysteem voor gegevens, een federatieve van dergelijke systemen mogelijk maakt. In sommige gevallen kan kan een cloudgateway onmiddellijk faciliteren de toegang voor apparaten voor speciale doeleinden van terminals zoals tablets of telefoons. In de context die hier worden besproken, is de 'cloud' bedoeld om te verwijzen naar een toegewezen gegevensverwerking-systeem die niet is gekoppeld aan dezelfde site als de gekoppelde apparaten of veldgateways. Ook operationele metingen in een Zone Cloud kennis te voorkomen dat bepaalde fysieke toegang en zijn niet noodzakelijkerwijs beschikbaar in de infrastructuur van een 'openbare cloud'.  

Een cloudgateway kan mogelijk worden toegewezen aan een netwerkvirtualisatie-overlay naar de cloudgateway en alle bijbehorende gekoppelde apparaten of veldgateways van ander netwerkverkeer wordt. De cloudgateway zelf is geen apparaat een besturingssysteem of verwerking of opslagfaciliteit voor gegevens van het apparaat; een interface met de cloudgateway die faciliteiten. De zone van de gateway cloud omvat de cloudgateway zelf samen met alle veldgateways en apparaten direct of indirect gekoppeld. De rand van de zone is een afzonderlijke gebied waar alle externe partijen via communiceren.

### <a name="the-services-zone"></a>De zone services

Een 'service' is gedefinieerd voor deze context als een softwareonderdeel of de module die is communicatie met apparaten via een veld of cloud-gateway voor het verzamelen van gegevens en analyse, evenals voor opdracht en controle. Services zijn bemiddelaar. Ze fungeren onder hun identiteit op gateways en andere subsystemen, opslaan en analyseren van gegevens, autonoom opdrachten te verlenen aan apparaten op basis van inzichten in gegevens of schema's en gegevens en mogelijkheden voor gemachtigde gebruikers beheren.

### <a name="information-devices-versus-special-purpose-devices"></a>Informatie-apparaten en apparaten voor speciale doeleinden

Pc's, telefoons en tablets zijn voornamelijk interactieve informatie-apparaten. Telefoons en tablets zijn expliciet geoptimaliseerd om het maximaliseren van de levensduur van de accu. Ze bij voorkeur uitschakelen gedeeltelijk wanneer niet direct interactie met een persoon, of wanneer het niet leveren van services zoals het afspelen van muziek of het begeleiden van de eigenaar naar een bepaalde locatie. Deze informatie technologie-apparaten zijn vanuit het oogpunt van systemen hoofdzakelijk fungeert als proxy's voor mensen. Ze zijn "mensen actuators" voorstellen acties en 'mensen sensoren' verzamelen van invoer.

Apparaten voor speciale doeleinden, zijn van eenvoudige Temperatuursensoren tot complexe fabrieksproductielijnen met duizenden onderdelen binnen deze verschillend. Deze apparaten nog veel meer in gebruik zijn gericht en zelfs als sommige gebruikersinterface die ze en leveren ze zijn voornamelijk gericht op communicatie met of worden geïntegreerd in activa in de fysieke wereld. Ze meten en rapporteren uitwerking omstandigheden, kleppen inschakelen servos beheren, geluids-waarschuwingen, verlichting overschakelen en veel andere taken uitvoeren. Ze helpen om te werken die een apparaat informatie te algemeen, te duur, te groot of te Fragiel blijft is. Het concreet doel bepaalt het technische ontwerp onmiddellijk als ook de beschikbare monetaire budget voor hun productie en de levensduur van de geplande bewerking. De combinatie van deze twee belangrijke factoren Hiermee beperkt u de beschikbare operationele energie budget, fysieke footprint en dus beschikbare opslag, compute- en beveiligingsmogelijkheden.

Als er iets "uitvalt verkeerde' met geautomatiseerde of extern te beheren zijn apparaten, bijvoorbeeld defecten fysieke defecten of besturingselement logische willful onbevoegde toegang en manipuleren. De productie-partijen kunnen worden vernietigd, gebouwen mogelijk looted of gebrand omlaag en personen gewond raken of zelfs die kunnen worden. Dit is een geheel andere klasse van de schade dan iemand bezetten een gestolen creditcard limiet. Het beveiligingsniveau voor apparaten die zaken snel te verplaatsen, en ook voor de sensorgegevens die uiteindelijk in opdrachten die ertoe leiden dingen resulteert dat te verplaatsen, moet hoger zijn dan in een e-commerce of een scenario voor bankieren.

### <a name="device-control-and-device-data-interactions"></a>Apparaatbeheer en interacties van apparaat-gegevens

Verbonden apparaten voor speciale doeleinden hebben een groot aantal potentiële interactie surface gebieden en interactie patronen, die allemaal van belang om een framework voor het beveiligen van digitale toegang tot deze apparaten. De term 'digitale toegang' wordt hier gebruikt om te onderscheiden van bewerkingen die worden uitgevoerd via directe apparaat interactie waarbij beveiliging wordt geboden via fysieke toegangsbeheer. Bijvoorbeeld, als het apparaat in een kamer met een vergrendeling op de deur van de. Hoewel fysieke toegang kan niet worden geweigerd met behulp van software en hardware, kunnen maatregelen worden getroffen om te voorkomen dat fysieke toegang ertoe leidt dat voor systeem storing.

Als u de interactie patronen ontdekken, bekijken "apparaat controle" en "apparaat" met hetzelfde niveau van aandacht tijdens risicomodel. 'Apparaatbeheer ' kan worden geclassificeerd als alle gegevens die met het doel van wijzigen of het gedrag van het naar de status of de status van de omgeving invloed op een apparaat wordt geleverd door een partij. 'Apparaatgegevens' kunnen worden geclassificeerd als alle informatie op die een apparaat naar een andere partij over de status en de waargenomen status van de omgeving verzendt.

## <a name="performing-threat-modeling-for-the-azure-iot-reference-architecture"></a>Uitvoeren van threat modeling voor de Azure IoT-referentiearchitectuur

Microsoft maakt gebruik van het framework eerder beschreven hiervoor threat modeling voor Azure IoT. De volgende sectie wordt het concrete voorbeeld van Azure IoT Reference Architecture om te demonstreren hoe om na te denken over threat modeling voor IoT en hoe de bedreigingen die verhelpen. In dit voorbeeld bevat vier hoofdgebieden van focus:

* Apparaten en gegevensbronnen
* Gegevenstransport,
* Apparaat- en verwerking van gebeurtenissen, en
* Presentatie

![Threat Modeling voor Azure IoT](media/iot-security-architecture/iot-security-architecture-fig2.png)

Het volgende diagram biedt een vereenvoudigde weergave van IoT-architectuur van Microsoft met behulp van een gegevensstroom-Diagram-model dat wordt gebruikt door Microsoft Threat Modeling Tool:

![Threat Modeling voor Azure IoT via MS Threat Modeling Tool](media/iot-security-architecture/iot-security-architecture-fig3.png)

Het is belangrijk te weten dat de architectuur van de mogelijkheden van het apparaat en de gateway scheidt. Deze aanpak kunt u gebruikmaken van gatewayapparaten die beter te beveiligen zijn: ze zijn geschikt voor communicatie met de cloudgateway maakt gebruik van beveiligde protocollen, waarvoor meestal groter verwerkingsoverhead dat kan een eigen apparaat - zoals een thermostaat - Geef op de eigen. In de zone van de Azure-services, wordt ervan uitgegaan dat de Cloudgateway wordt vertegenwoordigd door de service Azure IoT Hub.

### <a name="device-and-data-sourcesdata-transport"></a>Apparaten en gegevens bronnen/gegevenstransport

Deze sectie gaat in op de architectuur die worden beschreven eerder het gaat om een risicomodel en een overzicht van het aanpakken van enkele van de inherente betrekking heeft op. In dit voorbeeld is gericht op de belangrijkste elementen van een risicomodel:

* Processen (zowel onder het beheer en het externe items)
* Communicatie (ook wel gegevensstromen)
* Opslag (ook wel gegevensarchieven)

#### <a name="processes"></a>Processen

In elk van de categorieën die worden beschreven in de Azure IoT-architectuur is in dit voorbeeld probeert om een aantal verschillende bedreigingen in de verschillende fasen bestaat gegevens/informatie in: proces, communicatie en opslag. Hieronder volgt een overzicht van de meest voorkomende voor de categorie 'proces', gevolgd door een overzicht van hoe deze bedreigingen beste kan mogelijk worden verholpen:

**Adresvervalsing (spoofing) (S)**: Een aanvaller kan cryptografische sleutelmateriaal extraheren uit een apparaat, op de software of hardwareniveau, en vervolgens toegang tot die het systeem met een ander fysiek of virtueel apparaat onder de identiteit van het apparaat het sleutelmateriaal is genomen van. Een goede illustratie is beheer op afstand die elke TV kunt inschakelen en die populaire prankster hulpprogramma's zijn.

**Denial of Service (D)**: Een apparaat kan worden gerenderd waarvoor niet werkt of de communicatie door te keuzerondje frequenties of knippen draden verstoren. Gegevens, kan bijvoorbeeld helemaal niet een camera toezicht op waarop de stroom of de netwerk-verbinding opzettelijk toenemende rapporteren.

**(T) knoeien**: Een aanvaller kan geheel of gedeeltelijk vervangen door de software die wordt uitgevoerd op het apparaat, waardoor de vervangen software gebruikmaken van de legitieme identiteit van het apparaat als het sleutelmateriaal of de cryptografische sleutel materiaal houden-installaties zijn beschikbaar voor de illegaal programma. Een aanvaller kan bijvoorbeeld gebruikmaken van uitgepakte sleutelmateriaal onderscheppen en gegevens van het apparaat op het communicatiepad onderdrukken en vervang deze door de waarde false gegevens die met de gestolen sleutelmateriaal is geverifieerd.

**Openbaarmaking van informatie (I)**: Als het apparaat gezelschapsdieren software wordt uitgevoerd, kan deze gezelschapsdieren software gegevens naar niet-geautoriseerde partijen mogelijk lekken. Een aanvaller kan bijvoorbeeld gebruikmaken van geëxtraheerde sleutelmateriaal zelf invoeren in het communicatiepad tussen het apparaat en een veld of controller gateway of cloudgateway naar siphon uit informatie.

**Misbruik van bevoegdheden (E)**: Een apparaat dat specifieke functie kan worden gedwongen om iets anders te doen. Bijvoorbeeld, kunt een klep die is geprogrammeerd om te openen halverwege worden misleiden helemaal openen.

| **Onderdeel** | **Threat** | **Risicobeperking** | **Risk** | **Implementatie** |
| --- | --- | --- | --- | --- |
| Apparaat |S |Identiteit toewijzen aan het apparaat en het apparaat te verifiëren |Apparaat of een deel van het apparaat vervangen door een ander apparaat. Hoe weet u dat het nu op het juiste apparaat? |Het apparaat, met behulp van Transport Layer Security (TLS) of IPSec-verificatie. Infrastructuur moet ondersteunen met behulp van vooraf gedeelde sleutel (PSK) op apparaten die volledige asymmetrische cryptografische kunnen niet worden verwerkt. Gebruikmaken van Azure AD [OAuth](https://www.rfc-editor.org/pdfrfc/rfc6755.txt.pdf) |
|| TRID |Toepassen tamperproof mechanismen waarmee u kunt het apparaat, bijvoorbeeld door waardoor het moeilijk is zelfs onmogelijk om op te halen van sleutels en andere cryptografische materiaal van het apparaat. |Het risico is als iemand het apparaat (fysieke interferentie) is geknoeid. Hoe weet u zeker dat, dat het apparaat is niet geknoeid met. |De meest effectieve oplossing is een vertrouwd platform module (TPM)-functie waarmee het opslaan van sleutels in speciale op-chip circuits van waaruit de sleutels kunnen niet worden gelezen, maar kunnen alleen worden gebruikt voor cryptografische bewerkingen die de sleutel gebruiken maar nooit vrijgeven van de sleutel. Geheugen-versleuteling van het apparaat. Sleutelbeheer voor het apparaat. Ondertekening van de code. |
|| E |Toegangsbeheer van het apparaat hebben. Het autorisatieschema voor. |Als het apparaat kunt u afzonderlijke acties worden uitgevoerd op basis van de opdrachten van een externe bron, of zelfs waarmee is geknoeid sensoren, kan de aanval voor het uitvoeren van bewerkingen niet toegankelijk is. |Autorisatieschema voor het apparaat hebben |
| Veldgateway |S |Verificatie van de gateway van het veld naar de Cloud-Gateway (zoals certificaat, PSK, of Claim.) |Als iemand Veldgateway vervalsen kan, klikt u vervolgens deze kan worden weergegeven als een apparaat. |TLS RSA/PSK, IPSec, [RFC 4279](https://tools.ietf.org/html/rfc4279). Dezelfde sleutel problemen voor de opslag en attestation van apparaten in het algemeen – aanbevolen case is TPM gebruiken. 6LowPAN-extensie voor IPSec-ter ondersteuning van draadloze Sensor netwerken (WSN). |
|| TRID |Beveiligen van de Veldgateway tegen knoeien (TPM)? |Spoofing aanvallen die verleiden om de cloud gateway denken dat deze met een veldgateway communiceert kan leiden tot vrijgeven van informatie en geknoei met gegevens |Geheugen versleuteling, TPM van, verificatie. |
|| E |Mechanisme voor toegangsbeheer voor Veldgateway | | |

Hier volgen enkele voorbeelden van bedreigingen in deze categorie:

**Adresvervalsing (spoofing)**: Een aanvaller kan cryptografische sleutelmateriaal extraheren uit een apparaat, op de software of hardwareniveau, en vervolgens toegang tot die het systeem met een ander fysiek of virtueel apparaat onder de identiteit van het apparaat het sleutelmateriaal is genomen van.

**Denial of Service**: Een apparaat kan worden gerenderd waarvoor niet werkt of de communicatie door te keuzerondje frequenties of knippen draden verstoren. Gegevens, kan bijvoorbeeld helemaal niet een camera toezicht op waarop de stroom of de netwerk-verbinding opzettelijk toenemende rapporteren.

**Knoeien**: Een aanvaller kan geheel of gedeeltelijk vervangen door de software die wordt uitgevoerd op het apparaat, waardoor de vervangen software gebruikmaken van de legitieme identiteit van het apparaat als het sleutelmateriaal of de cryptografische sleutel materiaal houden-installaties zijn beschikbaar voor de illegaal programma.

**Knoeien**: Een camera toezicht dat wordt weergegeven een afbeelding zichtbaar spectrum van een lege gang kan worden gericht op een foto van een dergelijke hotellobby. Een sensor rook of brand kan iemand met een lichtere daaronder reporting. In beide gevallen moet het apparaat mogelijk technisch volledig vertrouwen op het systeem, maar deze rapporteert gezelschapsdieren informatie.

**Knoeien**: Een aanvaller kan gebruikmaken van de uitgepakte sleutelmateriaal onderscheppen en gegevens van het apparaat op het communicatiepad onderdrukken en vervang deze door de waarde false gegevens die met de gestolen sleutelmateriaal is geverifieerd.

**Knoeien**: Een aanvaller kan geheel of gedeeltelijk vervangen door de software die wordt uitgevoerd op het apparaat, waardoor de vervangen software gebruikmaken van de legitieme identiteit van het apparaat als het sleutelmateriaal of de cryptografische sleutel materiaal houden-installaties beschikbaar zijn aan het programma illegale.

**Openbaarmaking van informatie**: Als het apparaat gezelschapsdieren software wordt uitgevoerd, kan deze gezelschapsdieren software gegevens naar niet-geautoriseerde partijen mogelijk lekken.

**Openbaarmaking van informatie**: Een aanvaller kan gebruikmaken van de uitgepakte sleutelmateriaal zelf invoeren in het communicatiepad tussen het apparaat en een veld of controller gateway of cloudgateway naar siphon uit informatie.

**Denial of Service**: Het apparaat kan worden uitgeschakeld of omgezet in een modus waarbij communicatie is niet mogelijk (dit is opzettelijk in veel industriële machines).

**Knoeien**: Het apparaat opnieuw kan worden geconfigureerd om te werken in een status onbekend op het systeem (buiten bekende kalibreren parameters) en dus zorgen dat gegevens die kan worden geïnterpreteerd

**Misbruik van bevoegdheden**: Een apparaat dat specifieke functie kan worden gedwongen om iets anders te doen. Bijvoorbeeld, kunt een klep die is geprogrammeerd om te openen halverwege worden misleiden helemaal openen.

**Denial of Service**: Het apparaat kan worden omgezet in een status waar communicatie niet mogelijk is.

**Knoeien**: Het apparaat kan opnieuw worden geconfigureerd om te werken in een status onbekend op het systeem (buiten bekende kalibreren parameters) en dus zorgen dat gegevens die kan worden geïnterpreteerd.

**Spoofing/Tampering/Repudiation**: Als niet-beveiligd (dit is slechts zelden het geval met beheer op afstand consumenten), een aanvaller de status van een apparaat anoniem kunt bewerken. Een goede illustratie is beheer op afstand die elke TV kunt inschakelen en die populaire prankster hulpprogramma's zijn.

#### <a name="communication"></a>Communicatie

Bedreigingen rond het communicatiepad tussen apparaten, apparaten en veldgateways en apparaat en cloud-gateway. De volgende tabel bevat enkele richtlijnen om open-sockets op het apparaat/VPN:

| **Onderdeel** | **Threat** | **Risicobeperking** | **Risk** | **Implementatie** |
| --- | --- | --- | --- | --- |
| Apparaat IoT-Hub |TID |(D) TLS (PSK/RSA) om het verkeer te versleutelen |Niet kan worden afgeluisterd of onderbreekt de communicatie tussen het apparaat en de gateway |Beveiliging op het protocolniveau. Met aangepaste protocollen moet u bepalen hoe ze beveiligen. In de meeste gevallen duurt de communicatie plaats van het apparaat naar de IoT Hub (de verbinding initieert apparaat). |
| Apparaten met apparaat |TID |(D) TLS (PSK/RSA) om het verkeer te versleutelen. |Lezen van gegevens in transit tussen apparaten. Knoeien met de gegevens. Overbelasting van het apparaat met nieuwe verbindingen |Beveiliging op het protocolniveau van het (MQTT/AMQP/HTTP-/ CoAP. Met aangepaste protocollen moet u bepalen hoe ze beveiligen. De oplossing voor de DoS-bedreiging is het peer-apparaten via een gateway van het veld of cloud en hebben ze alleen act als clients op het netwerk. De peering kan leiden tot een rechtstreekse verbinding tussen de peers na dat door de gateway zijn brokered |
| Externe entiteit apparaat |TID |Sterke koppelen van de externe entiteit op het apparaat |De verbinding met het apparaat niet kan worden afgeluisterd. Onderbreekt de communicatie met het apparaat |De externe entiteit op het apparaat NFC/Bluetooth LE veilig te koppelen. Het operationele deelvenster van het apparaat (fysiek) beheren |
| Field Gateway Cloud Gateway |TID |TLS (PSK/RSA) om het verkeer te versleutelen. |Niet kan worden afgeluisterd of onderbreekt de communicatie tussen het apparaat en de gateway |Beveiliging op het protocolniveau van het (MQTT/AMQP/HTTP-/ CoAP). Met aangepaste protocollen moet u bepalen hoe ze beveiligen. |
| De Cloudgateway apparaat |TID |TLS (PSK/RSA) om het verkeer te versleutelen. |Niet kan worden afgeluisterd of onderbreekt de communicatie tussen het apparaat en de gateway |Beveiliging op het protocolniveau van het (MQTT/AMQP/HTTP-/ CoAP). Met aangepaste protocollen moet u bepalen hoe ze beveiligen. |

Hier volgen enkele voorbeelden van bedreigingen in deze categorie:

**Denial of Service**: Beperkte-apparaten worden in het algemeen DoS threat wanneer ze actief naar binnenkomende verbindingen of ongevraagde datagrammen in een netwerk, luisteren omdat een aanvaller kan veel verbindingen open parallel en die niet worden deze of ze langzaam service of het apparaat kan worden overspoeld met ongevraagd verkeer. In beide gevallen wordt kan het apparaat effectief worden weergegeven in het netwerk niet meer werkt.

**Vervalsing, openbaarmaking van informatie**: Beperkte apparaten en apparaten voor speciale doeleinden hebben vaak een voor alle zekerheid faciliteiten, zoals wachtwoord of PINCODE beveiliging, of dat ze geheel afhankelijk zijn van het vertrouwen van het netwerk, wat betekent dat ze toegang verlenen tot gegevens wanneer een apparaat zich op hetzelfde netwerk, en dat het netwerk is vaak alleen beveiligd met een gedeelde sleutel. Dit betekent dat wanneer het gedeelde geheim op het apparaat of het netwerk wordt vermeld, is het mogelijk voor het beheren van het apparaat of gegevens die afkomstig zijn van het apparaat te observeren.  

**Adresvervalsing (spoofing)**: een aanvaller kan worden onderschept gedeeltelijk overschrijven de uitzending en vervalsen de oorspronkelijke aanvrager (man in het midden)

**Knoeien**: een aanvaller kan onderscheppen gedeeltelijk overschrijven de uitzending en onjuiste gegevens verzenden 

**Vrijgeven van informatie:** een aanvaller kan op een broadcast afluisteren en informatie zonder toestemming te verkrijgen **denial of Service:** een aanvaller kan het signaal jam en informatie distributie weigeren

#### <a name="storage"></a>Opslag

Elke gateway-apparaat en elk veld heeft een vorm van opslag (tijdelijke voor de gegevens, het besturingssysteem (OS) afbeeldingopslag queuing).

| **Onderdeel** | **Threat** | **Risicobeperking** | **Risk** | **Implementatie** |
| --- | --- | --- | --- | --- |
| Apparaatopslag |TRID |Versleuteling van opslag, ondertekening van de logboeken |Lezen van gegevens uit de opslag (PII-gegevens), te wijzigen met de telemetrische gegevens. Gemanipuleerde in de wachtrij geplaatst of in de cache opgeslagen opdracht control-gegevens. Knoeien met de configuratie of de firmware-update-pakketten kan in de cache opgeslagen of lokaal in de wachtrij leiden tot OS en/of systeem onderdelen worden aangetast |Versleuteling, message authentication code (MAC) of digitale handtekening. Waar mogelijk, sterk toegangsbeheer via toegang tot bronnen beheren (ACL's) of machtigingen. |
| Installatiekopie van besturingssysteem van het apparaat |TRID | |Gemanipuleerde OS / vervangen van de OS-componenten |OS-partitie, alleen-lezen, ondertekend installatiekopie van het besturingssysteem, versleuteling |
| Veldgateway opslag (queuing de gegevens) |TRID |Versleuteling van opslag, ondertekening van de logboeken |Lezen van gegevens uit de opslag (PII-gegevens), manipulatie van telemetriegegevens, manipulatie in de wachtrij geplaatst of in de cache opgeslagen opdracht control-gegevens. Knoeien met de configuratie of de firmware-update-pakketten (dat is bestemd voor apparaten of veldgateway) kan terwijl in de cache opgeslagen of lokaal in de wachtrij leiden tot OS en/of systeem onderdelen worden aangetast |BitLocker |
| Installatiekopie van het veld Gateway besturingssysteem |TRID | |Gemanipuleerde OS / vervangen van de OS-componenten |OS-partitie, alleen-lezen, ondertekend installatiekopie van het besturingssysteem, versleuteling |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Apparaat en gebeurtenissen verwerken/cloud gateway zone

Een cloudgateway is een systeem dat externe communicatie van en naar apparaten of veldgateways van verschillende sites in openbaar netwerk ruimte, meestal op een besturingselement cloud-gebaseerde en analysesysteem voor gegevens, een federatieve van dergelijke systemen mogelijk maakt. In sommige gevallen kan kan een cloudgateway onmiddellijk faciliteren de toegang voor apparaten voor speciale doeleinden van terminals zoals tablets of telefoons. In de context besproken hier 'cloud' is bedoeld om te verwijzen naar een toegewezen gegevensverwerking-systeem die niet is gekoppeld aan dezelfde site als de gekoppelde apparaten of veldgateways, en waar operationele maatregelen te voorkomen dat bepaalde fysieke toegang, maar is niet noodzakelijkerwijs tot een " infrastructuur voor openbare cloud'. Een cloudgateway kan mogelijk worden toegewezen aan een netwerkvirtualisatie-overlay naar de cloudgateway en alle bijbehorende gekoppelde apparaten of veldgateways van ander netwerkverkeer wordt. De cloudgateway zelf is geen apparaat een besturingssysteem of verwerking of opslagfaciliteit voor gegevens van het apparaat; een interface met de cloudgateway die faciliteiten. De zone van de gateway cloud omvat de cloudgateway zelf samen met alle veldgateways en apparaten direct of indirect gekoppeld.

Cloudgateway is voornamelijk aangepaste ingebouwde stukje software die wordt uitgevoerd als een service met beschikbaar gestelde eindpunten waarmee veldgateway en apparaten verbinding wilt maken. Het moet als zodanig zijn ontworpen met beveiliging in gedachten. Ga als volgt [SDL](https://www.microsoft.com/sdl) proces voor het ontwerpen en bouwen van deze service.

#### <a name="services-zone"></a>Services zone

Een besturingssysteem (of een domeincontroller) is een oplossing voor software die is gekoppeld aan een apparaat, of een veldgateway of cloudgateway voor het beheren van een of meerdere apparaten en/of voor het verzamelen en/of opslaan en/of analyseren van gegevens van het apparaat voor presentatie, of volgende controledoeleinden. Systemen voor toegangsbeheer zijn de worden alleen entiteiten in het bereik van deze discussie die interactie met mensen onmiddellijk vergemakkelijken. De uitzonderingen zijn tussenliggende fysieke besturingselement oppervlakken op apparaten, zoals een switch waarmee een gebruiker het apparaat uitschakelen of andere eigenschappen wijzigen, en waarvoor er geen functioneel equivalent die digitaal kan worden geopend is.

Tussenliggende fysieke besturingselement oppervlakken zijn wanneer de functie van de fysieke beheren van surface van bestuur logische beperkt dat een gelijkwaardige-functie op afstand kan worden gestart of invoer veroorzaakt een conflict met externe invoer kunnen worden vermeden – zoals intermediated besturingselement oppervlakken zijn conceptueel gezien op een lokale besturingssysteem die gebruikmaakt van dezelfde onderliggende functionaliteit als elk ander systeem beheer op afstand die het apparaat mogelijk gelijktijdig worden gekoppeld aan gekoppeld. Belangrijkste bedreigingen voor de cloud computing kan worden gelezen op [Cloud Security Alliance (CSA)](https://cloudsecurityalliance.org/articles/csa-releases-top-threats-to-cloud-computing-deep-dive/) pagina.

## <a name="additional-resources"></a>Aanvullende resources

Raadpleeg voor meer informatie de volgende artikelen:

* [SDL Threat Modeling Tool](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
* [Microsoft Azure IoT-referentiearchitectuur](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)
