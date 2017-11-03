---
title: Concepten van Azure IoT Hub X.509 beveiliging | Microsoft Docs
description: Concept - inzicht in de waarde X.509-certificaat certificaten in de productie van IoT-apparaat en verificatie.
services: iot-hub
documentationcenter: .net
author: eustacea
manager: arjmands
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: eustacea
ms.openlocfilehash: 34d4be431b76d5ba8258d932cb21ed6f4818863e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="conceptual-understanding-of-x509-ca-certificates-in-the-iot-industry"></a>Algemene kennis van x.509-CA-certificaten in de branche IoT

In dit artikel beschrijft de waarde van het gebruik van X.509-certificaat (certificeringsinstantie)-certificaten in productie van IoT-apparaat en verificatie met IoT Hub.  Deze bevat informatie over levering keten van setup en markeer voordelen.

Dit artikel wordt beschreven:

* Wat x.509-CA-certificaten zijn en hoe ze
* Het registreren van uw x.509-CA-certificaat naar IoT Hub
* Het instellen van een productie toeleveringsketen voor verificatie op basis van een x.509-CA
* Hoe apparaten die zijn ondertekend met een x.509-CA verbinding maken met IoT Hub

## <a name="overview"></a>Overzicht

X.509-certificeringsinstantie (CA) verificatie is een methode voor het verifiëren van apparaten met IoT Hub met behulp van een methode die aanzienlijk eenvoudiger identiteit maken en de levenscyclus van beheer van apparaten in de keten.

Een onderscheidende kenmerk van het x.509-CA-verificatie is een een-op-veel-relatie die een CA-certificaat met de downstream-apparaten heeft.  Deze relatie kan registratie van een willekeurig aantal apparaten in IoT Hub door een CA X.509-certificaat eenmaal te registreren, anders unieke apparaatcertificaten vooraf geregistreerde voor elk apparaat moeten zijn voordat een apparaat verbinding kan maken. Deze een-op-veel-relatie vereenvoudigt ook apparaat certificaten levenscyclus beheertaken uit te voeren.

Een ander belangrijk kenmerk van het x.509-CA-verificatie is vereenvoudiging van levering keten logistiek.  Veilige verificatie van apparaten is vereist dat elk apparaat een unieke geheim, zoals een sleutel als basis voor de vertrouwensrelatie bevat. In de verificatie op basis van certificaten is dit geheim een persoonlijke sleutel. Een typische apparaat stroom productie omvat meerdere stappen en bewaarders.  Veilig is apparaat persoonlijke sleutels beheren in meerdere bewaarders en het onderhouden van vertrouwensrelatie moeilijk en kostbaar proces.  Met behulp van certificeringsinstanties verhelpt dit probleem door elke vallen in een cryptografische vertrouwensketen ondertekening in plaats van ze aanwijzing met persoonlijke sleutels van apparaat.  Elke vallen ondertekent op zijn beurt apparaten bij hun respectieve processtap van de productiestroom.  Het resultaat is een optimale toelevering met ingebouwde accountability door het gebruik van de cryptografische vertrouwensketen.  Hierbij moet worden opgemerkt dat dit proces resulteert in de meeste beveiliging wanneer apparaten hun unieke persoonlijke sleutels beveiligt.  Met dit end Wij raden ten zeerste het gebruik van Hardware Secure Modules (HSM) geschikt is voor intern genereren van persoonlijke sleutels die nooit het licht van de dag te zien.

In dit artikel biedt een end-to-end-weergave van het gebruik van de verificatie x.509-CA van levering keten instellingen tot het apparaatverbinding gebruiken tijdens het maken van een voorbeeld van een concrete understanding versterken.

## <a name="introduction"></a>Inleiding

Het x.509-CA-certificaat is een digitaal certificaat waarvan houder andere certificaten kan ondertekenen.  Deze digitale certificaat X.509 omdat het voldoet aan een certificaat dat door de IETF RFC 5280 standaard voorgeschreven standaard opmaak, en een certificeringsinstantie (CA) omdat de houder van andere certificaten ondertekenen kunt.

