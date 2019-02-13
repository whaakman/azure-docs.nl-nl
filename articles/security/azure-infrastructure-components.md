---
title: Onderdelen van het systeem Azure informatie en grenzen
description: Dit artikel bevat een algemene beschrijving van de Microsoft Azure-architectuur en het beheer.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: b390dc9bd2b690837a85a5bab361a534b9c9d5a5
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118136"
---
# <a name="azure-information-system-components-and-boundaries"></a>Onderdelen van het systeem Azure informatie en grenzen
Dit artikel bevat een algemene beschrijving van de Azure-architectuur en het beheer. De Azure-systeem-omgeving bestaat uit de volgende netwerken:

- Microsoft Azure-productienetwerk (Azure-netwerk)
- Zakelijke Microsoft-netwerk (corpnet)

Afzonderlijke IT-teams zijn verantwoordelijk voor bewerkingen en onderhoud van deze netwerken.

## <a name="azure-architecture"></a>Azure-architectuur
Azure is een cloud computing-platform en infrastructuur voor het bouwen, implementeren, en beheren van toepassingen en services via een netwerk van datacenters. Microsoft beheert deze datacenters. Op basis van het aantal resources die u opgeeft, maakt Azure virtuele machines (VM's) op basis van resource-behoeften. Deze virtuele machines worden uitgevoerd op een Azure-hypervisor, die is ontworpen voor gebruik in de cloud en is niet toegankelijk voor het publiek.

Op elk knooppunt Azure fysieke server is er een hypervisor die rechtstreeks via de hardware uitgevoerd. De hypervisor verdeelt een knooppunt in een variabele aantal Gast-VM's. Elk knooppunt heeft ook een toegangspunt VM, die het host-besturingssysteem wordt uitgevoerd. Windows Firewall is ingeschakeld op elke virtuele machine. Definieert u welke poorten kunnen worden opgevraagd door het configureren van het servicedefinitiebestand. Deze poorten zijn de enige die open en adresseerbare, intern of extern. Alle verkeer en toegang tot de schijf en het netwerk wordt via door de hypervisor en root-besturingssysteem.

Azure virtuele machines worden uitgevoerd op het niveau van de host, een aangepaste en beveiligde versie van de meest recente Windows-Server. Gebruikt een versie van Windows Server met alleen de onderdelen die nodig zijn voor de host virtuele machines van Azure. Dit verbetert de prestaties en vermindert de kwetsbaarheid voor aanvallen. De machinegrenzen van de worden afgedwongen door de hypervisor, die niet afhankelijk van de beveiliging van het besturingssysteem zijn.

### <a name="azure-management-by-fabric-controllers"></a>Azure management van infrastructuurcontrollers

In Azure, zijn virtuele machines die worden uitgevoerd op fysieke servers (blades/knooppunten) gegroepeerd in clusters van ongeveer 1000. De virtuele machines worden afzonderlijk beheerd door een platform met uitgebreide en redundante softwarecomponent, genaamd de infrastructuurcontroller (FC).

Elke FC beheert de levenscyclus van toepassingen die worden uitgevoerd in de cluster en de bepalingen en controleert de status van de hardware onder het besturingselement. Deze wordt uitgevoerd autonome bewerkingen, zoals VM-exemplaren op in orde servers reincarnating wanneer wordt vastgesteld dat een server is mislukt. De FC voert ook een toepassing-management-bewerkingen, zoals het implementeren, bijwerken en schalen van toepassingen.

Het datacenter is onderverdeeld in clusters. Clusters fouten op het niveau van de FC isoleren en te voorkomen dat bepaalde klassen van fouten die betrekking hebben op servers buiten het cluster waarin deze zich voordoen. FCs die dienen een bepaald Azure-cluster zijn gegroepeerd in een FC-cluster.

### <a name="hardware-inventory"></a>Hardware-inventaris

De FC bereidt een inventarisatie maken van Azure-hardware- en apparaten tijdens de bootstrap-configuratie. De nieuwe hardware en netwerkonderdelen invoeren van de Azure-productie-omgeving moeten de bootstrap configuratieprocedure volgen. De FC is verantwoordelijk voor het beheren van de volledige inventaris die worden vermeld in het configuratiebestand datacenter.xml.

### <a name="fc-managed-operating-system-images"></a>Installatiekopieën van besturingssystemen FC-beheerd

Het team van het besturingssysteem biedt afbeeldingen, in de vorm van virtuele Hardeschijven, geïmplementeerd op alle host en de Gast-VM's in de Azure-productie-omgeving. Het team vormt deze basisinstallatiekopieën via een geautomatiseerd buildproces voor offline. De basisinstallatiekopie is een versie van het besturingssysteem waarin de kernel en andere basisonderdelen zijn gewijzigd en geoptimaliseerd voor het ondersteunen van de Azure-omgeving.

Er zijn drie typen van installatiekopieën van besturingssystemen beheerde fabric:

- Host: Een aangepast besturingssysteem die wordt uitgevoerd op host virtuele machines.
- Native: Een systeemeigen besturingssysteem die wordt uitgevoerd op tenants (bijvoorbeeld Azure Storage). Dit besturingssysteem beschikt niet over een hypervisor.
- Gast: Een gast-besturingssysteem die wordt uitgevoerd op de Gast-VM's.

De host en de systeemeigen FC-beheerde besturingssystemen zijn ontworpen voor gebruik in de cloud en zijn niet openbaar toegankelijk is.

#### <a name="host-and-native-operating-systems"></a>Host- en systeemeigen besturingssystemen

Host- en native zijn installatiekopieën van beveiligde besturingssystemen die als host van de fabric-agents en worden uitgevoerd op een rekenknooppunt (wordt uitgevoerd als eerste virtuele machine op het knooppunt) en storage-knooppunten. Het voordeel van het gebruik van geoptimaliseerde basisinstallatiekopieën voor host en native modus is dat de surface area die worden weergegeven door de API's of niet-gebruikte onderdelen vermindert. Deze kunnen hoog risico's voor beveiliging en vergroot het bereik van het besturingssysteem. Verminderde footprint-besturingssystemen bevatten alleen de onderdelen die nodig zijn voor Azure.

#### <a name="guest-operating-system"></a>Gast-besturingssysteem

Azure-interne onderdelen die worden uitgevoerd op het gastbesturingssysteem system virtuele machines hebben geen kans om uit te voeren van Remote Desktop Protocol. Eventuele wijzigingen in de configuratiebasislijn-instellingen moeten doorlopen van de wijziging en release management-proces.

## <a name="azure-datacenters"></a>Azure-datacenters
Het team van Microsoft-Cloud-infrastructuur en bewerkingen (MCIO) beheert de fysieke infrastructuur- en datacenter-installaties voor alle Microsoft online services. MCIO is primair verantwoordelijk is voor het beheren van de fysieke en omgevingsbeveiliging besturingselementen binnen de datacenters, evenals beheren en ondersteunen van buitenste perimeter network apparaten (zoals randrouters en datacenter-routers). MCIO is ook verantwoordelijk voor het instellen van de bare-minimale serverhardware op rekken in het datacenter. Klanten hebben geen directe interactie met Azure.

## <a name="service-management-and-service-teams"></a>Service management en service-teams
Verschillende technische groepen, service-teams, ook wel beheren de ondersteuning van de Azure-service. Elke service-team is verantwoordelijk voor een gebied van ondersteuning voor Azure. Elke service-team moet een technicus 24 x 7 beschikbaar om te onderzoeken en oplossen van fouten in de service maken. Service-teams, standaard geen fysieke toegang tot de hardware uitgevoerd in Azure.

De serviceteams zijn:

- Application-platform
- Azure Active Directory
- Azure Compute
- Azure Net
- Cloud Engineering Services
- ISSD: Beveiliging
- Multifactor Authentication
- SQL Database
- Opslag

## <a name="types-of-users"></a>Typen gebruikers
Werknemers (of ingehuurd personeel) van Microsoft worden beschouwd als interne gebruikers. Alle andere gebruikers worden beschouwd als externe gebruikers. Alle Azure-interne gebruikers hebben de status van de werknemer gecategoriseerd met een gevoeligheidsniveau waarmee de toegang tot klantgegevens (toegang of geen toegang). Gebruiker heeft bevoegdheden die naar Azure (autorisatie machtiging nadat verificatie plaatsgevonden heeft) worden in de volgende tabel beschreven:

| Rol | Intern of extern | Het gevoeligheidsniveau van | Geautoriseerde bevoegdheden en functies die worden uitgevoerd | Toegangstype
| --- | --- | --- | --- | --- |
| Azure-datacenter engineer | Intern | Geen toegang tot klantgegevens | De fysieke beveiliging van de locatie beheren. Patrouilles vanuit het datacenter uitvoeren en bewaken van alle toegangspunten toegepast. Begeleiding van en naar het datacenter van bepaalde niet-gewist personeel die bieden algemene services (zoals eten of opschonen) of op het werk van de IT binnen het datacenter. Routinematige bewaking en het onderhoud van netwerkhardware uitvoeren. Incident werken voor beheer en probleemoplossing uitvoeren met behulp van verschillende hulpprogramma's. Voeren routinematige bewaking en het onderhoud van de fysieke hardware in de datacenters. Toegang tot de omgeving op aanvraag via de eigenschap eigenaren. Kan het uitvoeren van forensische onderzoeken, Incidentrapporten aanmelden en verplichte trainings- en beleid beveiligingsvereisten vereisen. Operationele eigendom en het onderhoud van kritieke beveiligingsprogramma's, zoals scanners en logboekverzameling. | Permanente toegang tot de omgeving. |
| Azure incident sorteren (snelle respons engineers) | Intern | Toegang tot klantgegevens | Beheer van communicatie tussen MCIO, ondersteuning en -engineering-teams. Sorteren platform incidenten, problemen met implementatie en serviceaanvragen. | Just-in-time-toegang tot de omgeving, met beperkte permanente toegang tot niet-systemen. |
| Implementatie van Azure-technici | Intern | Toegang tot klantgegevens | Implementeren en upgraden van de platformonderdelen, software en wijzigingen in de geplande configuratie ter ondersteuning van Azure. | Just-in-time-toegang tot de omgeving, met beperkte permanente toegang tot niet-systemen. |
| Ondersteuning voor Azure-klant onderbreking (tenant) | Intern | Toegang tot klantgegevens | Fouten opsporen en diagnosticeren van platform storingen en fouten voor afzonderlijke computerknooppunten tenants en Azure-accounts. Analyseer fouten. Kritieke fixes station naar het platform of de klant en Verwerf technische verbeteringen voor ondersteuning. | Just-in-time-toegang tot de omgeving, met beperkte permanente toegang tot niet-systemen. |
| Technici van Azure live site (bewaking engineers) en incidenten | Intern | Toegang tot klantgegevens | Opsporen en corrigeren van de gezondheid van platform met behulp van diagnostische hulpprogramma's. Oplossingen voor volumestuurprogramma station, items als gevolg van storingen te herstellen en helpen onderbreking herstelbewerking acties. | Just-in-time-toegang tot de omgeving, met beperkte permanente toegang tot niet-systemen. |
|Azure-klanten | Extern | N/A | N/A | N/A |

Azure maakt gebruik van de unieke id's om te verifiëren van de organisatie-gebruikers en klanten (of processen fungeert namens gebruikers organisatie). Dit geldt voor alle activa en apparaten die deel van de Azure-omgeving uitmaken.

### <a name="azure-internal-authentication"></a>Azure interne verificatie

Communicatie tussen Azure-interne onderdelen zijn beveiligd met TLS-versleuteling. In de meeste gevallen zijn de X.509-certificaten zelfondertekend. Certificaten met verbindingen die kunnen worden benaderd vanaf buiten het netwerk van Azure vormen een uitzondering, zoals certificaten voor de FCs zijn. FCs dat is uitgegeven door een Microsoft-certificaten van de certificeringsinstantie (CA) die wordt ondersteund door een vertrouwde basis-CA-certificaten hebben. Hiermee wordt de openbare sleutels FC eenvoudig worden vernieuwd. Bovendien gebruiken Microsoft-ontwikkelhulpprogramma's FC openbare sleutels. Wanneer ontwikkelaars nieuwe toepassingsinstallatiekopieën verzendt, worden de afbeeldingen versleuteld met de openbare sleutel van een FC om een ingesloten geheimen beveiligen.

### <a name="azure-hardware-device-authentication"></a>Verificatie van de Azure-hardware-apparaten

De FC houdt een set referenties (sleutels en/of wachtwoorden) gebruikt om u te authenticeren bij verschillende hardware-apparaten onder het besturingselement. Microsoft maakt gebruik van een systeem om te voorkomen dat toegang tot deze referenties. Met name is het transport, persistentie en het gebruik van deze referenties ontworpen om te voorkomen dat Azure-ontwikkelaars en beheerders en back-services en medewerkers toegang tot gevoelige, persoonlijke of vertrouwelijke informatie.

Microsoft maakt gebruik van versleuteling op basis van de openbare sleutel van de FC van master identiteit. Dit gebeurt op FC-instellingen en FC herconfiguratie tijden, om over te dragen van de referenties die worden gebruikt voor toegang tot netwerken hardwareapparaten. Wanneer de FC de referenties moet, wordt de FC opgehaald en ontsleutelt deze.

### <a name="network-devices"></a>Netwerkapparaten

Het Azure networking-team configureert u netwerk service-accounts om in te schakelen van een Azure-client om netwerkapparaten te beheren (routers, switches en netwerktaakverdelers) te verifiëren.

## <a name="secure-service-administration"></a>Beveiligen van servicebeheer
Azure-bewerkingen personeel moet secure admin workstations (saw) gebruiken. Klanten kunnen vergelijkbare besturingselementen worden geïmplementeerd met behulp van privileged access workstations. Beheermedewerkers gebruiken met saw, een afzonderlijk toegewezen beheerdersaccount gebruikt dat is gescheiden van het standaardgebruikersaccount van de gebruiker. De ZAG bouwt voort op deze werkwijze door te geven van een betrouwbaar werkstation voor deze gevoelige accounts.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over wat Microsoft doet om te helpen beveiligen van de Azure-infrastructuur:

- [Azure faciliteiten, lokale en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van Azure-infrastructuur](azure-infrastructure-availability.md)
- [Architectuur van Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Azure SQL Database-beveiligingsfuncties](azure-infrastructure-sql.md)
- [-Azure-productiebewerkingen en beheer](azure-infrastructure-operations.md)
- [Azure-infrastructuur bewaken](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Azure-klant-gegevensbeveiliging](azure-protection-of-customer-data.md)
