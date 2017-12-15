---
title: Ondersteuning voor meerdere tenants voor VMware VM replicatie naar Azure (CSP programma) | Microsoft Docs
description: Hierin wordt beschreven hoe u Azure Site Recovery implementeert in een omgeving met meerdere tenants voor replicatie, failovers en herstel van on-premises VMware virtuele machines (VM's) naar Azure via het programma CSP indelen met behulp van de Azure-portal
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: manayar
ms.openlocfilehash: 273efe0bdef421d753ea51e01060d48351cbe6fc
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="multi-tenant-support-in-azure-site-recovery-for-replicating-vmware-virtual-machines-to-azure-through-csp"></a>Multitenant-ondersteuning in Azure Site Recovery voor virtuele VMware-machines repliceren naar Azure via CSP

Azure Site Recovery biedt ondersteuning voor meerdere tenants omgevingen voor abonnementen van de tenant. Het ondersteunt ook multi-tenancymodus voor tenant-abonnementen die worden gemaakt en beheerd via het programma Cloud Solution Provider (CSP). In dit artikel beschrijft de richtlijn voor de implementatie en beheer van meerdere tenants VMware naar Azure-scenario's. Zie voor meer informatie over het maken en tenant-abonnementen beheren [multi-tenancymodus met CSP beheren](site-recovery-manage-multi-tenancy-with-csp.md) .

In deze richtlijnen wordt getekend sterk van de bestaande documentatie voor de virtuele VMware-machines repliceren naar Azure. Zie voor meer informatie [repliceren VMware-machines in Azure met Site Recovery](site-recovery-vmware-to-azure.md).

## <a name="multi-tenant-environments"></a>Multitenant-omgevingen
Er zijn drie primaire modellen voor meerdere tenants:

* **Hosting-Services Provider (HSP) gedeeld**: de partner is eigenaar van de fysieke infrastructuur, en maakt gebruik van gedeelde resources (vCenter, datacenters, fysieke opslag, enzovoort) voor het hosten van meerdere tenants virtuele machines op dezelfde infrastructuur. Herstel na noodgevallen management als een beheerde service krijgt u de partner of herstel na noodgevallen als een oplossing voor self-service kan eigenaar van de tenant.

* **Hosting-Services Provider-specifieke**: de partner eigenaar is van de fysieke infrastructuur, maar toegewezen resources (meerdere Vcenter, fysieke datastores, enzovoort) worden gebruikt voor het hosten van elke tenant-VM's op een aparte infrastructuur. De partner management als een beheerde service herstel na noodgevallen kunt opgeven of de tenant kan de eigenaar ervan als een oplossing voor selfservice.

* **Beheerde Provider (MSP-Services)**: de klant eigenaar van de fysieke infrastructuur die als host fungeert voor de virtuele machines en de partner biedt inschakelen voor herstel na noodgevallen en beheer.

## <a name="shared-hosting-multi-tenant-guidance"></a>Gedeeld-hosting multitenant richtlijnen
Deze sectie worden het scenario gedeeld-hosting in detail behandeld. De andere twee scenario's zijn subsets van het scenario gedeeld-hosting en ze gebruiken dezelfde principes. De verschillen worden beschreven aan het einde van de richtlijnen gedeeld-hosting.

Een eerste vereiste in een multitenant-scenario is het isoleren van de verschillende tenants. Een tenant moet niet kunnen zien wat een andere tenant is gehost. Deze vereiste is niet zo belangrijk als het zich in een omgeving met selfservice, waar dit kan essentieel zijn in een omgeving beheerd door een partner. In deze richtlijnen wordt ervan uitgegaan dat de isolatie van tenants vereist is.

De architectuur is opgenomen in het volgende diagram:

![Gedeelde HSP met één vCenter](./media/site-recovery-multi-tenant-support-vmware-using-csp/shared-hosting-scenario.png)  
**Gedeeld-hosting scenario met één vCenter**

Zoals in het voorgaande diagram kunt zien, heeft elke klant een afzonderlijke beheerserver. Deze configuratie tenant toegang beperkt tot tenantspecifieke VM's en schakelt tenantisolatie. Een VMware-replicatie voor virtuele machines scenario maakt gebruik van de configuratieserver voor het beheren van accounts voor virtuele machines detecteren en agents installeren. De principes van de toepassing voor multitenant-omgevingen, met de toevoeging van VM-detectie via vCenter toegangsbeheer beperken.

De vereiste isolatie van gegevens vereist dat alle infrastructuur gevoelige gegevens (zoals toegangsreferenties) niet openbaar te maken voor tenants blijft. Daarom is het raadzaam dat alle onderdelen van de beheerserver onder de exclusieve controle van de partner blijven. De management server-onderdelen zijn:
* Configuratieserver (CS)
* De processerver (PS)
* Hoofddoelserver (MT)

Een scale-out PS is ook onder het beheer van de partner.

### <a name="every-cs-in-the-multi-tenant-scenario-uses-two-accounts"></a>Elke CS in het scenario voor meerdere tenants gebruikt twee accounts

- **vCenter toegangsaccount**: dit account gebruiken voor het detecteren van tenant-VM's. Hieraan toegangsmachtigingen vCenter toegewezen (zoals beschreven in de volgende sectie). Om te voorkomen van toegang onbedoeld lekken, is het raadzaam dat partners in het configuratiehulpprogramma van deze referenties zelf invoert.

- **Virtuele machine toegangsaccount**: dit account gebruiken voor het installeren van de mobility-agent op de tenant-VM's via een automatische push. Dit is meestal een domeinaccount die een tenant kan leveren aan een partner of dat de partner kan rechtstreeks worden beheerd. Als een tenant niet rechtstreeks de details met de partner delen wilt, kunnen ze toegestaan voert u de referenties met de tijdelijke toegang aan de CS, of met hulp bij de partner handmatig mobility agents installeren.

### <a name="requirements-for-a-vcenter-access-account"></a>Vereisten voor een vCenter-toegangsaccount

Zoals vermeld in de vorige sectie, moet u de CS configureren met een account met een speciale rol is toegewezen. De roltoewijzing moet worden toegepast op de vCenter-toegangsaccount voor elk object vCenter en niet doorgegeven aan de onderliggende objecten. Deze configuratie zorgt er tenantisolatie, omdat toegang doorgeven tot onbedoeld toegang tot andere objecten leiden kan.

![De doorgeven aan de optie voor onderliggende objecten](./media/site-recovery-multi-tenant-support-vmware-using-csp/assign-permissions-without-propagation.png)

Het alternatief is voor het toewijzen van het gebruikersaccount en de rol op het object van het datacenter en deze kan doorgeven aan de onderliggende objecten. Geef het account een *geen toegang* rol voor ieder object (zoals andere tenants VM's) die niet toegankelijk voor een bepaalde tenant worden moet. Deze configuratie is omslachtig en beschrijft de onbedoeld toegangscontroles, omdat elke nieuwe onderliggend object is ook automatisch toegang die overgenomen van het bovenliggende item. Daarom raden we aan dat u de eerste benadering.

De vCenter-accounttoegang procedure is als volgt:

1. Een nieuwe rol maken door het klonen van de vooraf gedefinieerde *alleen-lezen* rol, en geef hieraan een geschikte naam (zoals Azure_Site_Recovery, zoals in dit voorbeeld).

2. De volgende machtigingen toewijzen aan deze rol:

    * **Datastore**: toewijzen van ruimte, bladeren gegevensopslag, bestandsbewerkingen op laag niveau, bestand verwijderen, bestanden van de virtuele machine bijwerken

    * **Netwerk**: netwerk toewijzen

    * **Resource**: toewijzen virtuele machine te migreren virtuele machine, uitgeschakeld resourcegroep migreren die zijn ingeschakeld op virtuele machine

    * **Taken**: taak, Update-taak maken

    * **Virtuele machine**:
        * Configuratie > alle
        * Interactie > beantwoorden van vraag, apparaatverbinding, configureer CD's, diskettes configureren, uitschakelen, inschakelen, VMware-hulpprogramma's installeren
        * Inventaris > maken op basis van bestaande, het maken van nieuwe, het registreren, registratie
        * Inrichting > downloaden van de virtuele machine toestaan, toestaan virtuele machine bestanden uploaden
        * Momentopname maken van beheer > momentopnamen verwijderen

    ![Het dialoogvenster rol bewerken](./media/site-recovery-multi-tenant-support-vmware-using-csp/edit-role-permissions.png)

3. Als volgt toegangsniveaus toewijzen aan de vCenter-account (gebruikt in de tenant CS) voor verschillende objecten:

>| Object | Rol | Opmerkingen |
>| --- | --- | --- |
>| vCenter | Alleen-lezen | Vereist alleen vCenter om toegang te verlenen voor het beheren van verschillende objecten. U kunt deze machtiging verwijderen als het account wordt nooit worden opgegeven voor een tenant of voor alle beheerbewerkingen op de vCenter-gebruikt. |
>| Datacenter | Azure_Site_Recovery |  |
>| Host en hostcluster | Azure_Site_Recovery | Opnieuw zorgt u ervoor dat de toegang op het objectniveau van het is zodat alleen toegankelijk hosts tenant-VM's voor failover- en na de failback hebben. |
>| Gegevensopslag en datastore-cluster | Azure_Site_Recovery | Hetzelfde als de voorgaande. |
>| Netwerk | Azure_Site_Recovery |  |
>| Beheerserver | Azure_Site_Recovery | Toegang tot alle onderdelen (CS PS en MT) buiten de machine CS bevat. |
>| Tenant-VM 's | Azure_Site_Recovery | Zorgt ervoor dat een nieuwe tenant-VM's van een bepaalde tenant ook deze toegang krijgen, of ze zich niet kunnen worden gedetecteerd via de Azure portal. |

De vCenter-toegang is nu voltooid. Deze stap wordt voldaan aan de vereiste minimale machtigingen failbackbewerkingen worden voltooid. U kunt ook deze machtigingen gebruiken met uw bestaande beleid. Uw bestaande machtigingen zijn ingesteld op rolmachtigingen uit stap 2, gedetailleerde eerder bevatten alleen wijzigen.

Herstel na noodgevallen om bewerkingen te beperken tot de failoverstatus (dat wil zeggen, zonder failback mogelijkheden), volgt u de voorgaande procedure, met een uitzondering: in plaats van het toewijzen van de *Azure_Site_Recovery* rol met de vCenter- toegang tot account, alleen wijst een *alleen-lezen* rol aan dit account. Deze machtigingenset kunt VM-replicatie en failover en failback niet mogelijk. Alles in het voorgaande proces blijft echter is. Om te garanderen isolatie van tenants en VM-detectie te beperken, wordt elke machtiging nog steeds op het niveau van alleen worden toegewezen en niet doorgegeven aan de onderliggende objecten.

## <a name="other-multi-tenant-environments"></a>Andere omgevingen met meerdere tenants

De vorige sectie beschreven hoe u een multitenant-omgeving voor een gedeelde hosting oplossing instelt. De andere twee belangrijke oplossingen zijn speciale hosting en beheerde service. De architectuur voor deze oplossingen wordt in de volgende secties beschreven.

### <a name="dedicated-hosting-solution"></a>Toegewezen hosting oplossing

Zoals u in het volgende diagram, is het architectuur verschil in een speciale hosting oplossing dat elke tenant-infrastructuur is ingesteld voor alleen die tenant. Omdat tenants via afzonderlijke Vcenter geïsoleerd zijn, wordt de hostingprovider moet nog steeds de stappen CSP opgegeven voor het hosten van gedeelde maar niet hoeft te hoeven maken over de isolatie van tenants. CSP setup blijft ongewijzigd.

![architectuur-gedeeld hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/dedicated-hosting-scenario.png)  
**Speciale hosting scenario met meerdere Vcenter**

### <a name="managed-service-solution"></a>Beheerde service-oplossing

Zoals u in het volgende diagram, is de architectuur verschil in een beheerde service-oplossing dat elke tenant infrastructuur ook fysiek gescheiden is van andere tenants infrastructuur. Dit scenario bestaat meestal wanneer de tenant is eigenaar van de infrastructuur en wil dat de oplossingsprovider van een voor het beheren van herstel na noodgevallen. Nogmaals, omdat tenants fysiek geïsoleerd via een andere infrastructuur, de partner moet de CSP stappen opgegeven voor het hosten van gedeelde maar niet hoeft te hoeven maken over de isolatie van tenants. CSP inrichting blijft ongewijzigd.

![architectuur-gedeeld hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/managed-service-scenario.png)  
**Service-scenario met meerdere Vcenter beheerd**

## <a name="next-steps"></a>Volgende stappen
[Meer informatie](site-recovery-role-based-linked-access-control.md) over op rollen gebaseerde toegangsbeheer voor het beheren van Azure Site Recovery-implementaties.

[Multi-tenancymodus met CSP beheren](site-recovery-manage-multi-tenancy-with-csp.md)