Het gebruik van x.509-CA best ten opzichte van een voorbeeld van een concreet begrijpen.  Overweeg het bedrijf-X, een maker van smartcard-X-Widgets ontworpen voor installatie professional. Bedrijf X heeft productie- en installatie.  Deze opdrachten voor Factory-Y voor de productie van de smartcard-X-Widgets en serviceprovider technicus Z voor het installeren van de fabrikant. Bedrijf X wenst dat smartcard-X-Widget rechtstreeks van de Factory-Y technicus z voor de installatie verzonden en dat deze rechtstreeks verbinding met bedrijf maakt-exemplaar van IoT Hub x na de installatie zonder verdere tussenkomst vanuit bedrijf X. Bedrijf X moet om dit te realiseren, om een paar eenmalige instellingsbewerkingen voor het bewerken van smartcard-X-Widget voor automatische verbinding te voltooien.  De rest van dit artikel is met het end-to-end-scenario in gedachten, als volgt opgebouwd:

* Het x.509-CA-certificaat verkrijgen

* X.509-CA-certificaat naar IoT Hub registreren

* Apparaten in een certificaatvertrouwensketen ondertekenen

* Apparaatverbinding

## <a name="acquire-the-x509-ca-certificate"></a>Het x.509-CA-certificaat verkrijgen

Bedrijf X heeft kiezen of aanschaffen van een x.509-CA-certificaat van een openbare basiscertificeringsinstantie of een tot en met een zelf-ondertekend proces maken.  Een optie om zou optimale via andere afhankelijk van het toepassingsscenario zijn.  Ongeacht de optie houdt de twee fundamentele stappen een openbaar/persoonlijk sleutelpaar genereren en de openbare sleutel in een certificaat voor ondertekening.

![IMG-csr-stroom](./media/csr-flow.png)

Meer informatie over het uitvoeren van deze stappen verschillen met verschillende serviceproviders.

### <a name="purchasing-an-x509-ca-certificate"></a>Het aanschaffen van een x.509-CA-certificaat

Een CA-certificaat aanschaffen, heeft het voordeel van een bekende basis-CA-act dat als een vertrouwde derde partij aan garant staat voor de geldigheid van IoT-apparaten wanneer de apparaten verbinding maken. Bedrijf X kiest deze optie als ze van plan bent smartcard X Widget om te communiceren met producten van derden of services na de eerste verbinding met IoT Hub.

Bedrijf X kiest om een x.509-CA-certificaat hebt gekocht, een toegangspunt certificaten serviceprovider. Goede leads resulteert in een zoekopdrachten op internet voor de zinsnede basis-CA.  De basis-CA leidt bedrijf X over het maken van het openbare/persoonlijke sleutelpaar en het genereren van een Certificate Signing Request (CSR) voor hun services.  Een certificaataanvraag is het formele proces van het toepassen van een certificaat van een certificeringsinstantie.  Het resultaat van deze aankoop is een certificaat voor gebruik als een certificeringsinstantie.  Gezien de verspreiding van X.509-certificaten, is het certificaat waarschijnlijk juist zijn opgemaakt aan de IETF RFC 5280 standaard.

### <a name="creating-a-self-signed-x509-ca-certificate"></a>Een zelfondertekend x.509-CA-certificaat maken

Het proces voor het maken van een zelfondertekend x.509-CA-certificaat is vergelijkbaar met het aanschaffen van een met uitzondering van een derde partij ondertekenaar zoals de hoofdcertificeringsinstantie met betrekking tot. Bedrijf X ondertekent het certificaat in plaats van een basiscertificeringsinstantie in ons voorbeeld.  Bedrijf X mogelijk deze optie voor het testen van totdat ze zijn een certificaat kopen. Bedrijf X kan een zelf-ondertekend x.509-CA-certificaat in productie, ook gebruiken als smartcard-X-object is niet bedoeld voor het verbinding maken met alle services van derden buiten de IoT-Hub.

