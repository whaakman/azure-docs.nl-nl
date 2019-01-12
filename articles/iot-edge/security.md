---
title: Security framework - Azure IoT Edge | Microsoft Docs
description: Meer informatie over de beveiliging, verificatie en autorisatie-normen die zijn gebruikt voor het ontwikkelen van Azure IoT Edge en moeten worden beschouwd als u uw oplossing ontwerpt
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: b174d7f9b4b8438687512a90dc7a65b5649f758a
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54229883"
---
# <a name="security-standards-for-azure-iot-edge"></a>Beveiligingsstandaarden voor Azure IoT Edge

Uw gegevens en analyses verplaatsen naar de intelligente edge maakt scenario's voor risico's die Azure IoT Edge is ontworpen om een adres. De IoT Edge-beveiligingsstandaarden zijn bedoeld om flexibiliteit te bieden voor verschillende risicoprofielen en implementatiescenario's terwijl tegelijk de dezelfde beveiliging die u kunt van alle Azure-services verwachten. 

Azure IoT Edge wordt uitgevoerd op verschillende hardware merken en modellen, ondersteunt verschillende besturingssystemen en van toepassing is op diverse implementatiescenario's. Beoordeling van het risico van een implementatiescenario, is afhankelijk van veel overwegingen, met inbegrip van eigendom van de oplossing, geografische implementatiegebied, vertrouwelijkheid van gegevens, privacy, verticaal en wettelijke vereisten van toepassing. In plaats van concrete oplossingen voor specifieke scenario's biedt, is IoT Edge een uitbreidbaar beveiliging op basis van goed gevestigd principes die is ontworpen voor schaalbaarheid. 
 
Dit artikel bevat een overzicht van het IoT Edge security-framework. Zie voor meer informatie, [beveiligen van de intelligente edge](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).

## <a name="standards"></a>Standaarden

Standaarden bevorderen gebruiksgemak controle en het gemak van implementatie, die beide hallmarks van beveiliging zijn. Een beveiligingsoplossing moet leent voor controle onder evaluatie vertrouwen te bouwen en mag niet een drempel voor implementatie. Het ontwerp van het framework voor het beveiligen van Azure IoT Edge is gebaseerd op beproefde en beproefde security protocollen voor kennis en hergebruik. 

## <a name="authentication"></a>Verificatie

Wanneer u een IoT-oplossing implementeert, moet u weten dat alleen vertrouwde actors, apparaten en -onderdelen toegang tot uw oplossing hebben. Deze kennis biedt veilige verantwoordelijkheid van de deelnemers aan het inschakelen van de basis voor toegangsbeheer.  Azure IoT Edge attains deze kennis door middel van verificatie.  Het primaire mechanisme voor verificatie voor de Azure IoT Edge-platform is verificatie op basis van certificaten.  Dit mechanisme is afgeleid van een set normen opgelegd Public Key Infrastructure (PKiX) door de Internet Engineering Task Force (IETF).     

Alle apparaten, modules en actoren interactie met de Azure IoT Edge-apparaat, of fysiek of via een netwerkverbinding, moeten uniek certificaat-id's hebben. Niet elk scenario of het onderdeel kan leent voor verificatie op basis van certificaten. In deze scenario's biedt de uitbreidbaarheid van het security framework beveiligde alternatieven. 

## <a name="authorization"></a>Autorisatie

Het principe van minimale bevoegdheden zegt dat gebruikers en de onderdelen van een systeem moeten alleen toegang hebben tot de minimale set resources en gegevens die nodig zijn om uit te voeren hun rollen. Apparaten, modules en actoren moeten toegang tot alleen de resources en gegevens binnen het machtigingsbereik van hun, en alleen wanneer deze een toegestane. Sommige machtigingen kunnen worden geconfigureerd met voldoende machtigingen en anderen qua afgedwongen.  Bijvoorbeeld, een module mag via bevoegde configuratie opnieuw verbinding maken met Azure IoT Hub. Er is echter geen reden waarom een module in een Azure IoT Edge-apparaat toegang het dubbele van een module in een andere Azure IoT Edge-apparaat tot moet.

Andere autorisatie schema's zijn Certificaatondertekening rechten, op rollen gebaseerd toegangsbeheer (RBAC) en andere volwassen autorisatie-schema's. 

## <a name="attestation"></a>Attestation

Attestation waarborgt de integriteit van software-bits.  Het is belangrijk voor de detectie en preventie van schadelijke software.  Het Azure IoT Edge security framework classificeert attestation in drie hoofdcategorieën:

* Statische attestation
* Runtime-attestation
* Software-attestation

### <a name="static-attestation"></a>Statische attestation

Statische attestation controleert de integriteit van alle software op een apparaat, met inbegrip van het besturingssysteem, alle runtimes en configuratie-informatie op opstarten van het apparaat. Dit wordt vaak aangeduid als beveiligd opstarten. De security-framework voor Azure IoT Edge-apparaten strekt zich uit tot fabrikanten en tevens beveiligde hardwaremogelijkheden om statische attestation-processen. Deze processen zijn beveiligd opstarten en veilige firmware bijwerken.  Werken in nauwe samenwerking met silicon leveranciers elimineert overbodige firmware-lagen, dus minimaliseert de bedreiging voor aanvallen. 

