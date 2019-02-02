---
title: Concepten van Azure IoT Hub x.509-beveiliging | Microsoft Docs
description: Concept - inzicht in de waarde X.509-certificaat certificaten van certificeringsinstanties in IoT-apparaat-productie en verificatie.
author: eustacea
manager: arjmands
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/18/2017
ms.author: eustacea
ms.openlocfilehash: e3d6464f3e69868b4903d999e52a37b0520018c6
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55659453"
---
# <a name="conceptual-understanding-of-x509-ca-certificates-in-the-iot-industry"></a>Conceptuele kennis van x.509-CA-certificaten in de branche IoT

Dit artikel beschrijft de waarde van het gebruik van x.509-certificaten van certificeringsinstanties (CA) in IoT-apparaat-productie en verificatie met IoT Hub.  Het bevat informatie over het aanbod keten van setup en markeer voordelen.

Dit artikel wordt beschreven:

* Wat CA X.509-certificaten zijn en hoe u ze
* Informatie over het registreren van uw X.509-CA-certificaat naar IoT Hub
* Over het instellen van een productiebedrijf toeleveringsketen voor verificatie op basis van X.509-CA
* Hoe apparaten die zijn ondertekend met een X.509-CA verbinding maken met IoT Hub

## <a name="overview"></a>Overzicht

X.509-certificaat (Certificeringsinstantie) verificatie is een benadering voor het verifiëren van apparaten bij IoT Hub met behulp van een methode die drastisch identiteit maken en de levenscyclus van Apparaatbeheer in de toeleveringsketen vereenvoudigt.

Een onderscheidende kenmerk van de X.509-CA-verificatie is een een-op-veel-relatie die een CA-certificaat met de downstream-apparaten heeft.  Deze relatie kunt registratie van een willekeurig aantal apparaten in IoT Hub door het registreren van een X.509-CA-certificaat eenmaal, anders unieke certificaten voor apparaten vooraf geregistreerde voor elk apparaat moeten zijn voordat een apparaat verbinding kan maken. Deze een-op-veel-relatie vereenvoudigt ook de bewerkingen voor certificaten levenscyclus van Apparaatbeheer.

Een ander belangrijk kenmerk van de X.509-CA-verificatie is supply chain logistiek te vereenvoudigen.  Veilige verificatie van apparaten is vereist dat elk apparaat een unieke geheim, zoals een sleutel als basis voor de vertrouwensrelatie bevat. In de verificatie op basis van certificaten is dit geheim een persoonlijke sleutel. Een typische apparaat productie-stroom omvat meerdere stappen en bewaarders.  Veilig is meerdere bewaarders apparaat persoonlijke sleutels te beheren en onderhouden van de vertrouwensrelatie moeilijk en kostbaar.  Met behulp van certificeringsinstanties lost dit probleem op door elke beheerder in een cryptografische vertrouwensketen ondertekening in plaats van waarbij ze met persoonlijke sleutels van apparaat.  Elke beheerder zich op zijn beurt apparaten bij hun respectieve processtap van de productiestroom.  Het resultaat is een optimale toeleveringsketen met ingebouwde aansprakelijkheid wordt versterkt door gebruik te maken van de cryptografische keten van vertrouwen.  Het is vermelden waard dat dit proces resulteert in de meeste beveiliging wanneer apparaten de unieke persoonlijke sleutels beveiligt.  Om dit te bereiken adviseren we het gebruik van Hardware Secure Modules (HSM) die geschikt is voor het genereren van intern persoonlijke sleutels die het licht van de dag wordt nooit te zien.

In dit artikel biedt een end-to-end-weergave van het gebruik van de X.509-CA-verificatie, vanuit supply chain setup apparaat verbinding, tijdens het maken van het gebruik van een voorbeeld van de echte wereld duidelijker begrip.

## <a name="introduction"></a>Inleiding

Het X.509-CA-certificaat is een digitaal certificaat waarvan houder van andere certificaten ondertekenen kunt.  Deze digitale certificaten X.509 is omdat het voldoet aan een certificaat opmaak van IETF RFC 5280 standaard voorgeschreven standard en een certificeringsinstantie (CA) is, omdat de houder van andere certificaten kunt ondertekenen.