## <a name="register-the-x509-certificate-to-iot-hub"></a>Registreren van het X.509-certificaat naar IoT Hub

Bedrijf X moet registreren van de CA x.509-IoT-hub waar fungeren voor de verificatie van smartcard-X-Widgets ze verbinding maken.  Dit is een eenmalige proces waarmee de mogelijkheid om te verifiëren en een willekeurig aantal X-smartcard-Widget apparaten te beheren.  Dit proces is eenmalige vanwege een een-op-veel-relatie tussen een certificaat en de apparaten en ook vormt een van de belangrijkste voordelen van het gebruik van de verificatiemethode x.509-CA.  Het alternatief is voor het uploaden van afzonderlijke certificaatvingerafdrukken voor elk X-smartcard-Widget apparaat waardoor toe te voegen aan de operationele kosten.

Registreren van het x.509-CA-certificaat is een proces, het certificaat uploaden en certificaat bewijs van bezit.

![IMG-pop-stroom](./media/pop-flow.png)

### <a name="x509-ca-certificate-upload"></a>X.509-CA-certificaat uploaden

Het x.509-CA-certificaat uploaden, is slechts, het CA-certificaat uploaden naar IoT Hub.  IoT Hub verwacht dat het certificaat in een bestand. Bedrijf X bestandsuploads gewoon het certificaatbestand. Het certificaatbestand moet niet onder alle omstandigheden bevat geen persoonlijke sleutels.  Best practices uit normen inzake Public Key Infrastructure (PKI) vereist deze kennis van bedrijf-x persoonlijke in dit geval uitsluitend binnen het bedrijf X zich bevindt.

### <a name="proof-of-possession-of-the-certificate"></a>Bewijs van bezit van het certificaat

