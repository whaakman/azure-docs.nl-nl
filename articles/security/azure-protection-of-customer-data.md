---
title: Beveiliging van klantgegevens in Azure
description: In dit artikel komen hoe klantgegevens worden beveiligd met Azure.
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
ms.openlocfilehash: 49615dcb2f077d2e1d8b93a4bb900b435e4c87bf
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56104486"
---
# <a name="azure-customer-data-protection"></a>Azure-klant-gegevensbeveiliging   
Toegang tot klantgegevens door Microsoft uitvoerend en ondersteunend personeel is standaard niet toegestaan. Als de toegang tot gegevens van de klant is verleend, leiderschap goedkeuring is vereist en wordt vervolgens toegang zorgvuldig beheerd en geregistreerd. De access-control-vereisten zijn gemaakt door het volgende beleid van de Azure-beveiliging:

- Geen toegang tot klantgegevens, standaard.
- Er is geen gebruiker of beheerder accounts op virtuele machines (VM's) van klanten.
- Verlenen van minimale bevoegdheden die vereist zijn om uit te voeren taak. controleren en toegangsaanvragen vastleggen.

Azure-ondersteuningspersoneel zijn unieke zakelijke Active Directory-accounts toegewezen door Microsoft. Azure is afhankelijk van Microsoft zakelijke Active Directory, beheerd door Microsoft Information Technology (MSIT), voor het beheren van toegang tot belangrijke informatiesystemen. Meervoudige verificatie is vereist en toegang te krijgen alleen uit beveiligde consoles.

Alle toegangspogingen worden bewaakt en kunnen worden weergegeven via een set met rapporten.

## <a name="data-protection"></a>Gegevensbeveiliging
Azure biedt klanten met sterke gegevensbeveiliging, zowel standaard als als opties voor de klant.

**Gegevensscheiding**: Azure is een service met meerdere tenants, wat dat klanten meerdere implementaties betekent en virtuele machines zijn opgeslagen op dezelfde fysieke hardware. Azure wordt logische isolatie gebruikt gegevens van de gegevens van andere gebruikers van elke klant te scheiden. Scheiding is wordt voorkomen dat klanten toegang krijgen tot andere gegevens de schaal en de economische voordelen van multitenant-services.

**De beveiliging van gegevens in rust**: Zijn klanten verantwoordelijk om ervoor te zorgen dat gegevens die zijn opgeslagen in Azure is versleuteld in overeenstemming met de standaarden. Azure biedt een breed scala aan versleutelingsmogelijkheden voor, zodat klanten de flexibiliteit om te kiezen welke oplossing het beste aan hun eisen voldoen. Azure Key Vault helpt klanten eenvoudig beheer van sleutels die door cloudtoepassingen en -services worden gebruikt om gegevens te versleutelen. Azure Disk Encryption kunnen klanten voor het versleutelen van virtuele machines. Azure Storage-Serviceversleuteling maakt het mogelijk is voor het versleutelen van alle gegevens die in de storage-account van de klant wordt geplaatst.

**De beveiliging van gegevens in transit**: Klanten kunnen versleuteling voor verkeer tussen hun eigen virtuele machines en eindgebruikers kunnen inschakelen. Azure beschermt gegevens die onderweg zijn naar of van buiten-onderdelen en gegevens in doorvoer intern, zoals tussen de twee virtuele netwerken. Azure maakt gebruik van de industriestandaard-Transport Layer Security (TLS)-protocol 1.2 of hoger met 2048-bits RSA/SHA256-versleutelingssleutels, zoals aanbevolen door CESG/NCSC, voor het versleutelen van communicatie tussen:

- De klant en de cloud.
- Intern tussen Azure-systemen en datacenters.

**Versleuteling**: Versleuteling van gegevens in de opslag en doorvoer kan worden geïmplementeerd door klanten als een best practice om ervoor te zorgen vertrouwelijkheid en integriteit van gegevens. Het is eenvoudig voor klanten om hun Azure-cloud-services om SSL te gebruiken om communicatie te beveiligen vanaf het internet en zelfs tussen hun wordt gehost op Azure-VM's te configureren.

**Gegevensredundantie**: Microsoft zorgt ervoor dat de gegevens zijn beveiligd als er een cyberattack of fysieke schade aan een datacenter. Klanten kunnen kiezen voor:

- Opslag in het land voor naleving of latentie-overwegingen.
- Opslag buiten het land voor beveiligings- of disaster recovery-toepassing.

Gegevens kunnen worden gerepliceerd binnen een geselecteerde geografische gebied voor gegevensredundantie, maar buiten deze kan niet worden verzonden. Klanten hebben meerdere opties voor het repliceren van gegevens, inclusief het aantal exemplaren en het aantal en de locatie van de replicatie-datacenters.

Wanneer u uw storage-account maakt, een van de volgende replicatieopties selecteren:

- **Lokaal redundante opslag (LRS)**: Lokaal redundante opslag onderhoudt drie kopieën van uw gegevens. LRS wordt binnen één faciliteit in één regio driemaal gerepliceerd. LRS beschermt uw gegevens tegen normale hardwarefouten, maar niet tegen het uitvallen van één faciliteit.
- **Zone-redundante opslag (ZRS)**: Zone-redundante opslag onderhoudt drie kopieën van uw gegevens. ZRS is drie keer gerepliceerd tussen twee of drie faciliteiten voor hogere duurzaamheid dan LRS. Replicatie vindt plaats binnen één regio of tussen twee regio's. ZRS zorgt ervoor dat uw gegevens duurzaam binnen één regio is.
- **Geografisch redundante opslag (GRS)**: Wanneer u een account maakt, is geografisch redundante opslag standaard ingeschakeld. GRS onderhoudt zes kopieën van uw gegevens. Met GRS, worden uw gegevens drie keer gerepliceerd binnen de primaire regio. Uw gegevens worden ook drie keer gerepliceerd in een secundaire regio die honderden afstand van de primaire regio, biedt het hoogste niveau van duurzaamheid. In het geval van een storing optreedt in de primaire regio wordt Azure Storage overgenomen door naar de secundaire regio. GRS zorgt ervoor dat uw gegevens duurzaam, in twee afzonderlijke regio's zijn.

**Gegevensvernietiging**: Wanneer klanten gegevens verwijderen of Azure laat, volgt Microsoft strikte standaarden bij het overschrijven van opslagbronnen voordat hun hergebruik, evenals de fysieke vernietiging van het uit bedrijf genomen hardware. Microsoft voert een volledige verwijdering van gegevens op verzoek van de klant en contract-beëindiging.

## <a name="customer-data-ownership"></a>Klant-Gegevenseigendom
Microsoft niet controleren, goedkeuren of bewaken van toepassingen die klanten naar Azure implementeren. Microsoft biedt bovendien niet weet welke soorten gegevens klanten ervoor kiezen om op te slaan in Azure. Microsoft maakt geen aanspraak op Gegevenseigendom via de klantgegevens die ingevoerd in Azure.

## <a name="records-management"></a>Beheer van records
Azure is tot stand gebracht vereisten voor de interne records-bewaarperiode voor back-end-gegevens. Klanten zijn verantwoordelijk voor het identificeren van de bewaarplicht van hun eigen gegevens. Voor de records die zijn opgeslagen in Azure, zijn klanten verantwoordelijk voor het ophalen van hun gegevens en die de inhoud van buiten Azure bewaren gedurende een door de klant opgegeven bewaarperiode.

Azure kan klanten gegevens exporteren en controleren van rapporten uit het product. De uitvoer worden lokaal opgeslagen als u wilt behouden van de gegevens voor het bewaren van een klant gedefinieerde periode.

## <a name="electronic-discovery-e-discovery"></a>Elektronische detectie (e-discovery)
Azure-klanten zijn verantwoordelijk voor het voldoen aan de vereisten voor het e-discovery in hun gebruik van Azure-services. Als Azure-klanten hun klantgegevens behouden moeten, kunnen ze exporteren en de gegevens lokaal opslaan. Klanten kunnen bovendien uitvoer van hun gegevens vragen van de afdeling voor klantenservice van Azure. Naast de mogelijkheid klanten om hun gegevens te exporteren, voert Azure uitgebreide logboekregistratie en bewaking intern.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over wat Microsoft doet voor het beveiligen van de Azure-infrastructuur:

- [Azure faciliteiten, lokale en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Azure SQL Database-beveiligingsfuncties](azure-infrastructure-sql.md)
- [-Azure-productiebewerkingen en beheer](azure-infrastructure-operations.md)
- [Azure-infrastructuur bewaken](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