Het gebruik van X.509-CA is het best ten opzichte van een voorbeeld van een concreet begrijpen.  Houd rekening met bedrijf X, maker van Smart-X-Widgets die is ontworpen voor installatie professional. Bedrijf X heeft zowel productie- en installatie.  Deze opdrachten Factory-Y voor de productie van de smartcard-X-Widgets en serviceprovider technicus Z voor het installeren van de fabrikant van. Bedrijf X wenst dat Smart-X-Widget rechtstreeks afkomstig is van Factory-Y technicus-z voor de installatie en dat deze maakt rechtstreeks verbinding met bedrijfs-exemplaar van IoT-Hub x na de installatie zonder verdere tussenkomst van de uit bedrijf X. Als u wilt dit gebeurt, moet bedrijf X enkele eenmalige installatiebewerkingen voor smartcard-X-Widget voor automatische verbinding moet worden voltooid.  De rest van dit artikel is met het end-to-end-scenario in gedachten, als volgt opgebouwd:

* De X.509-CA-certificaat verkrijgen

* X.509-CA-certificaat naar IoT Hub registreren

* Meld u apparaten in een certificaatketen van vertrouwensrelatie

* Apparaatverbinding

## <a name="acquire-the-x509-ca-certificate"></a>De X.509-CA-certificaat verkrijgen

Bedrijf X is de optie aanschaffen van een X.509-CA-certificaat van een openbare basiscertificeringsinstantie of een door een zelf-ondertekend proces te maken.  Een optie zijn optimale of andere afhankelijk van het toepassingsscenario.  Ongeacht de optie houdt de twee fundamentele stappen, een openbaar/persoonlijk sleutelpaar genereren en de openbare sleutel in een certificaat voor ondertekening.

![img-csr-flow](./media/csr-flow.png)

Meer informatie over het uitvoeren van deze stappen verschillen met verschillende serviceproviders.

### <a name="purchasing-an-x509-ca-certificate"></a>Een X.509-CA-certificaat kopen

Aanschaffen van een CA-certificaat heeft het voordeel van een bekende basis-CA-act dat als een vertrouwde derde partij garant staat voor de geldigheid van IoT-apparaten wanneer de apparaten verbinding maken. Bedrijf X kiest deze optie als ze van plan bent Smart-X-Widget om te communiceren met producten van derden of services na de eerste verbinding met IoT Hub.

Bedrijf X kiest voor een X.509-CA-certificaat hebt gekocht, een basis-certificaten services-provider. Goede leads resulteert in een internet zoeken naar de woordgroep basis-CA.  De basis-CA leidt bedrijf X voor het maken van het openbare/persoonlijke sleutelpaar en het genereren van een Certificate Signing Request (CSR) voor de services.  Een CSR is het formele proces van het toepassen van een certificaat van een certificeringsinstantie.  Het resultaat van deze aankoop is een certificaat voor gebruik als een certificaat.  Gezien de alomtegenwoordigheid van X.509-certificaten, is het certificaat waarschijnlijk goed aan de IETF RFC 5280 standard zijn opgemaakt.

### <a name="creating-a-self-signed-x509-ca-certificate"></a>Het maken van een zelfondertekend x.509-CA-certificaat

Het proces voor het maken van een zelfondertekend x.509-CA-certificaat is vergelijkbaar met het aanschaffen met uitzondering van met betrekking tot een ondertekenaar van derden, zoals de basis-CA. Bedrijf X ondertekent het certificaat in plaats van een basis-CA-certificaat in ons voorbeeld.  Bedrijf X kan deze optie voor het testen van totdat ze klaar om aan te schaffen van een certificaat. Bedrijf X kan een zelfondertekend X.509-CA-certificaat in productie, ook gebruiken als Smart-X-object is niet bedoeld voor het verbinding maken met services van derden buiten de IoT-Hub.

