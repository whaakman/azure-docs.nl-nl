---
title: Beveiliging van gegevens van de klant in Azure
description: In dit artikel komen hoe Azure beschermt gegevens van de klant.
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
ms.openlocfilehash: 9a3b00e39f78f65b05b7d730447440d481979539
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102323"
---
# <a name="protection-of-customer-data-in-azure"></a>Beveiliging van gegevens van de klant in Azure   
Standaard is toegang tot gegevens door Microsoft operations en ondersteunend personeel van de klant geweigerd. Als toegang tot gegevens van de klant is verleend, leidinggevende goedkeuring is vereist en vervolgens toegang goed wordt beheerd en geregistreerd. De vereisten voor toegangsbeheer worden ingesteld door de volgende beveiligingsbeleid voor Microsoft Azure:

- Geen toegang tot gegevens van de klant standaard
- Er zijn geen accounts gebruiker of beheerder van de klant virtuele machines
- Verleen de minimale bevoegdheden die zijn vereist voor de taak voltooid. controleren en bijhouden toegangsaanvragen

Microsoft Azure ondersteunend personeel zijn unieke zakelijk AD-accounts toegewezen door Microsoft. Microsoft Azure, is afhankelijk van Microsoft zakelijk Active Directory, beheerd door MSIT toegang tot belangrijke informatie-systemen besturen. Multi-factor authentication-server is vereist en alleen toegang tot van beveiligde consoles.

Alle toegangspogingen worden bewaakt en via een elementaire reeks rapporten kunnen worden weergegeven.

## <a name="data-protection"></a>Gegevensbeveiliging
Azure biedt klanten sterk gegevensbeveiliging – zowel standaard als als opties voor de klant.

**Gegevensscheiding** -Azure is een multitenant-service, wat betekent dat de implementaties meerdere klanten en de virtuele machines zijn opgeslagen op dezelfde fysieke hardware. Azure maakt gebruik van logische isolatie van elke klant gegevens van de gegevens van andere scheiden. Scheiding biedt de schaal en economische voordelen van multitenant-services en te voorkomen dat klanten toegang krijgen tot gegevens van elkaar.

**Bescherming van gegevens in rust** -klanten zijn verantwoordelijk voor het garanderen dat de gegevens die zijn opgeslagen in Azure worden gecodeerd in overeenstemming met de standaarden. Azure biedt een breed scala aan mogelijkheden voor versleuteling, waardoor klanten de flexibiliteit om te kiezen welke oplossing het beste voldoet aan hun behoeften. Azure Sleutelkluis kunnen klanten eenvoudig controle houdt over de sleutels die door cloudtoepassingen en -services worden gebruikt om gegevens te versleutelen. Azure Disk Encryption kunnen klanten voor het versleutelen van virtuele machines. Azure Storage-Service: versleuteling, maakt het mogelijk alle gegevens die zijn opgenomen in de storage-account van de klant te versleutelen.

**Gegevensbeveiliging in transit** -klanten versleuteling voor verkeer tussen hun eigen virtuele machines en eindgebruikers kunnen inschakelen. Azure beschermt gegevens die worden verzonden naar of van buiten-onderdelen en -gegevens in doorvoer intern, zoals tussen de twee virtuele netwerken. Azure maakt gebruik van de industriestandaard TLS Transport Layer Security () 1.2 of hoger protocol met 2048-bits RSA/SHA256 versleutelingssleutels, als aanbevolen door CESG/NCSC voor het versleutelen van communicatie tussen:

- de klant en de cloud
- intern tussen Azure en datacenters

**Versleuteling** -versleuteling van gegevens in de opslag en bij verzending door klanten kan worden geïmplementeerd als een aanbevolen procedure voor het garanderen van de vertrouwelijkheid en integriteit van gegevens. Het is eenvoudig voor klanten om hun Azure-cloud-services om SSL te gebruiken om communicatie te beveiligen vanaf het Internet te configureren en zelfs tussen hun Azure gehoste virtuele machines.

**Gegevensredundantie** -Microsoft zorgt voor de gegevens worden beveiligd als er een cyberattack of fysieke schade aan een datacenter. Klanten kunnen ervoor kiezen voor:

- in het land-opslag voor naleving of latentie-overwegingen
- opslag voor beveiliging of noodgeval hersteldoeleinden out van land

Gegevens kunnen worden gerepliceerd binnen een geselecteerde geografische gebied voor de redundantie van maar niet buiten het verzonden. Klanten hebben meerdere opties voor het repliceren van gegevens, inclusief het aantal exemplaren en aantal en de locatie van replicatie datacenters.

Wanneer u uw storage-account maakt, moet u een van de volgende replicatieopties selecteren:

- Lokaal redundante opslag (LRS). Lokaal redundante opslag onderhoudt drie kopieën van uw gegevens. LRS wordt binnen één faciliteit in één regio driemaal gerepliceerd. LRS beschermt u uw gegevens tegen normale hardwarefouten, maar niet tegen het uitvallen van één faciliteit.
- Zone-redundante opslag (ZRS). Zone-redundante opslag onderhoudt drie kopieën van uw gegevens. ZRS is driemaal gerepliceerd in twee of drie faciliteiten bieden hogere duurzaamheid dan LRS. Replicatie vindt plaats binnen één regio of tussen twee regio's. ZRS houdt uw gegevens duurzaam binnen één regio.
- Geografisch redundante opslag (GRS). Wanneer u een account maakt, is geografisch redundante opslag standaard ingeschakeld. GRS onderhoudt zes kopieën van uw gegevens. Met GRS worden uw gegevens driemaal gerepliceerd binnen de primaire regio. Uw gegevens ook in een secundaire regio op honderden mijl weg van de primaire regio, biedt het hoogste niveau van duurzaamheid driemaal gerepliceerd. Als er een storing optreedt in de primaire regio, wordt er door Azure Storage een failover uitgevoerd naar de secundaire regio. GRS houdt uw gegevens duurzaam binnen twee afzonderlijke regio's.

**Gegevensvernietiging** : wanneer klanten gegevens verwijderen of laat de eigenschap Azure, Microsoft strenge standaarden volgt voor het overschrijven van opslagbronnen voordat opnieuw gebruiken, evenals fysieke vernietiging van buiten gebruik gestelde hardware. Microsoft voert een volledig verwijderen van gegevens op verzoek van de klant en contract beëindiging.

## <a name="customer-data-ownership"></a>Klant-Gegevenseigendom
Microsoft niet controleren, goedkeuren of bewaken van toepassingen die klanten in Azure implementeren. Microsoft biedt bovendien niet weet wat soort gegevens klanten ervoor kiezen om op te slaan in Azure. Microsoft maakt geen aanspraak op Gegevenseigendom via de klantgegevens die is ingevoerd in Azure.

## <a name="records-management"></a>Beheer van records
Azure tot stand heeft gebracht vereisten voor de bewaarperiode voor back-endgegevens interne records. Klanten zijn verantwoordelijk voor het identificeren van de bewaarplicht van hun eigen gegevens. Records die zijn opgeslagen in Azure, is de klant verantwoordelijk voor het ophalen van hun gegevens en de inhoud buiten Azure bewaren voor een klant opgegeven bewaarperiode.

Azure biedt de klant de mogelijkheid voor het exporteren van gegevens en rapporten van het product controleren. De uitvoer worden lokaal opgeslagen voor het bewaren van gegevens voor het bewaren van een klant gedefinieerde periode.

## <a name="electronic-discovery-e-discovery"></a>Elektronische detectie (e-discovery)
Azure-klanten zijn verantwoordelijk voor het voldoen aan vereisten voor het e-discovery in het gebruik van Azure-services. Als een Azure-klant hun klantgegevens behouden moet, kunnen ze exporteren en de gegevens lokaal opslaan. Bovendien kunnen worden aangevraagd uitvoer van de bijbehorende gegevens van Azure Customer Support-afdeling. Naast de mogelijkheid klanten om hun gegevens te exporteren, voert Azure uitgebreide logboekregistratie en controle het intern.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Microsoft biedt voor het beveiligen van de Azure-infrastructuur:

- [Azure opslagruimten, ruimten en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van de Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van de Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Beveiligingsfuncties van Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Azure productie-uitvoering en beheer](azure-infrastructure-operations.md)
- [Bewaking van Azure-infrastructuur](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