Het X.509-CA-certificaat, net als een digitaal certificaat is openbare informatie die is vatbaar voor inbreuk.  Als zodanig kan meeluistert onderscheppen van een certificaat en probeert te uploaden als hun eigen.  In ons voorbeeld graag IoT Hub om ervoor te zorgen dat het CA-certificaat van bedrijf X uploaden daadwerkelijk van bedrijf X. Dit biedt dus door lastig bedrijf X naar aangetoond dat ze in feite bezit zijn van het certificaat via een [bewijs van bezit (PoP) stroom](https://tools.ietf.org/html/rfc5280#section-3.1). De stroom bewijs van bezit houdt IoT-Hub genereren van een willekeurig getal te worden ondertekend met bedrijf-X met behulp van de persoonlijke sleutel.  Als bedrijf X gevolgd PKI aanbevolen procedures en de persoonlijke sleutel beveiligd zou vervolgens alleen ze worden in positie correct reageren op de challenge bewijs van bezit.  IoT Hub verloopt in het x.509-CA-certificaat bij een geslaagde reactie van de uitdaging bewijs van bezit registreren.

Een geslaagde reactie op de vraag bewijs van bezit uit IoT Hub is voltooid voor de registratie van het x.509-CA.

## <a name="sign-devices-into-a-certificate-chain-of-trust"></a>Apparaten in een Certificaatvertrouwensketen ondertekenen

IoT moet elk apparaat dat een unieke identiteit.  Deze identiteiten zijn in de vorm certificaten voor verificatiemethoden op basis van certificaten.  Dit betekent elke Widget voor het X van een smartcard, moet ze beschikken over een unieke apparaat-certificaat in het voorbeeld.  Hoe bedrijf X instellen voor dit in de toeleveringsketen?

Een manier om te gaan over deze is voor het vooraf genereren van certificaten voor smartcards-X-Widgets en aanwijzing kennis van de bijbehorende persoonlijke sleutels van de unieke apparaat met toeleverende partners.  Voor bedrijf-X, betekent dit dat aanwijzing Factory Y en technicus Z.  Hoewel dit een geldige methode, beschikt u over uitdagingen die moeten worden ondervangen om ervoor te zorgen vertrouwensrelatie als volgt:

1. Persoonlijke sleutels apparaat delen met toeleverende partners, naast de PKI worden genegeerd met procedures aanbevolen van persoonlijke sleutels, zorgt ervoor dat de opbouw van vertrouwen in de toeleveringsketen dure nooit delen.  Betekent dit investering systemen zoals beveiligde ruimten tot house apparaat persoonlijke sleutels en processen zoals periodieke beveiligingscontrole moeten worden geïnstalleerd.  Beide toevoegen kosten aan de toeleveringsketen.

2. Veilig accounting voor apparaten in de keten en deze later te beheren in de implementatie wordt een-op-een taak voor elk paar sleutel-naar-apparaat vanaf het moment van apparaat uniek certificaat (daarom persoonlijke sleutel) genereren voor een apparaat buiten gebruik stellen. Dit sluit beheer van groepen van apparaten, tenzij het concept van groepen is expliciet ingebouwd in het proces enigszins. Beveiligde accounting- en levenscyclus beheer, wordt daarom lastig zware bewerkingen.  In ons voorbeeld zou bedrijf X deze last vergeet.

Verificatie via certificaat x.509-CA biedt elegante oplossingen voor afore uitdagingen door het gebruik van certificaatketens vermeld.  Een certificaatketen wordt veroorzaakt door een CA een tussenliggende Certificeringsinstantie die op zijn beurt ondertekent een andere tussenliggende CA en dus gezet totdat een definitieve tussenliggende CA zich aanmeldt voor een apparaat te ondertekenen.  Bedrijf X ondertekent in ons voorbeeld Factory-Y, die op zijn beurt ondertekent technicus-Z die ten slotte smartcard-X-Widget ondertekent.

![IMG cert-keten hiërarchie](./media/cert-chain-hierarchy.png)

Geeft de logische aflevering van autoriteit hierboven cascade van certificaten in de keten.  Veel aanbod ketens Volg deze logische aflevering waarbij elke tussenliggende CA opgehaald bij de keten worden aangemeld tijdens het ontvangen van alle upstream CA-certificaten en de laatste tussenliggende CA ondertekent ten slotte elk apparaat en invoeren van de CA-certificaten van de keten in het apparaat. Dit is normaal wanneer de productiebedrijf contract met een hiërarchie van de fabrieken commissies een bepaalde factory te doen de productie.  Tijdens de hiërarchie mogelijk verschillende niveaus diep (bijvoorbeeld per Geografie/product line type/productie), alleen de fabriek aan het einde opgehaald om te communiceren met het apparaat, maar de keten vanaf de bovenkant van de hiërarchie wordt bijgehouden.

Alternatieve ketens wellicht andere tussenliggende CA communiceren met het apparaat waarop injects geval de CA-interactie met het apparaat inhoud voor keten van certificaten op dat moment.  Hybride modellen zijn ook mogelijk waar alleen enkele van de CA fysiek interactie met het apparaat.

In ons voorbeeld communiceren zowel Factory Y als medewerker Z met de smartcard-X-Widget.  Tijdens het bedrijf X eigenaar is van de smartcard-X-Widget, communiceert het daadwerkelijk fysiek niet met deze in de gehele keten.  De certificaatketen van vertrouwen voor smartcard-X-Widget daarom bestaan uit bedrijf X ondertekening Factory-Y die op zijn beurt ondertekent technicus-Z, die vervolgens laatste handtekening op smartcard-X-object bieden. De productie en de installatie van de smartcard-X-Widget omvatten Factory Y en technicus-Z hun respectieve tussenliggende CA-certificaten gebruikt om elk X-smartcard-Widgets te ondertekenen. Het eindresultaat van deze hele proces is de smartcard-X-Widgets met unieke apparaat-certificaten en certificaatvertrouwensketen naar boven aan bedrijf X CA-certificaat.

![IMG--fabrikant certificaatketen](./media/cert-mfr-chain.png)

Dit is een goede punt om te controleren van de waarde van het x.509-CA-methode.  In plaats van het vooraf genereren en afleveren van certificaten voor elke smartcard-X-object in de keten, hand bedrijf X alleen om te ondertekenen Factory Y eenmaal.  Bedrijf X mag niet in plaats van elk apparaat gedurende de levenscyclus apparaten bijhouden, bijhouden en apparaten beheren via de groepen die natuurlijk ontstaan bij het proces van de keten levering bijvoorbeeld apparaten zijn geïnstalleerd door een technicus Z na juli van een aantal jaar.

Laatste maar niet in het minst infuses de CA-methode voor verificatie beveiligde accountability in het apparaat toelevering productie. De acties van elk lid in de keten is vanwege het proces van de certificaatketen cryptografisch opgenomen en controleerbare.

Dit proces is afhankelijk van bepaalde veronderstellingen die voor de volledigheid moeten worden opgehaald.  Hiervoor moeten onafhankelijk maken van apparaat uniek openbaar/persoonlijk sleutelpaar en dat de persoonlijke sleutel in het apparaat worden beveiligd.  Gelukkig bestaan beveiligde silicon chips in de vorm van Hardware beveiligde Modules (HSM) kunnen intern genereren van sleutels en bescherming van persoonlijke sleutels.  Bedrijf X moet alleen een van deze chips in stuklijst smartcard-X-Widget onderdeel toevoegen.

## <a name="device-connection"></a>Apparaatverbinding

Vorige secties hierboven hebt is aanleggen naar apparaatverbinding.  Via het registreren van een x.509-CA-certificaat naar IoT Hub gewoon een tijd, hoe mogelijk miljoenen apparaten verbinding maken en ophalen van de eerste keer geverifieerd?  Eenvoudige; via de hetzelfde certificaat uploaden en bewijs van bezit stroom we eerder aangetroffen bij het registreren van het x.509-CA-certificaat.

Apparaten die zijn geproduceerd voor verificatie van de x.509-CA zijn uitgerust met unieke certificaten van apparaten en een certificaatketen van de bijbehorende productie toelevering.  Apparaatverbinding, zelfs voor de eerste keer gebeurt in een proces in twee stappen: keten uploaden en bewijs van bezit van het certificaat.

Tijdens het uploaden van de certificaatketen uploadt u het apparaat een uniek apparaatcertificaat samen met de certificaatketen die erin worden geïnstalleerd met IoT Hub.  Met behulp van het vooraf geregistreerde x.509-CA-certificaat, kunt IoT-Hub cryptografisch valideren een aantal dingen, dat de geüploade certificaatketen intern consistent is, en dat de keten is afkomstig van de geldige eigenaar van het x.509-CA-certificaat.  NET was met het registratieproces x.509-CA, IoT-Hub zou start een vraag en antwoord-bewijs van bezit vast te stellen de keten en daarom apparaatcertificaat daadwerkelijk hoort bij het uploaden van het apparaat.  Dit gebeurt door het genereren van willekeurige moeilijk te worden ondertekend met het apparaat dat de persoonlijke sleutel gebruikt voor validatie van de door de IoT Hub.   Een geslaagde reactie activeert IoT Hub het apparaat als authentiek worden geaccepteerd en verleen deze verbinding.

In ons voorbeeld zou elke smartcard-X-Widget haar apparaat uniek certificaat samen met de Factory Y en technicus Z x.509-CA-certificaten te uploaden en vervolgens te reageren op de challenge bewijs van bezit uit IoT Hub.

![IMG-apparaat-pop-stroom](./media/device-pop-flow.png)

U ziet dat de basis van de vertrouwensrelatie is gebaseerd in de beveiliging van persoonlijke sleutels, met inbegrip van persoonlijke sleutels van apparaat.  Er kan geen daarom stress voldoende het belang van beveiligde silicon chips in de vorm van Hardware beveiligde Modules (HSM) voor het beveiligen van persoonlijke sleutels van apparaat en de algehele beste praktijken van het delen van nooit persoonlijke sleutels, zoals een factory aanwijzing van andere bijbehorende persoonlijke sleutel.