## <a name="register-the-x509-certificate-to-iot-hub"></a>Registreren van het X.509-certificaat naar IoT Hub

Bedrijf X moet voor het registreren van het X.509-CA naar IoT Hub waar fungeren Smart-X-Widgets verifiëren omdat ze verbinding maken.  Dit is een eenmalige proces dat het mogelijk om te verifiëren en een willekeurig aantal Smart-X-Widget apparaten te beheren.  Dit proces is eenmalige vanwege een een-op-veel-relatie tussen een certificaat en de apparaten en ook vormt een van de belangrijkste voordelen van het gebruik van de X.509-CA-verificatiemethode.  Het alternatief is voor het uploaden van afzonderlijke certificaatvingerafdrukken voor elk Smart-X-Widget apparaat waardoor toe te voegen aan operationele kosten.

Registratie van het X.509-CA-certificaat is een proces in twee stappen, het certificaat uploaden en het certificaat van bewijs van eigendom.

![img-pop-flow](./media/pop-flow.png)

### <a name="x509-ca-certificate-upload"></a>X.509-CA-certificaat uploaden

Het x.509-CA-certificaat uploaden dat is, het CA-certificaat uploaden naar IoT Hub.  IoT Hub wordt verwacht dat het certificaat in een bestand. Bedrijf X uploadt gewoon het certificaatbestand. Het certificaatbestand moet niet onder alle omstandigheden bevat geen persoonlijke sleutels.  Best practices van normen opgelegd Public Key Infrastructure (PKI) deze kennis van bedrijf mandaten-x particuliere in dit geval exclusief binnen het bedrijf X zich bevindt.

### <a name="proof-of-possession-of-the-certificate"></a>Bewijs van eigendom van het certificaat

