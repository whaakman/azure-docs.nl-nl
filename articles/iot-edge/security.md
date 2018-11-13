---
title: Beveiliging in Azure IoT Edge | Microsoft Docs
description: Beveiliging, verificatie en autorisatie van IoT Edge-apparaten
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 4292dc3abf775d48ee33cc7a48793d080156efb1
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568934"
---
# <a name="securing-azure-iot-edge"></a>Azure IoT Edge beveiligen

Het beveiligen van de intelligente edge is die nodig zijn voor het verlenen van vertrouwen in de werking van een end-to-end-IoT-oplossing. Azure IoT Edge is ontworpen voor beveiliging die is uitgebreid om andere risicoprofiel, implementatiescenario's, en biedt de dezelfde beveiliging die u kunt van alle Azure-services verwachten.

Azure IoT Edge wordt uitgevoerd op andere hardware ondersteunt zowel Windows als Linux en is van toepassing op verschillende implementatiescenario's.  Geëvalueerde risico's, is afhankelijk van veel overwegingen, met inbegrip van eigendom van de oplossing, geografische implementatiegebied, vertrouwelijkheid van gegevens, privacy, verticaal en wettelijke vereisten van toepassing.  In plaats van concrete oplossingen voor specifieke scenario's biedt, is het verstandig om het ontwerpen van een uitbreidbare security framework op basis van goed gevestigd principes die is ontworpen voor schaalbaarheid. 
 
Dit artikel bevat een overzicht van de security-framework. Zie voor meer informatie, [beveiligen van de intelligente edge](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).

## <a name="standards"></a>Standaarden

Standaarden bevorderen gebruiksgemak controle en het gemak van implementatie, die het kenmerk van beveiliging.  Een goed Ontwerppunten beveiligingsoplossing moet leent voor controle onder evaluatie vertrouwen te bouwen en mag geen een drempel voor implementatie.  Het ontwerp van het framework voor het beveiligen van Azure IoT Edge toe vanuit beproefde en beproefde security protocollen gebruikmaken van kennis en hergebruik. 

## <a name="authentication"></a>Verificatie

Zonder een waarover twijfel bestaat te weten welke actors, apparaten en -onderdelen zijn deelnemen aan het leveren van waarde dankzij een complete IoT-oplossing is cruciaal bij het bouwen van vertrouwen.  Deze kennis biedt veilige verantwoordelijkheid van de deelnemers aan het inschakelen van de basis voor toegangsbeheer.  Azure IoT Edge attains deze kennis door middel van verificatie.  Het primaire mechanisme voor verificatie voor de Azure IoT Edge-platform is verificatie op basis van certificaten.  Dit mechanisme is afgeleid van een set normen opgelegd Public Key Infrastructure (PKiX) door de Internet Engineering Task Force (IETF).     

Het Azure IoT Edge security framework aanroepen voor uniek certificaat-id's voor alle apparaten, modules (containers waarin de logica in het apparaat) en actoren interactie met de Azure IoT Edge-apparaat fysiek of via een netwerkverbinding.  Niet elk scenario of het onderdeel kan leent voor verificatie op basis van een certificaat waarvoor de uitbreidbaarheid van het security framework beveiligde mogelijkheden voor accommodatie biedt. 

## <a name="authorization"></a>Autorisatie

De mogelijkheid om te delegeren-instantie en beheren van toegang is van cruciaal belang voor het bereiken van een fundamentele beveiligingsprincipal – het principe van minimale bevoegdheden.  Apparaten, modules en actoren kunnen alleen toegang tot bronnen en gegevens binnen hun machtigingsbereik en alleen wanneer deze een toegestane krijgen.  Dit betekent dat sommige machtigingen kunnen worden geconfigureerd met voldoende bevoegdheden en anderen qua afgedwongen.  Bijvoorbeeld, terwijl een module mag via bevoegde configuratie opnieuw verbinding maken met Azure IoT Hub, is er geen reden waarom een module in een Azure IoT Edge-apparaat toegang het dubbele van een module in een andere Azure IoT Edge-apparaat tot moet.  Daarom moet de laatste zou worden qua belet om. 

Andere autorisatie schema's zijn certificaat ondertekenen rechten en op rollen gebaseerd toegangsbeheer (RBAC).  De uitbreidbaarheid van het security framework kan acceptatie van andere volwassen autorisatie-schema's. 

## <a name="attestation"></a>Attestation

Attestation waarborgt de integriteit van software-bits.  Het is belangrijk voor de detectie en preventie van schadelijke software.  Het Azure IoT Edge security framework classificeert attestation in drie hoofdcategorieën:

* Statische attestation
* Runtime-attestation
* Software-attestation

### <a name="static-attestation"></a>Statische attestation

Statische attestation is het verifiëren van de integriteit van alle software-bits, met inbegrip van de besturingssystemen, alle runtimes en configuratie-informatie op apparaat power-up.  Dit wordt vaak aangeduid als beveiligd opstarten.  De security-framework voor Azure IoT Edge-apparaten strekt zich uit tot silicon leveranciers en hardwaremogelijkheden om te verzekeren dat statische attestation processen beveiligde is opgenomen. Deze processen zijn beveiligd opstarten en veilige firmware bijwerken processen.  Werken in nauwe samenwerking met silicon leveranciers elimineert overbodige firmware lagen, waardoor het oppervlak bedreigingen te minimaliseren. 

### <a name="runtime-attestation"></a>Runtime-attestation

