---
title: Onderdelen van het systeem Azure informatie en grenzen
description: In dit artikel biedt een algemene beschrijving van de architectuur van Microsoft Azure en het beheer.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 8db1dce5fcc56c229d1fdd746bafbd2fae2c9bad
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102336"
---
# <a name="azure-information-system-components-and-boundaries"></a>Onderdelen van het systeem Azure informatie en grenzen
In dit artikel biedt een algemene beschrijving van de architectuur van Microsoft Azure en het beheer. De Azure systeemomgeving bestaat uit de volgende netwerken:

- Microsoft Azure productienetwerk (Azure-netwerk)
- Microsoft Corporate-netwerk (Corpnet-netwerk)

Afzonderlijke IT-teams zijn verantwoordelijk voor bewerkingen en onderhoud van de Azure-netwerk- en CorpNet-netwerken.

## <a name="azure-architecture"></a>Azure-architectuur
Microsoft Azure is een cloud computing platform en infrastructuur voor het bouwen, te implementeren, en het beheer van toepassingen en services via een netwerk van Microsoft beheerde datacenters. Op basis van het aantal bronnen die zijn opgegeven door klanten, maakt Azure VM's op basis van de resource nodig. Deze virtuele machines worden uitgevoerd op een Microsoft Azure-Hypervisor, die is ontworpen voor gebruik in de cloud en is niet toegankelijk voor het publiek.

