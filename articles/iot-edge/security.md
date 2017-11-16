---
title: Beveiliging in Azure IoT-rand | Microsoft Docs
description: Beveiliging, verificatie en autorisatie van IoT Edge-apparaten
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 8a5bf1f35fcdd779cf27edeba7dfd5705cbae205
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="securing-azure-iot-edge---preview"></a>Beveiligen van Azure IoT-Edge - preview

Beveiliging van de rand intelligent is nodig om het verlenen van vertrouwen in de werking van een end-to-end-IoT-oplossing. Azure IoT-rand is ontworpen voor beveiliging die kan worden uitgebreid naar andere risicoprofiel, implementatiescenario's en biedt dezelfde beveiliging die u van alle Azure-services verwacht.

Azure IoT-rand wordt uitgevoerd op andere hardware, biedt ondersteuning voor Linux- en Windows en is van toepassing op verschillende implementatiescenario's.  Beoordeelde risico is afhankelijk van een aantal zaken zoals oplossing eigendom, implementatie Geografie, gegevens gevoeligheid, privacy, application verticale en wettelijke vereisten.  In plaats van het aanbieden van concrete oplossingen voor specifieke scenario's, is het zinvol voor het ontwerpen van een uitbreidbare security-framework op basis van goed geaarde principes ontworpen voor schaal. 
 
Dit artikel bevat een overzicht van de security-framework. Zie voor meer informatie [beveiliging van de rand intelligent][lnk-edge-blog].

>[!NOTE]
>Beveiligingskader voor het hieronder beschreven is nu wordt toevoegen aan het product en beschikbaar zijn bij het vrijgeven van de algemene beschikbaarheid van Azure IoT rand. Het product is momenteel in de openbare preview, bedoeld een release te maken voor ontwikkeling en maken van een prototype van rand oplossingen, geen volledige productie-implementaties die het beveiligingskader voor het volledige moeten.   

## <a name="standards"></a>Standaarden

Standaarden promoveren gebruiksgemak controle en het gemak van implementatie, die het kenmerk van beveiliging.  Een goede architectuur beveiligingsoplossing moet zichzelf goed lenen voor controle onder evaluatie voor het bouwen van de vertrouwensrelatie en mag geen een drempel voor implementatie.  Het ontwerp van het framework voor het beveiligen van Azure IoT rand toe vanuit duurzaamheid en bedrijfstak bewezen beveiliging protocollen bekend zijn en hergebruik en gebruik te maken. 

## <a name="authentication"></a>Authentication

Zonder twijfel weten welke actoren, apparaten en onderdelen die deel uitmaken van de levering via een end-to-end-IoT-oplossing van cruciaal belang bij het bouwen van de vertrouwensrelatie is.  Deze kennis biedt veilige accountability van deelnemers aan het inschakelen van de basis voor toegangsbeheer.  Azure IoT-rand attains deze kennis door middel van verificatie.  Het belangrijkste mechanisme voor verificatie voor de rand van Azure IoT-platform is verificatie op basis van certificaten.  Dit mechanisme is afgeleid van een verzameling standaarden van bestuur Public Key Infrastructure (PKiX) door de Internet Engineering Task Force (IETF).     

Het beveiligingskader voor het Azure-IoT-Edge-aanroepen voor uniek certificaat identiteiten voor alle apparaten, modules (containers dat logica in het apparaat inkapselen) en actoren interactie met de Azure-IoT-randapparaat fysiek of via een netwerkverbinding.  Niet elk scenario of onderdeel kan zichzelf goed lenen voor verificatie op basis van een certificaat waarvoor de uitbreidbaarheid van beveiligingskader voor het beveiligde mogelijkheden voor accommodatie biedt. 

## <a name="authorization"></a>Autorisatie

