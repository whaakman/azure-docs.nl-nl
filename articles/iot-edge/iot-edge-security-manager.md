---
title: Azure IoT Edge-beveiligingsbeheer | Microsoft Docs
description: Beheert de koers beveiliging van IoT Edge-apparaat en de integriteit van beveiligingsservices.
services: iot-edge
keywords: beveiliging, -element, enclave, IoT Edge
author: eustacea
manager: timlt
ms.author: eustacea
ms.date: 07/30/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 4594685291396b8b80e62abe57be109f0abbd81d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46126340"
---
# <a name="azure-iot-edge-security-manager"></a>Azure IoT Edge-beveiligingsbeheer

De Azure IoT Edge security manager is de kern van een goed begrensde beveiliging voor het beveiligen van de IoT Edge-apparaat en alle bijbehorende onderdelen door om de hardware van de beveiligde silicon te abstraheren. Het is het centrale punt voor beveiliging en voorziet in technologie integratiepunt naar de oorspronkelijke fabrikanten van apparaten (OEM).

![Azure IoT Edge-beveiligingsbeheer](media/edge-security-manager/iot-edge-security-manager.png)

IoT Edge-beveiligingsbeheer is erop gericht op het verdedigen van de integriteit van de IoT Edge-apparaat en alle verbonden software-bewerkingen.  Dit gebeurt door over te stappen van vertrouwensrelatie van de onderliggende hardware hoofdmap van de vertrouwensrelatie hardware (indien beschikbaar) voor het veilig opstarten van IoT Edge-runtime en blijven controleren de integriteit van hun werkzaamheden.  De IoT Edge security manager wordt in wezen bestaat uit software werkt in combinatie met beveiligde silicon hardware, indien beschikbaar en ingeschakeld te laten leveren de hoogste veiligheidsgaranties mogelijk.  

De verantwoordelijkheden van de IoT Edge security manager bevatten (maar niet beperkt tot):

* Beveiligd en gemeten opstarten van de Azure IoT Edge-apparaat.
* Apparaatinrichting voor identiteit en de overgang van de vertrouwensrelatie indien van toepassing.
* Host en beveiligen van Apparaatonderdelen van cloud-services zoals Device Provisioning Service.
* IoT Edge-modules met de unieke id van module veilig inrichten.
* Gatekeeper apparaat hardware vertrouwensbasis via notaris services.
* De integriteit van IoT Edge-bewerkingen tijdens runtime bewaken.

IoT Edge security manager bestaat uit drie primaire onderdelen:

* IoT Edge security daemon.
* Hardware security module platform abstraction Layer (PAL HSM).
* Optioneel maar ten zeerste aanbevolen hardware silicon hoofdmap van de vertrouwensrelatie of HSM.

## <a name="the-iot-edge-security-daemon"></a>De daemon van de IoT Edge-beveiliging

IoT Edge security-daemon is de software die verantwoordelijk is voor de logische bewerkingen van IoT Edge-beveiligingsbeheer.  Het omvat een groot deel van de vertrouwde computing basis van de IoT Edge-apparaat. 

### <a name="design-principles"></a>Ontwerpprincipes

Afdoende en nagenoeg uitvoeren haar verantwoordelijkheden, IoT Edge security-daemon is erop gericht om te voldoen aan de volgende core principes:

#### <a name="maximize-operational-integrity"></a>Operationele integriteit maximaliseren

IoT Edge security-daemon is ontworpen om te werken met de hoogste integriteit mogelijk binnen de mogelijkheid defense van elk opgegeven hoofdmap van de vertrouwensrelatie hardware. Met de juiste integratie, de hoofdmap van de vertrouwensrelatie hardware meet en en gecontroleerd de daemon security statisch tijdens runtime bestand tegen knoeien.

Fysieke toegang is altijd een bedreiging voor IoT-apparaten in het algemeen.  Hardware vertrouwensbasis wordt daarom speelt een belangrijke rol bij het beschermen van de integriteit van de IoT Edge-daemon voor beveiliging.  Vertrouwensbasis hardware zijn er twee versies:

* beveiligde-elementen voor de beveiliging van gevoelige informatie, zoals geheimen en cryptografische sleutels.
* beveiligde enclaves voor de beveiliging van geheimen zoals sleutels en gevoelige werkbelastingen, zoals meten en facturering.

Sprake is van deze twee modellen voor het gebruik van hardware vertrouwensbasis, leidt tot twee soorten uitvoeringsomgevingen:

* De standard- of uitgebreide uitvoeringsomgeving (REE) die afhankelijk zijn van het gebruik van beveiligde-elementen voor het beveiligen van vertrouwelijke gegevens.
* De vertrouwde uitvoeringsomgeving (t) die afhankelijk zijn van het gebruik van beveiligde enclave-technologie voor het beveiligen van vertrouwelijke gegevens en bieden bescherming tot het uitvoeren van software.

