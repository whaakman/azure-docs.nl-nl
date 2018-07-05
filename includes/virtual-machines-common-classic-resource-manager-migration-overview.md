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
ms.openlocfilehash: d1a6ff8dbd17d2792709a1ce065bcf793154e585
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37780669"
---
# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Platform ondersteunde migratie van IaaS-resources van klassiek naar Azure Resource Manager
In dit artikel wordt beschreven hoe u voor het migreren van infrastructuur als een service (IaaS)-resources van het klassieke naar Resource Manager-implementatiemodel en details van hoe u resources van de twee implementatiemodellen die naast elkaar worden gebruikt in uw abonnement met behulp van virtueel netwerk verbinden site-naar-site gateways. U kunt meer lezen over [Azure Resource Manager-functies en voordelen](../articles/azure-resource-manager/resource-group-overview.md). 

## <a name="goal-for-migration"></a>Doel voor migratie
Resource Manager kunt implementeren van complexe toepassingen via sjablonen, virtuele machines configureren met behulp van VM-extensies en bevat toegangsbeheer en tagging. Azure Resource Manager bevat schaalbare, parallelle implementatie voor virtuele machines in beschikbaarheidssets. Het nieuwe implementatiemodel biedt ook beheer van de levenscyclus van compute, netwerk en opslag onafhankelijk van elkaar. Er is ten slotte een focus over het inschakelen van beveiliging standaard bij het afdwingen van virtuele machines in een virtueel netwerk.

Bijna alle functies van het klassieke implementatiemodel worden ondersteund voor berekening, netwerk en opslag in Azure Resource Manager. Als u wilt profiteren van de nieuwe mogelijkheden in Azure Resource Manager, kunt u bestaande implementaties migreren uit het klassieke implementatiemodel.

## <a name="supported-resources-for-migration"></a>Ondersteunde resources voor migratie
Deze klassieke IaaS-resources worden ondersteund tijdens de migratie

* Virtuele machines
* Beschikbaarheidssets
* Cloudservices met virtuele Machines
* Opslagaccounts
* Virtuele netwerken
* VPN Gateways
* Express Route-Gateways _(in hetzelfde abonnement als het Virtueelnetwerk alleen)_
* Netwerkbeveiligingsgroepen
* Routetabellen
* Gereserveerde IP-adressen

## <a name="supported-scopes-of-migration"></a>Ondersteunde scopes van de migratie
Er zijn vier verschillende manieren om de migratie van Reken-, netwerk- en opslagresources te voltooien:

* [Migratie van virtuele machines (niet in een virtueel netwerk)](#migration-of-virtual-machines-not-in-a-virtual-network)
* [Migratie van virtuele machines (in een virtueel netwerk)](#migration-of-virtual-machines-in-a-virtual-network)
* [Migratie van storage-accounts](#migration-of-storage-accounts)
* [Migratie van niet-gekoppelde resources](#migration-of-unattached-resources)

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migratie van virtuele machines (niet in een virtueel netwerk)
In het Resource Manager-implementatiemodel, beveiliging standaard afgedwongen voor uw toepassingen. Alle virtuele machines moeten zich in een virtueel netwerk in het Resource Manager-model. De Azure-platform wordt opnieuw opgestart (`Stop`, `Deallocate`, en `Start`) de virtuele machines als onderdeel van de migratie. U hebt twee opties voor de virtuele netwerken die de virtuele Machines worden gemigreerd naar:

* U kunt het platform voor het maken van een nieuw virtueel netwerk en de virtuele machine migreren naar het nieuwe virtuele netwerk aanvragen.
* U kunt de virtuele machine migreren naar een bestaand virtueel netwerk in Resource Manager.

> [!NOTE]
> In dit migratiebereik van zowel de beheerlaag bewerkingen en de data plane-bewerkingen mogelijk niet toegestaan voor een bepaalde periode tijdens de migratie.
>

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migratie van virtuele machines (in een virtueel netwerk)
Voor de meeste configuraties van virtuele machine, wordt alleen de metagegevens migreren tussen het klassieke en Resource Manager-implementatiemodel. De onderliggende virtuele machines worden uitgevoerd op dezelfde hardware, in hetzelfde netwerk, en met dezelfde opslag. De bewerkingen van het beheervlak mogelijk niet toegestaan voor een bepaalde periode tijdens de migratie. De gegevenslaag blijft echter om te werken. Dat wil zeggen, de toepassingen uitvoeren op virtuele machines (klassiek) genereren geen uitvaltijd tijdens de migratie.

De volgende configuraties worden momenteel niet ondersteund. Als er is ondersteuning toegevoegd in de toekomst enkele VM's in deze configuratie kan kosten voor downtime (Ga via stoppen, toewijzing ongedaan maken en bewerkingen van de virtuele machine opnieuw opstarten).

* U hebt meer dan één beschikbaarheidsset in een enkele cloudservice.
* U hebt een of meer beschikbaarheidssets en virtuele machines die zich niet in een beschikbaarheidsset in een enkele cloudservice.

> [!NOTE]
> In dit migratiebereik zijn het beheervlak niet toegestaan voor een bepaalde periode tijdens de migratie. Voor bepaalde configuraties zoals eerder beschreven, data plane-uitval optreedt.
>

### <a name="migration-of-storage-accounts"></a>Migratie van storage-accounts
Als u wilt toestaan dat naadloze migratie, kunt u resourcemanager-VM's implementeren in een klassiek opslagaccount. Met deze functie reken- en netwerkbronnen kunnen en onafhankelijk van storage-accounts moeten worden gemigreerd. Nadat u via uw virtuele Machines en Virtueelnetwerk migreert, moet u migreren via uw storage-accounts om het migratieproces te voltooien.

Als uw storage-account beschikt niet over alle gekoppelde schijven of de gegevens op virtuele Machines en alleen blobs, bestanden, tabellen en wachtrijen heeft kan de migratie naar Azure Resource Manager worden gedaan als een zelfstandige migratie zonder afhankelijkheden.

> [!NOTE]
> De Resource Manager-implementatiemodel beschikt niet over het concept van klassieke installatiekopieën en schijven. Wanneer het opslagaccount is gemigreerd, klassieke installatiekopieën en schijven, niet zichtbaar in de Resource Manager-stack, maar de back-ups VHD's blijven in de storage-account.
>

### <a name="migration-of-unattached-resources"></a>Migratie van niet-gekoppelde resources
Storage-Accounts met geen bijbehorende schijven of virtuele Machines gegevens kunnen onafhankelijk worden gemigreerd.

Network Security Groups, routetabellen en gereserveerde IP-adressen die niet zijn gekoppeld aan alle virtuele Machines en virtuele netwerken kunnen ook onafhankelijk van elkaar worden gemigreerd.

<br>

## <a name="unsupported-features-and-configurations"></a>Niet-ondersteunde functies en configuraties
Sommige functies en configuraties worden momenteel niet ondersteund; de volgende secties beschrijven onze aanbevelingen.

### <a name="unsupported-features"></a>Niet-ondersteunde functies
De volgende functies worden momenteel niet ondersteund. U kunt ervoor kiezen om deze instellingen verwijderen, het migreren van de virtuele machines en de instellingen in het Resource Manager-implementatiemodel vervolgens opnieuw inschakelen.

| Resourceprovider | Functie | Aanbeveling |
| --- | --- | --- |
| Compute | Niet-gekoppelde virtuele-machineschijven. | De VHD-blobs achter deze schijven worden gemigreerd als het Opslagaccount dat wordt gemigreerd |
| Compute | Installatiekopieën van virtuele machines. | De VHD-blobs achter deze schijven worden gemigreerd als het Opslagaccount dat wordt gemigreerd |
| Netwerk | Eindpunt-ACL's. | Verwijderen van de eindpunt-ACL's en probeer opnieuw de migratie. |
| Netwerk | Application Gateway | De toepassingsgateway verwijderen voordat u begint met migratie en maakt de toepassingsgateway opnieuw zodra de migratie is voltooid. |
| Netwerk | Virtuele netwerken met behulp van VNet-Peering. | Virtuele netwerken migreren naar Resource Manager en klik vervolgens op hetzelfde niveau. Meer informatie over [VNet-Peering](../articles/virtual-network/virtual-network-peering-overview.md). |

### <a name="unsupported-configurations"></a>Niet-ondersteunde configuraties
De volgende configuraties worden momenteel niet ondersteund.

| Service | Configuratie | Aanbeveling |
| --- | --- | --- |
| Resource Manager |Rollen gebaseerd toegangsbeheer (RBAC) voor klassieke resources |Omdat de URI van de resources is gewijzigd na migratie, is het raadzaam dat u van plan de RBAC-beleid-updates die moeten bent worden uitgevoerd na de migratie. |
| Compute |Meerdere subnetten die zijn gekoppeld aan een virtuele machine |Werk de subnetconfiguratie om te verwijzen naar slechts één subnet. Mogelijk moet u een secundaire NIC's (die verwijst naar een ander subnet) verwijderen uit de VM en koppel deze opnieuw nadat de migratie is voltooid. |
| Compute |Virtuele machines die deel uitmaken van een virtueel netwerk maar niet een expliciete subnet toegewezen |Eventueel kunt u de virtuele machine verwijderen. |
| Compute |Virtuele machines met waarschuwingen, beleid voor automatisch schalen |De migratie hanteert en deze instellingen worden verwijderd. Het is raadzaam dat u uw omgeving evalueren voordat u de migratie. U kunt ook de instellingen voor meldingen configureren nadat de migratie is voltooid. |
| Compute |XML-VM-extensies (BGInfo 1.*, Visual Studio-foutopsporing, Web Deploy en foutopsporing op afstand) |Dit wordt niet ondersteund. Het wordt aanbevolen dat u deze uitbreidingen van de virtuele machine om door te gaan migratie verwijderen of wordt deze automatisch verwijderd tijdens het migratieproces. |
| Compute |Diagnostische gegevens over opstarten met Premium storage |Functie voor diagnostische gegevens over opstarten voor de virtuele machines uitschakelen voordat u doorgaat met de migratie. U kunt diagnostische gegevens over opstarten in het Resource Manager-stack opnieuw inschakelen nadat de migratie voltooid is. Bovendien moeten blobs die worden gebruikt voor schermafbeelding en seriële logboeken worden verwijderd, zodat u niet meer in rekening voor deze blobs gebracht worden. |
| Compute | Cloudservices met web-/ werkrollen | Dit wordt momenteel niet ondersteund. |
| Compute | Cloudservices met meer dan één beschikbaarheid instellen of meerdere beschikbaarheidssets. |Dit wordt momenteel niet ondersteund. Verplaats de virtuele Machines op dezelfde beschikbaarheidsset voordat u migreert. |
| Compute | Virtuele machine met de extensie Azure Security Center | Azure Security Center worden extensies automatisch geïnstalleerd op uw virtuele Machines om te controleren van de beveiliging en alarmen. Deze uitbreidingen krijgen meestal automatisch geïnstalleerd als het Azure Security Center-beleid is ingeschakeld op het abonnement. Uitschakelen voor het migreren van de virtuele Machines, de security center-beleid voor het abonnement, waarbij het Security Center monitoring-extensie van de virtuele Machines wordt verwijderd. |
| Compute | Virtuele machine met de back-up of momentopname-extensie | Deze uitbreidingen worden geïnstalleerd op een virtuele Machine geconfigureerd met de Azure Backup-service. Tijdens de migratie van deze virtuele machines wordt niet ondersteund, volgt u de instructies [hier](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq#vault) te houden van back-ups die zijn uitgevoerd vóór de migratie.  |
| Netwerk |Virtuele netwerken die virtuele machines en web-/ werkrollen bevatten. |Dit wordt momenteel niet ondersteund. Verplaats de Web/Worker-rollen met hun eigen Virtueelnetwerk voordat u migreert. Zodra het klassieke Virtueelnetwerk wordt gemigreerd, kan het gemigreerde Azure Resource Manager-netwerk worden gekoppeld aan het klassieke Virtueelnetwerk voor een vergelijkbare configuratie als voorheen.|
| Netwerk | Klassieke Expressroute-circuits |Dit wordt momenteel niet ondersteund. Deze circuits moeten naar Azure Resource Manager worden gemigreerd voordat u begint met IaaS-migratie. Zie voor meer informatie, [verplaatsen van ExpressRoute-circuits van het klassieke naar het Resource Manager-implementatiemodel](../articles/expressroute/expressroute-move.md).|
| Azure App Service |Virtuele netwerken met App Service-omgevingen |Dit wordt momenteel niet ondersteund. |
| Azure HDInsight |Virtuele netwerken die HDInsight-services bevatten |Dit wordt momenteel niet ondersteund. |
| Microsoft Dynamics Lifecycle Services |Virtuele netwerken die virtuele machines die worden beheerd door Dynamics Lifecycle Services bevatten |Dit wordt momenteel niet ondersteund. |
| Azure AD Domain Services |Virtuele netwerken met Azure AD Domain services |Dit wordt momenteel niet ondersteund. |
| Azure RemoteApp |Virtuele netwerken met Azure RemoteApp-implementaties |Dit wordt momenteel niet ondersteund. |
| Azure API Management |Virtuele netwerken met Azure API Management-implementaties |Dit wordt momenteel niet ondersteund. Voor het migreren van het VNET IaaS, wijzigt u het VNET van de API Management-implementatie, dit een bewerking waarbij er geen uitvaltijd is. |