Net als een digitaal certificaat, de X.509-CA-certificaat is openbare informatie die is deze vatbaar voor inbreuk.  Als zodanig kan meeluistert onderscheppen van een certificaat en probeert te uploaden als hun eigen.  In ons voorbeeld graag IoT Hub om ervoor te zorgen dat de CA-certificaat van bedrijf X uploaden echt deel uitmaakt van bedrijf X. Dit gebeurt, zodat door uitdagende bedrijf X om te controleren die ze in feite bezit zijn van het certificaat via een [bewijs van eigendom (PoP)-stroom](https://tools.ietf.org/html/rfc5280#section-3.1). De stroom bewijs van eigendom omvat IoT-Hub genereren van een willekeurig getal moet worden ondertekend door de bedrijfsportal-X met behulp van de persoonlijke sleutel.  Als bedrijf X gevolgd PKI aanbevolen procedures en de persoonlijke sleutel beveiligd vervolgens alleen ze zou bevindt zich op positie goed reageren op de challenge bewijs van eigendom.  IoT Hub wordt voortgezet voor het registreren van het x.509-CA-certificaat bij een geslaagde respons van de uitdaging bewijs van eigendom.

Een geslaagde reactie op de uitdaging bewijs van eigendom van IoT Hub is voltooid voor de registratie van de X.509-CA.

## <a name="sign-devices-into-a-certificate-chain-of-trust"></a>Meld u apparaten in een certificaatketen van vertrouwensrelatie

IoT moet elk apparaat in het bezit van een unieke identiteit.  Deze id's in de vorm certificaten voor verificatie op basis van certificaat schema's.  In ons voorbeeld, betekent dit dat elke smartcard-X-Widget moet beschikken over een unieke apparaat-certificaat.  Hoe werkt bedrijf X setup voor deze in de toeleveringsketen?

Een manier om te gaan hierover is voor het vooraf genereren van certificaten voor smartcard-X-Widgets en aanwijzing kennis van de bijbehorende persoonlijke sleutels van de unieke apparaat met supply chain partners.  Voor bedrijf X, betekent dit dat aanwijzing Factory-Y en technicus Z.  Hoewel dit een geldige methode, beschikt u over de uitdagingen die moeten worden verholpen om ervoor te zorgen vertrouwensrelatie als volgt:

1. Voor het delen van persoonlijke sleutels van apparaat met toeleverende partners, naast de PKI worden genegeerd met procedures aanbevolen van het delen van persoonlijke sleutels, maakt het bouwen van vertrouwen in de toeleveringsketen dure nooit.  Betekent dit dat kapitaal systemen zoals beveiligde ruimten naar huis apparaat persoonlijke sleutels en processen, zoals een periodieke beveiligingscontrole moeten worden geïnstalleerd.  Beide toevoegen kosten aan de toeleveringsketen.

2. Veilig accounting voor apparaten in de toeleveringsketen en beheren van deze later in de implementatie wordt een-op-een taak voor elk paar sleutel-naar-apparaat vanaf het moment van apparaat uniek certificaat (daarom persoonlijke sleutel) genereren voor het buiten gebruik stellen. Deze uitsluit groepsbeheer van apparaten, tenzij het concept van groepen is expliciet ingebouwd in het proces loopt de toepassing. Veilig accounting- en apparaat levenscyclus beheer, wordt daarom een werkbelasting met zware bewerkingen.  Bedrijf X zou deze last draagt in ons voorbeeld.

Verificatie via X.509-CA-certificaat biedt elegante oplossingen voor afore uitdagingen door het gebruik van certificaatketens vermeld.  Een certificaatketen resultaat is van een CA een tussenliggende CA die op zijn beurt ondertekent een andere tussenliggende CA en dus gezet totdat een definitieve tussenliggende CA zich aanmeldt voor een apparaat te ondertekenen.  In ons voorbeeld ondertekent bedrijf X Factory-Y, die zich op zijn beurt technicus-Z, die zich ten slotte Smart-X-Widget.

![img-cert-chain-hierarchy](./media/cert-chain-hierarchy.png)

Geeft de logische aflevering van instantie hierboven cascade van certificaten in de keten.  Veel toeleveringsketens, volgt u deze logische aflevering waarbij elke tussenliggende CA wordt ondertekend in de keten bij het ontvangen van alle upstream CA-certificaten en de laatste tussenliggende CA zich ten slotte elk apparaat en alle CA-certificaten van de keten invoeren bij het apparaat. Dit is gebruikelijk wanneer het contract productiebedrijf met een hiërarchie van de fabrieken commissies een bepaalde factory doen van de productie.  Tijdens de hiërarchie kan mogelijk op verschillende niveaus worden diep (bijvoorbeeld per geografische locatie/product type/productie regel), alleen de fabriek aan het einde opgehaald om te communiceren met het apparaat, maar de keten vanaf de bovenkant van de hiërarchie wordt onderhouden.

Alternatieve ketens mogelijk verschillende tussenliggende CA communiceren met het apparaat waarop injects geval de CA-interactie met het apparaat inhoud voor keten van certificaten op dat moment.  Hybride modellen zijn ook mogelijk waarbij slechts enkele van de CA fysiek interactie met het apparaat heeft.

In ons voorbeeld communiceren zowel Factory-Y als medewerker Z met de smartcard-X-Widget.  Terwijl het bedrijf X is eigenaar van de smartcard-X-Widget, communiceert het daadwerkelijk fysiek niet met deze in de hele toeleveringsketen.  Bedrijf X daarom deel uitmaken van de certificaatketen van de vertrouwensrelatie voor smartcard-X-Widget Factory-Y die zich op zijn beurt technicus-Z, dat vervolgens laatste handtekening in de Smart-X-Widget ondertekenen. De productie en de installatie van Smart-X-Widget omvatten Factory-Y en technicus-Z met behulp van hun respectieve tussenliggende CA-certificaten aan te melden elk Smart-X-Widgets. Het eindresultaat van dit hele proces is Smart-X-Widgets met unieke apparaat-certificaten en certificaatvertrouwensketen toe aan bedrijf X CA-certificaat.

![img-cert-mfr-chain](./media/cert-mfr-chain.png)

Dit is een goed punt om te controleren van de waarde van de X.509-CA-methode.  In plaats van het vooraf genereren en afleveren van certificaten voor elke smartcard-X-Widget in de toeleveringsketen, moesten bedrijf X alleen Factory-Y één keer aanmelden.  Bedrijf X kan nu in plaats van dat voor het bijhouden van elk apparaat in de gehele levenscyclus van het apparaat, bijhouden en apparaten beheren via de groepen die op een natuurlijke manier de toeleveringsprocessen bijvoorbeeld apparaten zijn geïnstalleerd door technicus Z na juli van een jaar geven.

Laatste, maar niet in het minst infuses de CA verificatiemethode beveiligde aansprakelijkheid bij het apparaat productie toeleveringsketen. De acties van elk lid in de keten is vanwege het proces van de certificaatketen cryptografisch opgenomen en te verifiëren.

Dit proces is afhankelijk van bepaalde veronderstellingen die voor de volledigheid moeten worden opgehaald.  Dit is vereist voor onafhankelijke maken van apparaat uniek openbaar/persoonlijk sleutelpaar en dat de persoonlijke sleutel in het apparaat worden beveiligd.  Gelukkig bestaan beveiligde silicon chips in de vorm van Hardware beveiligde Modules (HSM) kan intern genereren van sleutels en de bescherming van persoonlijke sleutels.  Bedrijf X moet alleen een van deze chips in stuklijst Smart-X-Widget onderdeel toevoegen.

## <a name="device-connection"></a>Apparaatverbinding

Voorgaande secties in bovenstaande hebt zijn waarbij wordt gebouwd op het apparaatverbinding.  Registreert gewoon een X.509-CA-certificaat naar IoT Hub een keer, hoe mogelijk miljoenen apparaten verbinden en ophalen van de eerste keer is geverifieerd?  Eenvoudige; via het hetzelfde certificaat uploaden en bewijs van eigendom stroom er eerder is met de registratie van het X.509-CA-certificaat.

Apparaten die zijn geproduceerd voor X.509-CA-verificatie zijn uitgerust met unieke certificaten voor apparaten en een certificaatketen van hun respectieve productie toeleveringsketen.  Apparaatverbinding, zelfs voor de eerste keer, gebeurt er in een proces in twee stappen: keten uploaden en bewijs van eigendom van het certificaat.

Tijdens het uploaden van de certificaatketen uploadt het apparaat het unieke apparaat-certificaat, samen met de certificaatketen die is geïnstalleerd in het IoT-hub.  Met behulp van het vooraf geregistreerde X.509-CA-certificaat, kunt IoT-Hub cryptografisch valideren een aantal dingen, dat de geüploade certificaatketen intern consistent is, en dat de keten is afkomstig van de geldige eigenaar van het x.509-CA-certificaat.  NET was met het registratieproces X.509-CA, IoT-Hub wilt initiëren een bewijs van eigendom vraag en antwoord-proces vast te stellen de keten en daarom apparaatcertificaat daadwerkelijk behoort tot het uploaden van het apparaat.  Dit gebeurt door het genereren van een willekeurige uitdaging om te worden ondertekend door het apparaat met behulp van de persoonlijke sleutel voor de validatie van IoT-Hub.   Een geslaagde respons wordt geactiveerd voor IoT Hub om te accepteren van het apparaat als authentiek en het verbinding geven.

In ons voorbeeld zou elke smartcard-X-Widget het unieke apparaat-certificaat, samen met Factory-Y en technicus Z X.509-CA-certificaten te uploaden en vervolgens te reageren op voor de controle bewijs van eigendom van IoT Hub.

![img-device-pop-flow](./media/device-pop-flow.png)

U ziet dat de basis van de vertrouwensrelatie berust bij het beschermen van persoonlijke sleutels, met inbegrip van persoonlijke sleutels van apparaat.  We daarom kan niet zorgen voldoende het belang van beveiligde silicon chips in de vorm van Hardware beveiligde Modules (HSM) voor het beveiligen van persoonlijke sleutels van apparaat, en de algemene aanbevolen procedures nooit delen van persoonlijke sleutels, zoals een factory aanwijzing van een ander met de persoonlijke sleutel.