### <a name="runtime-attestation"></a>Runtime-attestation

Zodra een systeem is een proces voor beveiligd opstarten is voltooid en actief is, zou goed ontworpen systemen pogingen tot het invoeren van schadelijke software en het juiste tegenmaatregelen neemt detecteert. Malware-aanvallen kunnen richten op de poorten en interfaces voor de toegang tot het systeem van het systeem. Of, als kwaadwillende actoren fysieke toegang hebben tot een apparaat dat ze kunnen knoeien met het apparaat zelf of side-channel-aanvallen gebruiken om toegang te krijgen. Dergelijke ontevreden klant, die in de vorm van kwaadaardige software of niet-geautoriseerde wijzigingen worden kan, is opgenomen na het opstartproces, zodat de statische attestation wouldn't worden gedetecteerd. Tegenmaatregelen aangeboden of afgedwongen door van het apparaat hardware help buiten deze bedreigingen.  Het framework van de beveiliging voor Azure IoT Edge-expliciet aanroepen voor uitbreidingen die runtime bedreigingen Combat.  

### <a name="software-attestation"></a>Software-attestation

Alle in orde systemen, met inbegrip van intelligente edge systemen moeten accepteren patches en upgrades.  Beveiliging is belangrijk voor update-processen anders dat deze potentiële bedreigingen met kunnen zijn.  Het framework van de beveiliging voor Azure IoT Edge-aanroepen voor updates via gemeten en ondertekende pakketten om te garanderen de integriteit en de bron van de pakketten te verifiëren.  Deze standaard is van toepassing op alle besturingssystemen en software-bits toepassing. 

## <a name="hardware-root-of-trust"></a>Hardware-vertrouwensbasis

Voor veel intelligente edge-apparaten, met name voor apparaten op locaties waar potentiële kwaadwillende actoren fysieke toegang tot het apparaat hebben is hardware apparaatbeveiliging het laatste defensie voor beveiliging. Fraudebestendig hardware is essentieel voor dergelijke implementaties. Azure IoT Edge worden gebruikers aangemoedigd beveiligde silicon hardwareleveranciers verschillende versies van hardware vertrouwensbasis voor verschillende risicoprofielen en implementatiescenario's bieden. Hardware-vertrouwensrelatie kan afkomstig zijn van de algemene beveiliging protocolstandaarden, zoals Trusted Platform Module (ISO/IEC 11889) en Trusted Computing Group van apparaat-id Composition Engine (DICE). Beveilig de enclave-technologieën zoals TrustZones en Software Guard-extensies (SGX) bieden ook hardware vertrouwen. 

## <a name="certification"></a>Certificering

Om klanten te helpen geïnformeerde beslissingen bij het aanschaffen van Azure IoT Edge-apparaten voor hun implementatie, bevat de Azure IoT Edge-framework vereisten voor certificering.  Fundamentele aan deze vereisten zijn met betrekking tot beveiligingsclaims-certificeringen en certificeringen die betrekking hebben op de validatie van de implementatie van beveiliging.  Een certificaat van de claim security zou bijvoorbeeld informeren dat de IoT Edge-apparaat maakt gebruik van beveiligde hardware die bekend zijn bij het opstarten aanvallen verzetten. Een validatie-certificering zou informeren over de veilige hardware op de juiste wijze is geïmplementeerd voor deze waarde in het apparaat.  Aan het principe van de eenvoud probeert het framework dat de werkbelasting van certificering minimale.   

## <a name="extensibility"></a>Uitbreidbaarheid

Uitbreidbaarheid is een eersteklas burgers in het Azure IoT Edge security-framework.  Met IoT-technologie verschillende typen zakelijke transformaties te stimuleren, staat deze reden dat beveiliging moet veranderen parallel naar adres opkomende scenario's.  Het Azure IoT Edge security framework begint met een solide basis waarop deze wordt gemaakt in uitbreidbaarheid in verschillende dimensies om op te nemen: 

* Eerste security services van derden, zoals de Device Provisioning Service voor Azure IoT Hub.
* Services van derden zoals beheerde beveiligingsservices voor andere toepassing verticalen (zoals industriële of gezondheidszorg) of technologie focus (zoals bewaking van de beveiliging netwerken of silicon hardware attestation-services NET) via een uitgebreide netwerk van partners.
* Verouderde systemen om op te nemen met alternatieve strategieën, aanpassing, zoals met behulp van veilige technologie dan certificaten voor verificatie en identiteitsbeheer.
* Beveilig hardware voor acceptatie van nieuwe hardwaretechnologieën voor veilige en silicon partner bijdragen.

Uiteindelijk moet het hoogste succes bij het beveiligen van de intelligente edge resultaat is van een gezamenlijke bijdragen van een openen door de community onderhouden door het gemeenschappelijk belang bij het beveiligen van IoT.  Deze bijdragen mogelijk in de vorm van beveiligde technologieën of services.  Het Azure IoT Edge security framework biedt een solide basis voor beveiliging dat kan worden uitgebreid voor de maximale dekking te bieden dezelfde mate van vertrouwen en integriteit in de intelligente edge als met Azure-cloud.  

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe Azure IoT Edge is [beveiligen van de intelligente edge](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).