De mogelijkheid om te delegeren-instantie en beheren van toegang is van cruciaal belang bij de verwezenlijking van een fundamenteel beveiligingsprincipe – het principe van minimale bevoegdheden.  Apparaten, modules en actoren kunnen alleen toegang tot bronnen en gegevens binnen hun bereik machtiging en wanneer het qua architectuur toegestane krijgen.  Dit betekent dat sommige machtigingen kunnen worden geconfigureerd met voldoende bevoegdheden en andere qua architectuur afgedwongen.  Bijvoorbeeld, is een module kan worden toegestaan door bevoorrechte configuratie om een verbinding met Azure IoT Hub te zetten, er geen reden waarom een module in een Azure-IoT-randapparaat toegang de twin van een module in een andere Azure-IoT-randapparaat tot moet.  Daarom moet de laatste zou worden qua architectuur belet om. 

Andere autorisatie schema's zijn certificaat ondertekenen rechten en op rollen gebaseerde toegangsbeheer (RBAC).  De uitbreidbaarheid van de security-framework is goedkeuring van andere volwassen autorisatie-schema's toegestaan. 

## <a name="attestation"></a>Attestation

Attestation zorgt ervoor dat de integriteit van software-onderdelen.  Het is belangrijk voor de detectie en preventie van schadelijke software.  Het beveiligingskader voor het Azure-IoT-rand wordt geclassificeerd attestation in drie hoofdcategorieën:

* Statische attestation
* Runtime-attestation
* Software attestation

### <a name="static-attestation"></a>Statische attestation

Statische attestation is de verificatie van de integriteit van alle software-onderdelen, met inbegrip van de besturingssystemen, alle runtimes en configuratie-informatie op het apparaat opstarten.  Dit wordt vaak aangeduid als beveiligd opstarten.  Het framework security voor Azure IoT-randapparaten strekt zich uit tot silicon leveranciers en mogelijkheden om te verzekeren dat statische attestation processen beveiligde hardware is opgenomen. Deze processen zijn beveiligd opstarten en beveiligde firmware bijwerken van processen.  In nauw overleg met silicon leveranciers elimineert overbodige firmware lagen waardoor het oppervlak threat minimaliseren. 

### <a name="runtime-attestation"></a>Runtime-attestation

Zodra een systeem een gevalideerde opstartproces is voltooid en actief is en actieve, goed ontworpen beveiligde systemen zou detecteren probeert te injecteren van schadelijke software via de systemen poorten en interfaces en de juiste tegenmaatregelen neemt.  Voor intelligent randapparaten in fysieke bewaring van schadelijke actoren, is het mogelijk om te injecteren ontevreden klant wijze dan apparaatinterfaces zoals knoeien en aanvallen van de kant-kanaal.   Dergelijke ontevreden klant, die in de vorm van kwaadaardige software of niet-geautoriseerde configuratiewijzigingen worden kan, normaal gesproken niet kunnen worden gedetecteerd door het proces voor beveiligd opstarten omdat ze na het opstartproces gebeuren.  Tegenmaatregelen aangeboden of afgedwongen door het apparaat hardware aanzienlijk bijdraagt warding uit dergelijke bedreigingen.  Het framework security voor Azure IoT rand illustreert expliciet voor uitbreidingen voor combatting runtime bedreigingen.     

### <a name="software-attestation"></a>Software attestation

Alle orde systemen inclusief intelligent rand systemen moet voorwerp patches en upgrades.  Beveiliging is belangrijk voor de update-processen anders dat deze potentiële bedreigingen met zich mee kunnen zijn.  Het beveiligingskader voor aanroepen voor updates via Azure IoT rand gemeten en pakketten voor de integriteit garanderen en verifiëren van de bron van de pakketten zijn ondertekend.  Dit is van toepassing op alle besturingssystemen en software-bits toepassing. 

## <a name="hardware-root-of-trust"></a>Hoofdmap van de hardware van vertrouwensrelatie