Zodra een systeem een gevalideerde opstartproces is voltooid en actief is en wordt uitgevoerd, goed ontworpen beveiligde systemen zou detecteren probeert te injecteren malware verspreiden via de systemen poorten en interfaces en juiste tegenmaatregelen te nemen.  Voor intelligente edge-apparaten in fysieke bewaring van kwaadwillende actoren, is het mogelijk om te injecteren ontevreden klant via middelen dan apparaatinterfaces, zoals geknoei en side-channel-aanvallen.   Dergelijke ontevreden klant, die in de vorm van kwaadaardige software of niet-geautoriseerde wijzigingen worden kan, doorgaans niet kunnen worden gedetecteerd door het proces voor beveiligd opstarten omdat deze zich na het opstartproces voordoen.  Tegenmaatregelen aangeboden of afgedwongen door hardware die van het apparaat aanzienlijk bijdraagt warding uit deze bedreigingen.  Het framework van de beveiliging voor Azure IoT Edge expliciet worden aangeroepen voor uitbreidingen voor het bestrijden van runtime bedreigingen.     

### <a name="software-attestation"></a>Software-attestation

Alle in orde systemen, met inbegrip van intelligente edge systemen moet aanmerking voor patches en upgrades.  Beveiliging is belangrijk voor de update-processen anders dat deze potentiële bedreigingen met kunnen zijn.  Het framework van de beveiliging voor Azure IoT Edge-aanroepen voor updates via gemeten en ondertekende pakketten om te garanderen de integriteit en de bron van de pakketten te verifiëren.  Dit is van toepassing op alle besturingssystemen en software-bits toepassing. 

## <a name="hardware-root-of-trust"></a>Hardware-vertrouwensbasis

Voor veel implementaties van intelligente edge-apparaten, met name degenen die geïmplementeerd op locaties waar potentiële kwaadwillende actoren fysieke toegang tot het apparaat hebben is beveiliging wordt geboden door het apparaat het laatste defensie voor beveiliging.  Om deze reden is verankerende vertrouwen in fraudebestendig hardware meest cruciale voor dergelijke implementaties.  Het framework van de beveiliging voor Azure IoT Edge omvat deze vorm van beveiligde silicon hardwareleveranciers verschillende versies van hardware vertrouwensbasis voor verschillende risicoprofielen en implementatiescenario's bieden. Het gaat hierbij om gebruik van beveiligde silicon aan algemene beveiliging protocolstandaarden, zoals Trusted Platform Module (ISO/IEC 11889) en Trusted Computing Group van apparaat-id Composition Engine (DICE).  Deze omvatten ook beveiligde enclave-technologieën, zoals TrustZones en Software Guard-extensies (SGX). 

## <a name="certification"></a>Certificering

Om klanten te helpen geïnformeerde beslissingen bij het aanschaffen van Azure IoT Edge-apparaten voor hun implementatie, bevat de Azure IoT Edge-framework vereisten voor certificering.  Fundamentele aan deze vereisten zijn met betrekking tot beveiligingsclaims-certificeringen en certificeringen die betrekking hebben op de validatie van de implementatie van beveiliging.  Een certificaat van de claim security zou bijvoorbeeld informeren dat de IoT Edge-apparaat maakt gebruik van beveiligde hardware die bekend zijn bij het opstarten aanvallen verzetten. Een validatie-certificering zou informeren over de veilige hardware op de juiste wijze is geïmplementeerd voor deze waarde in het apparaat.  Aan het principe van de eenvoud biedt het framework de visie van de werkbelasting van certificering minimale houden.   

## <a name="extensibility"></a>Uitbreidbaarheid

Uitbreidbaarheid is een eersteklas burgers in het Azure IoT Edge security-framework.  Met IoT-technologie verschillende typen zakelijke transformaties te stimuleren, staat deze reden dat beveiliging naadloos te veranderen in lockstep naar adres opkomende scenario's.  Het Azure IoT Edge security framework begint met een solide basis waarop deze wordt gemaakt in uitbreidbaarheid in verschillende dimensies om op te nemen: 

* Eerste security services van derden, zoals de Device Provisioning Service voor Azure IoT Hub.
* Services van derden zoals beheerde beveiligingsservices voor andere toepassing verticalen (zoals industriële of gezondheidszorg) of technologie focus (zoals bewaking van de beveiliging netwerken of silicon hardware attestation-services NET) via een uitgebreide netwerk van partners.
* Verouderde systemen om op te nemen met alternatieve strategieën, aanpassing, zoals met behulp van veilige technologie dan certificaten voor verificatie en identiteitsbeheer.
* Beveilig hardware voor naadloze ingebruikname van nieuwe hardwaretechnologieën voor veilige en silicon partner bijdragen.

Dit zijn slechts enkele voorbeelden van dimensies voor uitbreidbaarheid en Azure IoT Edge security framework is ontworpen om te beveiligen vanaf de grond ter ondersteuning van deze uitbreidbaarheid. 

Uiteindelijk moet het hoogste succes bij het beveiligen van de intelligente edge resultaat is van een gezamenlijke bijdragen van een openen door de community onderhouden door het gemeenschappelijk belang bij het beveiligen van IoT.  Deze bijdragen mogelijk in de vorm van beveiligde technologieën of services.  Het Azure IoT Edge security framework biedt een solide basis voor beveiliging dat kan worden uitgebreid voor de maximale dekking te bieden dezelfde mate van vertrouwen en integriteit in de intelligente edge als met Azure-cloud.  

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe Azure IoT Edge is [beveiligen van de intelligente edge](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).
