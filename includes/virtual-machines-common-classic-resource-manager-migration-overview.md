---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: jpconnock
ms.service: virtual-machines
ms.topic: include
ms.date: 05/18/2018
ms.author: jeconnoc
ms.custom: include file
ms.openlocfilehash: 629cdf3907f45419ecfa5fce59430a163767c8fb
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36943263"
---
# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Migratie van IaaS resources van klassieke in Azure Resource Manager-platform ondersteund
In dit artikel beschrijft het migreren van infrastructuur als een dienst (IaaS) resources van het klassieke naar Resource Manager-implementatiemodel en details van bronnen van de twee implementatiemodellen die naast elkaar worden gebruikt in uw abonnement met behulp van virtueel netwerk verbinden site-naar-site gateways. U kunt meer lezen over [Azure Resource Manager-functies en voordelen](../articles/azure-resource-manager/resource-group-overview.md). 

## <a name="goal-for-migration"></a>Doel voor de migratie
Resource Manager kunt implementeren van complexe toepassingen via sjablonen, configureert u virtuele machines met behulp van de VM-extensies en opgenomen met het beheer van toegang en labels. Azure Resource Manager bevat schaalbare parallelle implementatie voor virtuele machines in beschikbaarheidssets. Het nieuwe implementatiemodel biedt ook levenscyclusbeheer van compute, netwerk en opslag onafhankelijk. Er is ten slotte een focus over het inschakelen van beveiliging standaard met het afdwingen van virtuele machines in een virtueel netwerk.

Bijna alle functies van het klassieke implementatiemodel worden ondersteund voor compute, netwerk en opslag in Azure Resource Manager. Als u wilt profiteren van de nieuwe mogelijkheden in Azure Resource Manager, kunt u bestaande implementaties van het klassieke implementatiemodel migreren.

## <a name="supported-resources-for-migration"></a>Ondersteunde bronnen voor migratie
Deze klassieke IaaS-bronnen worden ondersteund tijdens de migratie

* Virtual Machines
* Beschikbaarheidssets
* Cloud Services
* Opslagaccounts
* Virtuele netwerken
* VPN Gateways
* Express Route-Gateways _(in hetzelfde abonnement als virtueel netwerk alleen)_
* Netwerkbeveiligingsgroepen
* Routetabellen
* Gereserveerde IP-adressen

## <a name="supported-scopes-of-migration"></a>Ondersteunde scopes van de migratie
Er zijn vier verschillende manieren om de migratie van compute, network en storage-resources te voltooien:

* [Migratie van virtuele machines (niet in een virtueel netwerk)](#migration-of-virtual-machines-not-in-a-virtual-network)
* [Migratie van virtuele machines (in een virtueel netwerk)](#migration-of-virtual-machines-in-a-virtual-network)
* [Migratie van de storage-accounts](#migration-of-storage-accounts)
* [Migratie van niet-gekoppelde resources](#migration-of-unattached-resources)

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migratie van virtuele machines (niet in een virtueel netwerk)
Beveiliging wordt in het Resource Manager-implementatiemodel standaard afgedwongen voor uw toepassingen. Alle virtuele machines moeten in een virtueel netwerk in het Resource Manager-model. De Azure-platform opnieuw wordt opgestart (`Stop`, `Deallocate`, en `Start`) de virtuele machines als onderdeel van de migratie. U hebt twee opties voor de virtuele netwerken die de virtuele Machines worden gemigreerd naar:

* U kunt het platform voor het maken van een nieuw virtueel netwerk en de virtuele machine migreren naar het nieuwe virtuele netwerk aanvragen.
* U kunt de virtuele machine migreren naar een bestaand virtueel netwerk in Resource Manager.

> [!NOTE]
> In dit bereik migratie kunnen zowel de operations management vlak en de gegevens vlak-bewerkingen niet worden toegestaan voor een bepaalde periode tijdens de migratie.
>

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migratie van virtuele machines (in een virtueel netwerk)
Alleen de metagegevens is voor de meeste configuraties van virtuele machine migreren tussen het klassieke en het Resource Manager-implementatiemodel. De onderliggende virtuele machines worden uitgevoerd op dezelfde hardware in hetzelfde netwerk, en met dezelfde opslag. De operations management vlak kunnen niet worden toegestaan voor een bepaalde periode tijdens de migratie. Het vlak van de gegevens blijft echter werken. Dat wil zeggen, de toepassingen die worden uitgevoerd op virtuele machines (klassiek) komen niet in rekening gebracht uitvaltijd tijdens de migratie.

De volgende configuraties zijn momenteel niet ondersteund. Als ondersteuning wordt toegevoegd in de toekomst een aantal virtuele machines in deze configuratie zich kan brengen van uitvaltijd (Ga toewijzing via stoppen en opnieuw opstarten van VM-bewerkingen).

* U hebt meer dan één beschikbaarheidsset in een enkel cloudservice.
* U hebt een of meer beschikbaarheidssets en virtuele machines die zich niet in een beschikbaarheidsset in een enkel cloudservice.

> [!NOTE]
> In dit bereik migratie zijn het beheervlak niet toegestaan voor een bepaalde periode tijdens de migratie. Voor bepaalde configuraties zoals eerder beschreven, gegevens vlak uitval optreedt.
>

### <a name="migration-of-storage-accounts"></a>Migratie van de storage-accounts
U kunt zodat naadloze migratie Resource Manager virtuele machines implementeren in een klassieke storage-account. Met deze mogelijkheid berekenings- en netwerkbronnen kunnen en onafhankelijk van de storage-accounts moeten worden gemigreerd. Als u via uw virtuele Machines en virtuele netwerken migreren, moet u voor het migreren van via uw storage-accounts om het migratieproces te voltooien.

Als uw storage-account beschikt niet over een gekoppelde schijven of virtuele Machines, gegevens en alleen blobs, bestanden, tabellen en wachtrijen kan de migratie naar Azure Resource Manager worden gedaan als een zelfstandige migratie zonder afhankelijkheden.

> [!NOTE]
> Het Resource Manager-implementatiemodel beschikt niet over het concept van klassieke installatiekopieën en schijven. Wanneer het opslagaccount is gemigreerd, klassieke installatiekopieën en schijven zijn niet zichtbaar in de stack van Resource Manager, maar de back-ups maken VHD's blijven in de storage-account.
>

### <a name="migration-of-unattached-resources"></a>Migratie van niet-gekoppelde resources
Storage-Accounts zonder gekoppelde schijven of virtuele Machines gegevens kunnen onafhankelijk worden gemigreerd.

Netwerkbeveiligingsgroepen, routetabellen & gereserveerde IP-adressen die niet zijn gekoppeld aan een virtuele Machines en virtuele netwerken kunnen ook afzonderlijk worden gemigreerd.

<br>

## <a name="unsupported-features-and-configurations"></a>Niet-ondersteunde functies en configuraties
Sommige functies en configuraties worden momenteel niet ondersteund; de volgende secties beschrijven onze aanbevelingen omheen.

### <a name="unsupported-features"></a>Niet-ondersteunde functies
De volgende functies worden momenteel niet ondersteund. U kunt deze instellingen desgewenst verwijderen, de virtuele machines migreren en de instellingen in het Resource Manager-implementatiemodel opnieuw inschakelen.

| Resourceprovider | Functie | Aanbeveling |
| --- | --- | --- |
| Computing | Niet-gekoppelde virtuele machine-schijven. | De VHD-blobs achter deze schijven worden ophalen gemigreerd wanneer het Opslagaccount wordt gemigreerd |
| Computing | Installatiekopieën van virtuele machines. | De VHD-blobs achter deze schijven worden ophalen gemigreerd wanneer het Opslagaccount wordt gemigreerd |
| Netwerk | Eindpunt-ACL's. | Verwijder de eindpunt-ACL's en migratie nogmaals uitvoeren. |
| Netwerk | Application Gateway | Verwijder de toepassingsgateway vóór de migratie en vervolgens opnieuw de toepassingsgateway maken zodra de migratie is voltooid. |
| Netwerk | Virtuele netwerken met VNet-Peering. | Migreer het virtuele netwerk naar Resource Manager en klik vervolgens op hetzelfde niveau. Meer informatie over [VNet-Peering](../articles/virtual-network/virtual-network-peering-overview.md). |

### <a name="unsupported-configurations"></a>Niet-ondersteunde configuraties
De volgende configuraties zijn momenteel niet ondersteund.

| Service | Configuratie | Aanbeveling |
| --- | --- | --- |
| Resource Manager |Rollen gebaseerd toegangsbeheer (RBAC) voor klassieke resources |Omdat de URI van de resources is gewijzigd na migratie, is het raadzaam dat u van plan de RBAC beleidsupdates die moeten worden bent nadat de migratie. |
| Computing |Meerdere subnetten die zijn gekoppeld aan een virtuele machine |Werk de subnetconfiguratie om te verwijzen naar slechts één subnet. Mogelijk moet u een secundaire NIC (die verwijst naar een ander subnet) verwijderen van de virtuele machine en koppel deze opnieuw nadat de migratie is voltooid. |
| Computing |Virtuele machines die deel uitmaken van een virtueel netwerk, maar geen een expliciete subnet toegewezen |U kunt de VM desgewenst verwijderen. |
| Computing |Virtuele machines met waarschuwingen, beleid voor automatisch schalen |De migratie doorloopt en deze instellingen worden verwijderd. Het is raadzaam dat u uw omgeving evalueren voordat u de migratie. U kunt ook de instellingen voor waarschuwingen configureren nadat de migratie is voltooid. |
| Computing |XML-VM-extensies (BGInfo-1.*, Visual Studio Debugger Web Deploy en foutopsporing op afstand) |Dit wordt niet ondersteund. Het wordt aanbevolen dat u deze uitbreidingen van de virtuele machine om door te gaan migratie verwijderen of wordt deze automatisch verwijderd tijdens het migratieproces. |
| Computing |Diagnostische gegevens over opstarten met Premium-opslag |De functie diagnostische gegevens over opstarten voor de virtuele machines uitschakelen voordat u doorgaat met de migratie. U kunt diagnostische gegevens over opstarten in de Resource Manager-stack weer inschakelen nadat de migratie voltooid is. Bovendien moeten blobs die worden gebruikt voor schermafbeelding en seriële logboeken worden verwijderd zodat u niet langer in rekening voor de blobs gebracht worden. |
| Computing | Cloudservices met web/werkrollen | Dit wordt momenteel niet ondersteund. |
| Computing | Cloudservices met meer dan één beschikbaarheid instellen of meerdere beschikbaarheidssets. |Dit wordt momenteel niet ondersteund. Verplaats de virtuele Machines naar dezelfde beschikbaarheidsset voordat u migreert. |
| Computing | Virtuele machine met de extensie Azure Security Center | Azure Security Center installeert automatisch extensies op uw virtuele Machines om te controleren van de beveiliging en waarschuwingen worden gegeven. Deze uitbreidingen ophalen meestal automatisch geïnstalleerd als het Azure Security Center-beleid is ingeschakeld op het abonnement. Uitschakelen om te migreren van de virtuele Machines, het beveiligingsbeleid voor center op het abonnement, zodat het Beveiligingscentrum bewaking van de uitbreiding van de virtuele Machines wordt verwijderd. |
| Computing | Virtuele machine met de extensie voor back-up of momentopname | Deze uitbreidingen worden geïnstalleerd op een virtuele Machine geconfigureerd met de Azure Backup-service. Tijdens de migratie van deze virtuele machines wordt niet ondersteund, volgt u de instructies [hier](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq#vault) te houden van back-ups die zijn uitgevoerd vóór de migratie.  |
| Netwerk |Virtuele netwerken die virtuele machines en web/werkrollen bevatten |Dit wordt momenteel niet ondersteund. Verplaats de rollen Web/Worker met hun eigen virtuele netwerk voordat u migreert. Zodra de klassiek virtueel netwerk wordt gemigreerd, wordt het gemigreerde virtuele netwerk voor Azure Resource Manager kunt brengen met het klassieke virtuele netwerk te bereiken vergelijkbare configuratie als voorheen.|
| Netwerk | Klassieke Express Route-circuits |Dit wordt momenteel niet ondersteund. Deze circuits moeten in Azure Resource Manager worden gemigreerd vóór de migratie van IaaS. Zie voor meer informatie, [verplaatsen van ExpressRoute-circuits van het klassieke naar het Resource Manager-implementatiemodel](../articles/expressroute/expressroute-move.md).|
| Azure App Service |Virtuele netwerken met App Service-omgevingen |Dit wordt momenteel niet ondersteund. |
| Azure HDInsight |Virtuele netwerken die HDInsight services bevatten |Dit wordt momenteel niet ondersteund. |
| Microsoft Dynamics levenscyclus van Services |Virtuele netwerken met virtuele machines die worden beheerd door Dynamics levenscyclus van Services |Dit wordt momenteel niet ondersteund. |
| Azure AD Domain Services |Virtuele netwerken die Azure AD Domain services bevatten |Dit wordt momenteel niet ondersteund. |
| Azure RemoteApp |Virtuele netwerken met Azure RemoteApp-implementaties |Dit wordt momenteel niet ondersteund. |
| Azure API Management |Virtuele netwerken met Azure API Management-implementaties |Dit wordt momenteel niet ondersteund. Wijzig het VNET van de API Management-implementatie een geen uitvaltijd-bewerking is voor het migreren van het VNET IaaS. |
