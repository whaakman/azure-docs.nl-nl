---
title: Overzicht van multitenant-ondersteuning voor VMware VM replicatie naar Azure (CSP) met Azure Site Recovery | Microsoft Docs
description: Biedt een overzicht van Azure Site Recovery-ondersteuning voor de tenant abonnementen in een omgeving met meerdere tenants via het CSP-programma.
services: site-recovery
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: manayar
ms.openlocfilehash: 285086964365339291e9027a7fe8e5ee0083e13b
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="overview-of-multi-tenant-support-for-vmware-replication-to-azure-with-csp"></a>Overzicht van multitenant-ondersteuning voor VMware-replicatie naar Azure met een CSP

[Azure Site Recovery](site-recovery-overview.md) biedt ondersteuning voor meerdere tenants omgevingen voor abonnementen van de tenant. Het ondersteunt ook multi-tenancymodus voor tenant-abonnementen die worden gemaakt en beheerd via het programma Cloud Solution Provider (CSP).

Dit artikel bevat een overzicht van de implementatie en beheer van meerdere tenants VMware naar Azure replicatie.

## <a name="multi-tenant-environments"></a>Multitenant-omgevingen

Er zijn drie primaire modellen voor meerdere tenants:

* **Hosting-Services Provider (HSP) gedeeld**: de partner is eigenaar van de fysieke infrastructuur, en maakt gebruik van gedeelde resources (vCenter, datacenters, fysieke opslag, enzovoort) voor het hosten van meerdere tenant-VM's op dezelfde infrastructuur. Herstel na noodgevallen management als een beheerde service krijgt u de partner of herstel na noodgevallen als een oplossing voor self-service kan eigenaar van de tenant.

* **Hosting-Services Provider-specifieke**: de partner eigenaar is van de fysieke infrastructuur, maar toegewezen resources (meerdere Vcenter, fysieke datastores, enzovoort) worden gebruikt voor het hosten van elke tenant-VM's op een aparte infrastructuur. De partner management als een beheerde service herstel na noodgevallen kunt opgeven of de tenant kan de eigenaar ervan als een oplossing voor selfservice.

* **Beheerde Provider (MSP-Services)**: de klant eigenaar van de fysieke infrastructuur die als host fungeert voor de virtuele machines en de partner biedt inschakelen voor herstel na noodgevallen en beheer.

## <a name="shared-hosting-services-provider-hsp"></a>Gedeeld-hosting serviceprovider (HSP)

De andere twee scenario's zijn subsets van het scenario gedeeld-hosting en ze gebruiken dezelfde principes. De verschillen worden beschreven aan het einde van de richtlijnen gedeeld-hosting.

Een eerste vereiste in een multitenant-scenario is dat tenants geïsoleerd worden. Een tenant moet niet kunnen zien wat een andere tenant is gehost. Deze vereiste is niet zo belangrijk als het zich in een omgeving met selfservice, waar dit kan essentieel zijn in een omgeving beheerd door een partner. In dit artikel wordt ervan uitgegaan dat de isolatie van tenants vereist is.

De architectuur wordt weergegeven in het volgende diagram.

![Gedeelde HSP met één vCenter](./media/vmware-azure-multi-tenant-overview/shared-hosting-scenario.png)  

**Gedeeld-hosting met een vCenter-server**

In het diagram heeft elke klant een afzonderlijke beheerserver. Deze configuratie tenant toegang beperkt tot tenantspecifieke VM's en tenantisolatie maakt. VMware VM replicatie maakt gebruik van de configuratieserver virtuele machines detecteren en agents installeren. De principes van de toepassing voor multitenant-omgevingen, met de toevoeging van VM-detectie met behulp van toegangsbeheer vCenter beperken.

De gegevens isolatie vereiste betekent dat alle infrastructuur gevoelige gegevens (zoals toegangsreferenties) niet openbaar te maken voor tenants blijft. Daarom is het raadzaam dat alle onderdelen van de beheerserver onder de exclusieve controle van de partner blijven. De management server-onderdelen zijn:

* Configuratieserver
* Processerver
* Hoofddoelserver

Een afzonderlijke schaalvergroting processerver is ook onder het beheer van de partner.

## <a name="configuration-server-accounts"></a>Configuratie-serveraccounts

Elke configuratieserver in het scenario voor meerdere tenants maakt gebruik van twee accounts:

- **vCenter toegangsaccount**: dit account wordt gebruikt voor het detecteren van tenant-VM's. Hieraan toegangsmachtigingen vCenter toegewezen. Om te voorkomen van toegang lekken, is het raadzaam dat partners in het configuratiehulpprogramma van deze referenties zelf invoert.

- **Virtuele machine toegangsaccount**: dit account wordt gebruikt voor het installeren van de Mobility-service-agent op de tenant-VM's, met een automatische push. Dit is meestal een domeinaccount die een tenant kan bieden aan een partner of een account met de partner kan rechtstreeks worden beheerd. Als een tenant niet rechtstreeks de details met de partner delen wilt, kunnen ze Voer de referenties met de tijdelijke toegang en de configuratieserver. Of met hulp bij de partner kunnen ze de Mobility-service-agent handmatig installeren.

## <a name="vcenter-account-requirements"></a>vereisten voor vCenter-account

De configuratieserver configureren met een account met een speciale rol is toegewezen.

- De roltoewijzing moet worden toegepast op de vCenter-toegangsaccount voor elk object vCenter en worden niet doorgegeven aan de onderliggende objecten. Deze configuratie zorgt er tenantisolatie, omdat toegang doorgeven tot onbedoeld toegang tot andere objecten leiden kan.

    ![De doorgeven aan de optie voor onderliggende objecten](./media/vmware-azure-multi-tenant-overview/assign-permissions-without-propagation.png)