Voor apparaten met behulp van veilige enclaves als vertrouwensbasis hardware, gevoelige logica in IoT Edge security-daemon naar verwachting bevinden zich in de enclave.  Niet-gevoelige gegevens delen van de security-daemon kunnen zich buiten de t.  In elk geval naar verwachting van het oorspronkelijke ontwerp fabrikanten (ODM) en original equipment manufacturer (OEM) uit te breiden vertrouwensrelatie van de HSM om te meten en te verdedigen van de integriteit van de Edge security-daemon op het opstart- en runtime.

#### <a name="minimize-bloat-and-churn"></a>Gegevensophoping minimaliseren en verloop

Een andere core-principal van de Edge security-daemon is het verloop te minimaliseren.  Voor het hoogste niveau van de vertrouwensrelatie, IoT Edge security-daemon kan nauw Combineer met de hoofdmap van de hardware apparaat van de vertrouwensrelatie, indien beschikbaar, en kunnen fungeren als systeemeigen code.  Het is gebruikelijk voor deze typen realizations om bij te werken de daemon-software via de hoofdmap van de hardware van vertrouwen van beveiligde update (in plaats van de opgegeven OS-updatemethoden) die kan lastig zijn afhankelijk van specifieke hardware- en -scenario.  Tijdens het vernieuwen van de beveiliging sterk aanbeveling voor IoT-apparaten is, staat deze reden overmatige vereisten of grote update nettoladingen de bedreiging voor aanvallen op tal van manieren kunnen uitbreiden.  Voorbeelden van updates voor een maximale beschikbaarheid operationele overslaan of hoofdmap van de hardware van de vertrouwensrelatie te beperkt tot het verwerken van grote update nettoladingen.  Als zodanig het ontwerp van IoT Edge security-daemon is beknopt te houden van de voetafdruk en wordt daarmee de vertrouwde computing base kleine en om vereisten.

### <a name="architecture-of-iot-edge-security-daemon"></a>Architectuur van IoT Edge security daemon

![Azure IoT Edge security daemon](media/edge-security-manager/iot-edge-security-daemon.png)

De IoT Edge security-daemon is ontworpen om te profiteren van alle beschikbare hardware-hoofdmap van de vertrouwensrelatie technologie voor beveiliging.  Meer dus het is zodanig ontworpen om toe te staan voor de split-wereld-bewerking tussen een Standard/uitgebreide uitvoering omgeving (REE) en een vertrouwde omgeving voor uitvoering (t) om te profiteren van de hardwaretechnologieën die vertrouwde uitvoeringsomgevingen (t) bieden.  Core op de architectuur van de IoT Edge security-daemon is rolspecifieke-interfaces voor het inschakelen van de wisselwerking tussen diagrammen van de belangrijkste onderdelen van Microsoft Edge om te verzekeren dat de integriteit van de IoT Edge-apparaat en de bewerkingen.

#### <a name="cloud-interface"></a>Cloudinterface