Beveiliging die geboden worden door de hardware is het laatste defensie voor beveiliging voor veel implementaties van intelligent edge-apparaten, met name voor die in de locaties waar mogelijk schadelijke actoren fysieke toegang tot het apparaat hebt geïmplementeerd.  Daarom is verankerende vertrouwen in verzegelde tegen hardware meest cruciale voor dergelijke implementaties.  Het framework security voor Azure IoT rand houdt samenwerking van beveiligde silicon hardwareleveranciers bieden verschillende versies van de hoofdmap van de hardware van vertrouwen voor verschillende risicoprofiel en implementatiescenario's. Het gaat hierbij om gebruik van beveiligde silicon aan algemene beveiligingsstandaarden van het type protocol zoals Trusted Platform Module (ISO/IEC 11889) en Trusted Computing Group van apparaat-id samenstelling-Engine (OPDELEN).  Hieronder vallen ook beveiligde enclave technologieën zoals TrustZones en Software Guard-extensies (SGX). 

## <a name="certification"></a>Certificering

Zodat klanten gefundeerde beslissingen nemen wanneer het kopen van Azure IoT-randapparaten voor de implementatie omvat het framework Azure IoT rand certificeringsvereisten.  Fundamentele aan deze vereisten zijn met betrekking tot beveiligingsclaims certificeringen en certificeringen met betrekking tot de validatie van de implementatie van beveiliging.  Een claim beveiliging certificering zou bijvoorbeeld informeren dat apparaat aan de rand van de IoT maakt gebruik van beveiligde hardware bekend is dat het bestand tegen aanvallen van de opstartinstallatiekopie. Een validatie-certificering zou informeren over de beveiligde hardware op de juiste wijze is geïmplementeerd om deze waarde in het apparaat aan te bieden.  Aan het principe van eenvoud biedt het framework de visie om de belasting van de certificeringsinstantie minimale te houden.   

## <a name="extensibility"></a>Uitbreidbaarheid

Uitbreidbaarheid is een uitstekende burger in het beveiligingskader voor het Azure IoT rand.  Met IoT-technologie kunnen verschillende soorten business transformaties, staat deze reden moet zich beveiliging naadloos ontwikkelen in lockstep naar adres opkomende scenario's.  Het beveiligingskader voor het Azure IoT rand begint met een solide basis waarop builds in uitbreidbaarheid in verschillende dimensies om op te nemen: 

* Eerste beveiligingsservices van derden, zoals de Service voor het inrichten van apparaten voor Azure IoT Hub.
* Services van derden, zoals beheerde beveiligingsservices voor verschillende groepen van toepassingen verticalen (zoals industriële of gezondheidszorg) of technologie focus (zoals veiligheidscontrole netwerken of silicon hardware attestation-services NET) via een uitgebreide netwerk van partners.
* Oudere systemen om op te nemen met terugwerkende kracht met alternatieve beveiligingsstrategieën, zoals het gebruik van beveiligde technologieën dan certificaten voor authenticatie en identiteit management.
* Hardware voor naadloze ingebruikname van nieuwe beveiligde hardware-technologieën en silicon bijdragen vanuit de partner beveiligen.

Dit zijn enkele voorbeelden van dimensies voor uitbreidbaarheid en Azure IoT rand security-framework is ontworpen om te worden beveiligd tegen een compleet nieuwe ter ondersteuning van deze uitbreidbaarheid. 

Uiteindelijk het hoogste succes in het beveiligen van de rand intelligent resultaat is van een gezamenlijke bijdragen van een open aangedreven door het gemeenschappelijk belang bij de beveiliging van IoT-community.  Deze bijdragen mogelijk in de vorm van beveiligde technologieën of services.  De Azure IoT rand security-framework biedt een solide basis vormt voor beveiliging die kan worden uitgebreid voor de maximale dekking te bieden dezelfde mate van vertrouwen en integriteit in het intelligent edge als met de Azure-cloud.  

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe Azure IoT-rand wordt [beveiliging van de rand intelligent][lnk-edge-blog].

<!-- Links -->
[lnk-edge-blog]: https://azure.microsoft.com/blog/securing-the-intelligent-edge/ 