Op elk knooppunt Azure fysieke server is er een Hypervisor die wordt direct boven de hardware worden uitgevoerd. Hypervisor verdeelt een knooppunt in een variabele aantal virtuele machines (VM's) voor gasten. Elk knooppunt heeft ook een speciale 'Root' virtuele machine, die het Host-besturingssysteem wordt uitgevoerd. Windows Firewall is ingeschakeld op elke virtuele machine. De enige poorten zijn geopend en adresseerbare, zijn intern of extern, poorten expliciet is gedefinieerd in het servicedefinitiebestand geconfigureerd door de klant. Alle verkeer van en toegang tot de schijf en netwerk beslissingen wordt beïnvloed door de Hypervisor en basis-besturingssysteem.

Op de host-laag, virtuele Azure-machines uitgevoerd op een aangepaste en beperkte versie van de meest recente Windows-Server. Microsoft Azure maakt gebruik van een minder uitgebreide versie van Windows Server met alleen de onderdelen die nodig zijn voor virtuele machines host. Dit wordt gedaan om prestaties te verbeteren, en ook om kwetsbaarheid te beperken. Machinegrenzen worden afgedwongen door de Hypervisor, die niet afhankelijk van de beveiliging van het besturingssysteem zijn.

**Azure Management door Infrastructuurcontrollers (FCs)**: In Azure, virtuele machines die worden uitgevoerd op fysieke servers (blades/knooppunten) zijn gegroepeerd in 'clusters' ongeveer 1000. De virtuele machines worden afzonderlijk beheerd door een uitgebreid en redundante platform softwareonderdeel met de naam van de FC.

Elke FC de levenscyclus van toepassingen die worden uitgevoerd in de cluster en de bepalingen beheert en bewaakt de status van de hardware onder het beheer. Zowel autonome bewerkingen, zoals VM-exemplaren op in orde servers reincarnating bij het bepalen dat een server niet wordt uitgevoerd. De FC voert ook Toepassingsbeheer bewerkingen zoals het implementeren, bijwerken en uitbreiden van toepassingen.

Het datacenter is onderverdeeld in clusters. Clusters fouten op het niveau van FC isoleren en te voorkomen dat bepaalde klassen van fouten die invloed hebben op servers buiten het cluster waarin ze plaatsvinden. FCs die een bepaald Azure cluster dienen zijn gegroepeerd in een FC-Cluster.

**Inventaris van Hardware**: een inventaris van Azure hardware- en -apparaten wordt voorbereid tijdens het configuratieproces van de bootstrap en in het configuratiebestand datacenter.xml beschreven. Het configuratieproces van de bootstrap moeten voldoen aan de nieuwe hardware en netwerkonderdelen invoeren van de Azure-productieomgeving. De FC is verantwoordelijk voor het beheren van de gehele voorraad weergegeven in het configuratiebestand datacenter.xml.

**FC-beheerde OS**: de OS-team biedt installatiekopieën van het besturingssysteem in de vorm van virtuele harde schijven (VHD) die zijn geïmplementeerd op alle Host en Gast-VM's in de Azure-productieomgeving. De OS-team vormt deze 'Installatiekopieën Base' via een geautomatiseerde offline buildproces. De Base-installatiekopie is een versie van het besturingssysteem op waarin de kernel en andere kernonderdelen zijn gewijzigd en geoptimaliseerd voor ondersteuning van de Azure-omgeving.

Er zijn drie typen installatiekopieën van het besturingssysteem Fabric worden beheerd:

- Host-OS: hostbesturingssysteem is een aangepaste besturingssysteem die wordt uitgevoerd op virtuele machines Host
- Systeemeigen OS: systeemeigen besturingssysteem dat wordt uitgevoerd op tenants (bijvoorbeeld Azure-opslag) waarop geen elke Hypervisor
- Gastbesturingssysteem – Gastbesturingssysteem dat wordt uitgevoerd op de Gast-VM 's

De Host en de beheerde systeemeigen FC-besturingssystemen zijn ontworpen voor gebruik in de cloud en zijn niet openbaar toegankelijk.

**Host en systeemeigen OS**: Host-OS en systeemeigen OS uitvoeren op een rekenknooppunt (werkt als eerste virtuele machine op het knooppunt) en de configuratie voor opslagknooppunten beperkte installatiekopieën van het besturingssysteem die als host fungeren voor de Fabric-Agents (VA) zijn. De voordelen van het gebruik van geoptimaliseerde Base installatiekopieën van de Host en systeemeigen besturingssysteem dat dit verlaagt het oppervlak door API's of ongebruikte onderdelen die hoge risico's voor beveiliging en het verhogen van de voetafdruk van het besturingssysteem beschikbaar gesteld. Deze besturingssystemen verminderd footprint bevatten alleen de onderdelen die nodig zijn voor Azure. Dit verbetert de prestaties en vermindert de kwetsbaarheid voor aanvallen.

**Gastbesturingssysteem**: Azure interne onderdelen die worden uitgevoerd op de Gast OS-VM's hebben geen mogelijkheid om uit te voeren van Remote Desktop Protocol (RDP) in tegenstelling tot externe klanten. Eventuele wijzigingen aan configuratie-instellingen van basislijn nodig zijn voor de wijziging doorlopen en beheerproces release.

## <a name="azure-datacenters"></a>Azure-datacenters
Het team van Microsoft Cloud-infrastructuur en bewerkingen (MCIO) beheert van Microsoft fysieke infrastructuur en datacenter installaties voor alle Microsoft online services. MCIO is voornamelijk verantwoordelijk voor het beheren van de fysieke en omgevingsbeveiliging besturingselementen binnen de datacentra, evenals beheren en de ondersteunende buitenste rand netwerkapparaten (Randrouters en Datacenter-Routers). MCIO is tevens verantwoordelijk voor het instellen van de bare minimale serverhardware op rekken in het datacenter. Klanten hebben geen directe interactie met Azure.

## <a name="service-management--service-teams"></a>Service management & serviceteams
Ondersteuning van de Azure-service wordt beheerd door een aantal technische groepen bekend als Service-Teams. Elk van de Teams Service is verantwoordelijk voor een gebied van ondersteuning voor Azure. Elk Service-Team moet een technicus 24 x 7 beschikbaar om te onderzoeken en oplossen van fouten in de service maken. Service-Teams, standaard geen fysieke toegang tot de hardware die wordt uitgevoerd in Azure.

Service-teams zijn:

- Application-platform
- Azure Active Directory
- Azure Compute
- Azure Net
- Engineering Cloudservices
- ISSD: beveiliging
- Multifactor Authentication
- SQL Database
- Storage

## <a name="types-of-users"></a>Type gebruikers
Alle Azure interne gebruikers hebben de status van de werknemer ingedeeld met een gevoeligheidsniveau die definieert de toegang tot klantgegevens (of geen toegang). Werknemers (of aannemers) van Microsoft als worden beschouwd als interne gebruikers. Alle andere gebruikers worden beschouwd als externe gebruikers. Gebruiker heeft bevoegdheden die naar Azure (autorisatie machtiging nadat verificatie plaatsgevonden heeft) worden in de volgende tabel beschreven:

| Rol | Interne of externe | Gevoeligheidsniveau | Geautoriseerde bevoegdheden en functies die worden uitgevoerd | Toegangstype
| --- | --- | --- | --- | --- |
| Azure Datacenter engineering | Intern | Geen toegang tot gegevens van de klant | Beheren van de fysieke beveiliging van de lokale; Patrouilles naar en vanuit het datacenter uitvoeren en bewaken van alle toegangspunten; Begeleiding services uitvoeren naar en van het datacenter voor bepaalde niet-gewist personeel die bieden algemene services (eten, reinigen) of IT-werk in het datacenter; Uitvoeren van routinematige bewaking en het onderhoud van de netwerkhardware; Incidentbeheer en schadevergoeding werk met tal van hulpprogramma's; uitvoeren Uitvoeren van routinematige bewaking en het onderhoud van de fysieke hardware in de datacentra; Toegang tot de omgeving op aanvraag van de eigenschap eigenaar van. Compatibele forensische onderzoeken, incident rapport logboekregistratie uitvoeren en vereisen verplichte beveiligingstraining & beleidsvereisten; Operationele eigendom en het onderhoud van kritieke beveiligingsprogramma's zoals scanners en logboekgegevens verzameld. | Permanente toegang tot de omgeving |
| Microsoft Azure Incident selectie (snelle respons Engineers) | Intern | Toegang tot gegevens van de klant | Beheren van de communicatie tussen de Operations-infrastructuur, ondersteuning en technische Azure teams; Selectie platform incidenten, problemen bij de implementatie en serviceaanvragen. | Alleen bij het toegang in uitvoeringstijd tot de omgeving - met beperkte permanente toegang tot niet-klant systemen |
| Microsoft Azure-implementatie Engineers | Intern | Toegang tot gegevens van de klant | Implementatie/upgrades van de platform-onderdelen, software en geplande configuratiewijzigingen ter ondersteuning van Microsoft Azure uitvoeren. | Alleen bij het toegang in uitvoeringstijd tot de omgeving - met beperkte permanente toegang tot niet-klant systemen |
| Microsoft Azure onderbreking klantenondersteuning (Tenant) | Intern | Toegang tot gegevens van de klant | Fouten opsporen en onderzoeken van platform storingen en fouten voor afzonderlijke compute tenants en Microsoft Azure-accounts; Fouten analyseren en kritieke updates station platform/klant, station technische verbeteringen voor ondersteuning. | Alleen bij het toegang in uitvoeringstijd tot de omgeving - met beperkte permanente toegang tot niet-klant systemen |
| (Bewaking Engineers) van Microsoft Azure Live Site Engineers & Incident | Intern | Toegang tot gegevens van de klant | Verantwoordelijk voor het analyseren en platform health beperkende met diagnostische hulpprogramma's; Station van oplossingen voor volume-stuurprogramma's, ten gevolge van storingen items herstellen en storing siteherstelacties helpen. | Alleen bij het toegang in uitvoeringstijd tot de omgeving - met beperkte permanente toegang tot niet-klant systemen |
|Microsoft Azure-klanten | Extern | N/A | N/A | N/A |

Azure maakt gebruik van unieke id's om te verifiëren van de organisatie-gebruikers en klanten (of processen fungeert namens organisatie gebruikers) voor alle activa/apparaten die deel van de Azure-omgeving uitmaken.

**Microsoft Azure interne verificatie**: communicatie tussen Azure interne onderdelen zijn beveiligd met TLS-codering. In de meeste gevallen zijn de X.509-certificaten zelf-ondertekend. Uitzonderingen worden gemaakt voor certificaten met verbindingen dat kunnen worden geopend op buiten het Azure-netwerk, en voor de FCs. FCs certificaten uitgegeven door een Microsoft Certificate van (Certificeringsinstantie) dat wordt ondersteund door een vertrouwde basiscertificeringsinstantie hebben. Hierdoor FC openbare sleutels eenvoudig worden vernieuwd. Bovendien worden FC openbare sleutels gebruikt door Microsoft-hulpprogramma's voor ontwikkelaars, zodat wanneer ontwikkelaars installatiekopieën van het nieuwe aanvraag indienen, ze zijn gecodeerd met een openbare sleutel FC om een ingesloten geheimen beveiligen.

**Microsoft Azure Hardware apparaat Authentication**: de FC houdt een set referenties (sleutels en/of wachtwoorden) gebruikt om u te authenticeren bij verschillende hardware-apparaten onder het beheer. Het systeem dat wordt gebruikt voor het transport van behouden blijven en gebruik van deze referenties is ontworpen om te voorkomen dat Azure ontwikkelaars en beheerders en back-services/medewerkers toegang tot gevoelige, vertrouwelijke of persoonlijke informatie.

Versleuteling op basis van de FC master identiteit openbare sleutel wordt gebruikt op een FC-instellingen en FC-herconfiguratie keer om over te dragen van de referenties gebruikt voor toegang tot hardware netwerkapparaten. Referenties worden opgehaald en worden ontsleuteld door de FC wanneer deze claimgegevens moet.

**Netwerkapparaten**: Network service-accounts zijn geconfigureerd door het team van Azure toegang om in te schakelen van een client voor Microsoft Azure om netwerkapparaten (routers, switches en netwerktaakverdelers) te verifiëren.

## <a name="secure-service-administration"></a>Beveiligen van servicebeheer
Medewerkers van Microsoft Azure-bewerkingen zijn vereist beveiligde beheerwerkstations (zagen; klanten mogelijk vergelijkbare controles te implementeren met behulp van Privileged Access Workstations of poten). De aanpak ZAG is een uitbreiding van de goed vastgestelde aanbevolen werkwijze met afzonderlijke admin en gebruikersaccounts voor beheerders. Deze procedure maakt gebruik van een afzonderlijk toegewezen Administrator-account dat is gescheiden van de standaard gebruikersaccount van de gebruiker. ZAG bouwt voort op deze account scheiding praktijk door te geven een betrouwbare werkstation voor die gevoelige accounts.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Microsoft biedt voor het beveiligen van de Azure-infrastructuur:

- [Azure opslagruimten, ruimten en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van de Azure-infrastructuur](azure-infrastructure-availability.md)
- [Architectuur van de Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Beveiligingsfuncties van Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Azure productie-uitvoering en beheer](azure-infrastructure-operations.md)
- [Bewaking van Azure-infrastructuur](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Beveiliging van gegevens van de klant in Azure](azure-protection-of-customer-data.md)