- Alternatieve methode is het toewijzen van het gebruikersaccount en de rol op het datacenter-object, en deze kan doorgeven aan de onderliggende objecten. Geef het account een **geen toegang** rol voor ieder object (zoals virtuele machines die deel uitmaken van andere tenants) die niet toegankelijk voor een bepaalde tenant moet worden. Deze configuratie is omslachtig. Het wordt per ongeluk tot standaardtoegangsbeheer, omdat elke nieuwe onderliggend object is ook automatisch toegang die overgenomen van de bovenliggende. Daarom raden we aan dat u de eerste benadering.

### <a name="create-a-vcenter-account"></a>Een vCenter-account maken

1. Een nieuwe rol maken door het klonen van de vooraf gedefinieerde *alleen-lezen* rol, en geef hieraan een geschikte naam (zoals Azure_Site_Recovery, zoals in dit voorbeeld).
2. De volgende machtigingen toewijzen aan deze rol:

    * **Datastore**: toewijzen van ruimte, bladeren gegevensopslag, bestandsbewerkingen op laag niveau, bestand verwijderen, bestanden van de virtuele machine bijwerken
    * **Netwerk**: netwerk toewijzen
    * **Resource**: toewijzen virtuele machine te migreren virtuele machine, uitgeschakeld resourcegroep migreren die zijn ingeschakeld op virtuele machine
    * **Taken**: taak, Update-taak maken
    * **VM - configuratie**: alle
    - **VM - interactie** > beantwoorden van vraag, apparaatverbinding, configureer CD's, diskettes configureren, uitschakelen, inschakelen, VMware-hulpprogramma's installeren
    - **VM - inventarisatie** > maken op basis van bestaande, het maken van nieuwe, het registreren, registratie
    - **VM - inrichting** > downloaden van de virtuele machine toestaan, toestaan virtuele machine bestanden uploaden
    - **VM - momentopname management** > momentopnamen verwijderen

        ![Het dialoogvenster rol bewerken](./media/vmware-azure-multi-tenant-overview/edit-role-permissions.png)

3. Toegangsniveaus voor de vCenter-account (gebruikt in de huurder configuratieserver) als volgt voor verschillende objecten toewijzen:

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

### <a name="failover-only"></a>Alleen failover
Om te beperken NOODHERSTELBEWERKINGEN tot alleen failover (dat wil zeggen, zonder failback mogelijkheden), gebruikt u de vorige procedure met de volgende uitzonderingen:

- In plaats van het toewijzen van de *Azure_Site_Recovery* de toegangsaccount vCenter-rol toewijzen alleen een *alleen-lezen* rol aan dit account. Deze machtigingenset kunt VM-replicatie en failover en failback niet mogelijk.
- Alles in het voorgaande proces blijft echter is. Om te garanderen isolatie van tenants en VM-detectie te beperken, wordt elke machtiging nog steeds op het niveau van alleen worden toegewezen en niet doorgegeven aan de onderliggende objecten.

### <a name="deploy-resources-to-the-tenant-subscription"></a>Resources aan het tenantabonnement implementeren

1. Een resourcegroep maken in de Azure portal en vervolgens implementeert u een Recovery Services-kluis per normale proces.
2. Download de registratiesleutel voor de kluis.
3. Registreer de CS voor de tenant met behulp van de sleutel van de registratie van de kluis.
4. Voer de referenties voor de twee toegangsaccounts, de account voor toegang tot de vCenter-server en de account voor toegang tot de virtuele machine.

    ![Accounts van server configuration Manager](./media/vmware-azure-multi-tenant-overview/config-server-account-display.png)

### <a name="register-servers-in-the-vault"></a>Servers in de kluis registreren

1. In de Azure-portal in de kluis die u eerder hebt gemaakt, registreert u de vCenter-server en de configuratieserver met de vCenter-account dat u hebt gemaakt.
2. Voltooiing van het 'Infrastructuur voorbereiden'-proces voor siteherstel per normale proces.
3. De virtuele machines zijn nu gereed om te worden gerepliceerd. Zorg dat alleen virtuele machines van de tenant worden weergegeven **repliceren** > **virtuele machines selecteren**.

## <a name="dedicated-hosting-solution"></a>Toegewezen hosting oplossing

Zoals u in het volgende diagram, is het architectuur verschil in een speciale hosting oplossing dat elke tenant-infrastructuur is ingesteld voor alleen die tenant.

![architectuur-gedeeld hsp](./media/vmware-azure-multi-tenant-overview/dedicated-hosting-scenario.png)  
**Speciale hosting scenario met meerdere Vcenter**

## <a name="managed-service-solution"></a>Beheerde service-oplossing

Zoals u in het volgende diagram, is de architectuur verschil in een beheerde service-oplossing dat elke tenant infrastructuur ook fysiek gescheiden is van andere tenants infrastructuur. Dit scenario bestaat meestal wanneer de tenant is eigenaar van de infrastructuur en wil dat de oplossingsprovider van een voor het beheren van herstel na noodgevallen.

![architectuur-gedeeld hsp](./media/vmware-azure-multi-tenant-overview/managed-service-scenario.png)  
**Service-scenario met meerdere Vcenter beheerd**

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie](site-recovery-role-based-linked-access-control.md) over op rollen gebaseerde toegangsbeheer in Site Recovery.
- Meer informatie over hoe [herstel na noodgevallen van virtuele VMware-machines naar Azure instellen](vmware-azure-tutorial.md).
- Meer informatie over [multi-tenancymodus met CSP voor virtuele VMWare-machines](vmware-azure-multi-tenant-csp-disaster-recovery.md).