De cloudinterface kunt IoT Edge security daemon voor toegang tot cloud-services zoals complimenten van cloud naar apparaatbeveiliging, zoals beveiliging vernieuwing.  Bijvoorbeeld, IoT Edge security-daemon op dit moment maakt gebruik van deze interface voor toegang tot de Azure IoT Hub [Device Provisioning Service (DPS)](https://docs.microsoft.com/azure/iot-dps/) voor beheer van identiteitslevenscycli apparaat.  

#### <a name="management-api"></a>Beheer-API

IoT Edge security daemon biedt een beheer-API, die wordt aangeroepen door de Edge Agent bij het maken/starten/stoppen/verwijderen van een edge-module. De daemon van de beveiliging IoT Edge slaat 'registraties' voor alle actieve modules. Deze registraties toewijzen van een module identiteit aan enkele eigenschappen van de module. Enkele voorbeelden van deze eigenschappen zijn de proces-id (pid) van het proces dat wordt uitgevoerd in de container of de hash van inhoud van de docker-container.

Deze eigenschappen worden gebruikt door de werkbelasting API (Zie hieronder) om te bevestigen dat de aanroeper is gemachtigd een actie uit te voeren.

De API Management is een privileged API, aanroepbare alleen vanaf de IoT Edge-agent.  Aangezien de IoT Edge security-daemon bootstrapt en de IoT Edge-agent wordt gestart, kunt dit een impliciete registratie maken voor de IoT Edge-agent, nadat deze is aangetoond dat de IoT Edge-agent niet is geknoeid met. De attestation-proces die gebruikmaakt van de werkbelasting API wordt gebruikt voor toegang tot de beheer-API om alleen de IoT Edge-agent te beperken.

#### <a name="container-api"></a>Container API

IoT Edge security daemon biedt de container-interface om te communiceren met het containersysteem gebruikt, zoals Moby en Docker voor module instantiëring.

#### <a name="workload-api"></a>Workload API

De werkbelasting API is een IoT Edge-security-daemon API die toegankelijk is voor alle modules, waaronder IoT Edge-agent. Biedt bewijs van identiteit in de vorm van een HSM geroot ondertekende token of X509 certificaat en de bijbehorende vertrouwensrelatie bundel naar een module. De vertrouwensrelatie bundel bevat CA-certificaten voor alle andere servers die de modules moeten vertrouwen.

IoT Edge security-daemon wordt een attestation-proces gebruikt ter bescherming van deze API. Als een module deze API wordt aangeroepen, wordt IoT Edge security daemon gezocht naar een registratie voor de identiteit. Als dit lukt, worden de eigenschappen van de registratie voor het meten van de module. Als het resultaat van de meting proces komt overeen met die de registratie, een nieuwe HSM geroot ondertekend token of X509 certificaat wordt gegenereerd en de bijbehorende keert terug naar de module CA-certificaten (vertrouwensrelatie bundel).  De module gebruikt dit certificaat te verbinden met IoT Hub, andere modules, of een server te starten. Wanneer de ondertekende tokens of het certificaat bijna is verlopen, is het de verantwoordelijkheid van de module aan een nieuw certificaat aanvragen.  Aanvullende overwegingen zijn opgenomen in het ontwerp dat het vernieuwingsproces identiteit zo weinig mogelijk te maken.

### <a name="integration-and-maintenance"></a>Integratie en onderhoud

Microsoft onderhoudt de belangrijkste code voor de [IoT Edge security-daemon op GitHub](https://github.com/Azure/iotedge/tree/master/edgelet).

#### <a name="installation-and-updates"></a>Installatie en updates

Wordt een eigen proces, wordt installatie en het bijwerken van de IoT Edge security-daemon beheerd via het pakketbeheersysteem van het besturingssysteem.  Het is echter waarschijnlijk dat IoT Edge-apparaten met hardware vertrouwensbasis en bieden extra beveiliging voor de integriteit van de daemon door het beheer van levenscyclus via de apparaten veilig opstarten en updates beheersystemen.  Het is van links naar apparaten makers deze mogelijkheden in overeenstemming met hun respectieve apparaatmogelijkheden verkennen.

#### <a name="versioning"></a>Versiebeheer

IoT Edge-runtime worden bijgehouden en de versie van de IoT Edge security-daemon-rapporten. De versie wordt gerapporteerd als de *runtime.platform.version* kenmerk van de Edge Agent-module gerapporteerde eigenschap.

### <a name="hardware-security-module-platform-abstraction-layer-hsm-pal"></a>Hardware security module platform abstraction layer (HSM PAL)

De HSM-PAL isoleert alle hoofdmap van de vertrouwensrelatie hardware voor het isoleren van de ontwikkelaar of gebruiker van IoT Edge van de complexiteit.  Het omvat een combinatie van Application Programming Interface (API) en trans domein communicatie procedures, bijvoorbeeld de communicatie tussen een standard uitvoeringsomgeving en een veilige enclave.  De daadwerkelijke implementatie van de HSM-PAL, is afhankelijk van de specifieke beveiligde hardware wordt gebruikt.  Begin van haar bestaan maakt het gebruik van vrijwel elke beveiligde silicon hardware voor de IoT-ecosysteem.

## <a name="secure-silicon-root-of-trust-hardware"></a>Beveiligde silicon hoofdmap van de vertrouwensrelatie hardware

Beveiligde silicon is nodig om anker vertrouwen in de hardware van de IoT Edge-apparaten.  Beveiligde silicon worden geleverd in verschillende om op te nemen Trusted Platform Module (TPM), ingesloten beveiligde Element (eSM), ARM Trustzone, Intel SGX en aangepaste beveiligde silicon technologieën.  Het gebruik van beveiligde silicon vertrouwensbasis op apparaten wordt sterk aanbevolen gegeven van de bedreigingen die zijn gekoppeld aan een fysiek de toegankelijkheid van de IoT-apparaten.

## <a name="iot-edge-security-manager-integration-and-maintenance"></a>Integratie van IoT Edge security manager en onderhoud

Één belangrijk doel van de IoT Edge security manager is om identiteit en de onderdelen die belast zijn met de beveiliging en integriteit van de Azure IoT Edge-platform voor aangepaste beveiliging verdediging te isoleren.  Als zodanig van derden, zoals apparatenmakers naar verwachting maken gebruik van aangepaste beveiligingsfuncties die beschikbaar zijn met hun hardware-apparaten.  Zie de sectie volgende stappen hieronder op koppelingen naar voorbeeld over het versterken van de Azure IoT security manager met de Trusted Platform Module (TPM) op Linux en Windows-platforms.  Deze voorbeelden software of virtuele TPM's gebruiken maar rechtstreeks van toepassing is voor het gebruik van afzonderlijke TPM-apparaten.  

## <a name="next-steps"></a>Volgende stappen

Lees de blog op [beveiligen van de intelligente edge](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).

Maken en inrichten van een Edge-apparaat met een [virtuele TPM op een Linux-machine](how-to-auto-provision-simulated-device-linux.md).

Maken en inrichten een [gesimuleerd TPM-Edge-apparaat op Windows](how-to-auto-provision-simulated-device-windows.md).

<!-- Links -->
[lnk-edge-blog]: https://azure.microsoft.com/blog/securing-the-intelligent-edge/
