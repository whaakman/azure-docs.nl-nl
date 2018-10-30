---
title: Overzicht van ondersteuning voor meerdere tenants voor herstel na noodgevallen voor VMware-VM naar Azure (CSP) met behulp van Azure Site Recovery | Microsoft Docs
description: Biedt een overzicht van Azure Site Recovery-ondersteuning voor noodherstel VMWare naar Azure in een omgeving met meerdere tenants (CSP)-programma.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: mayg
ms.openlocfilehash: 89e731d6c255092b087f0615bad49185c7181f1f
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50210752"
---
# <a name="overview-of-multi-tenant-support-for-vmware-fisaster-recovery-to-azure-with-csp"></a>Overzicht van ondersteuning voor meerdere tenants fisaster voor herstel voor VMware naar Azure met CSP

[Azure Site Recovery](site-recovery-overview.md) biedt ondersteuning voor omgevingen met meerdere tenants voor tenant-abonnementen. Het ondersteunt ook meerdere tenants voor tenant-abonnementen die worden gemaakt en beheerd via het programma Microsoft Cloud Solution Provider (CSP).

Dit artikel bevat een overzicht van de implementatie en beheer van meerdere tenants replicatie van VMware naar Azure.

## <a name="multi-tenant-environments"></a>Omgevingen met meerdere tenants

Er zijn drie belangrijke modellen voor meerdere tenants:

* **Gedeeld die als host fungeert Services serviceprovider (HSP)**: de partner is eigenaar van de fysieke infrastructuur, en maakt gebruik van gedeelde resources (vCenter, datacenters, fysieke opslag, enzovoort) voor het hosten van meerdere tenant-VM's op dezelfde infrastructuur. De partner herstel na noodgevallen management als een beheerde service kunt opgeven of de tenant eigenaar kan herstel na noodgevallen als een oplossing voor de selfservice.

* **Services-Provider die als host fungeert toegewezen**: de partner is eigenaar van de fysieke infrastructuur, maar worden toegewezen resources (meerdere vCenters, fysieke gegevensopslag, enzovoort) gebruikt voor het hosten van elke tenant-VM's op een aparte infrastructuur. De partner herstel na noodgevallen management als een beheerde service kunt opgeven of de tenant eigenaar kan het als een oplossing voor de selfservice.

* **Beheerde Services Provider (MSP)**: de klant is eigenaar van de fysieke infrastructuur die als host fungeert voor de virtuele machines en de partner biedt inschakelen voor herstel na noodgevallen en beheer.

## <a name="shared-hosting-services-provider-hsp"></a>Gedeeld-hosting services serviceprovider (HSP)

De andere twee scenario's zijn subsets van het scenario gedeeld-hosting, en ze gebruiken dezelfde principes. De verschillen worden aan het einde van de gedeelde-hosting richtlijnen beschreven.

De algemene vereiste in een scenario met meerdere tenants is dat tenants geïsoleerd worden moeten. Een tenant moet niet kunnen zien wat een andere tenant is gehost. Deze vereiste is niet zo belangrijk als het zich in een omgeving met selfservice, waar deze kan essentieel zijn in een omgeving beheerd door een partner. In dit artikel wordt ervan uitgegaan dat de isolatie van tenants vereist is.

De architectuur wordt weergegeven in het volgende diagram.

![Gedeelde HSP met een VMware vCenter](./media/vmware-azure-multi-tenant-overview/shared-hosting-scenario.png)  

**Gedeeld-hosting met een vCenter-server**

In het diagram heeft elke klant een afzonderlijke beheerserver. Deze configuratie tenant toegang beperkt tot tenant-specifieke virtuele machines en tenantisolatie maakt. VMware VM-replicatie maakt gebruik van de configuratieserver voor virtuele machines detecteren en de installatie van agents. Voor omgevingen met meerdere tenants, met de toevoeging van het beperken van de VM-detectie met behulp van toegangsbeheer voor vCenter gelden dezelfde principes.

De gegevensvereiste-isolatie betekent dat alle infrastructuur voor gevoelige informatie (zoals toegangsreferenties) niet openbaar te maken voor tenants blijft. Daarom is het raadzaam dat alle onderdelen van de beheerserver onder de exclusieve controle van de partner blijven. De management server-onderdelen zijn:

* Configuratieserver
* Processerver
* Hoofddoelserver

Een afzonderlijke scale-out processerver is ook onder beheer van de partner.

## <a name="configuration-server-accounts"></a>Configuratie-serveraccounts

Elke configuratieserver in het scenario met meerdere tenants gebruikt twee accounts:

- **account voor toegang tot vCenter**: dit account wordt gebruikt voor het detecteren van de tenant-VM's. VCenter toegangsmachtigingen worden toegewezen aan deze heeft. Om te voorkomen van toegang lekken, is het raadzaam dat partners in het configuratiehulpprogramma van deze referenties zelf invoert.

- **Virtuele machine toegangsaccount**: dit account wordt gebruikt voor het installeren van de Mobility-service-agent op de tenant-VM's, met een automatische push. Dit is meestal een domeinaccount die een tenant aan een partner kan leveren, of een account dat de partner direct kunt beheren. Als een tenant niet de gegevens rechtstreeks delen met de partner, kunnen ze Voer de referenties via tijdelijke toegang met de configuratieserver. Of met de hulp van de partner, kunnen ze de Mobility-service-agent handmatig installeren.

## <a name="vcenter-account-requirements"></a>accountvereisten voor vCenter

De configuratieserver configureren met een account met een speciale rol zijn toegewezen.

- De roltoewijzing moet worden toegepast op de vCenter-toegangsaccount voor elk object vCenter en worden niet doorgegeven aan de onderliggende objecten. Deze configuratie zorgt ervoor dat de isolatie van tenants, omdat toegang doorgifte in onbedoelde toegang tot andere objecten resulteren kan.

    ![Het doorgeven naar de onderliggende objecten-optie](./media/vmware-azure-multi-tenant-overview/assign-permissions-without-propagation.png)

- De alternatieve methode is het toewijzen van het gebruikersaccount en de rol op het datacenter-object, en het doorgeven ervan aan de onderliggende objecten. Geef het account een **geen toegang** rol voor ieder object (zoals virtuele machines die deel uitmaken van andere tenants) die moeten toegankelijk is voor een bepaalde tenant. Deze configuratie is het omslachtig. Besturingselementen voor toegang per ongeluk beschikbaar worden gemaakt omdat elke nieuwe onderliggend object is ook automatisch toegang die overgenomen van het bovenliggende item. Daarom raden wij aan dat u de eerste benadering.

### <a name="create-a-vcenter-account"></a>Een vCenter-account maken

1. Een nieuwe rol maken door het klonen van de vooraf gedefinieerde *alleen-lezen* rol, en wijs hieraan een handige naam (zoals Azure_Site_Recovery, zoals wordt weergegeven in dit voorbeeld).
2. De volgende machtigingen toewijzen aan deze rol:

    * **Gegevensopslag**: toewijzen van ruimte, bladeren gegevensopslag, bestandsbewerkingen op laag niveau, bestand, updatebestanden voor de virtuele machine
    * **Netwerk**: netwerk toewijzen
    * **Resource**: virtuele machine toewijzen aan resourcegroep, VM, uitgeschakeld migreren migreren ingeschakeld virtuele machine
    * **Taken**: taak, Update-taak maken
    * **VM - configuratie**: alle
    - **VM - interactie** > beantwoorden vraag, apparaatverbinding, CD configureren media, diskettes configureren, uitschakelen, inschakelen, VMware-hulpprogramma's installeren
    - **VM - inventarisatie** > maken op basis van bestaande, maak een nieuwe, registreren, registratie opheffen
    - **VM - inrichting** > downloaden van de virtuele machine toestaan, toestaan virtuele machine bestanden uploaden
    - **VM - beheer van momentopnamen** > momentopnamen verwijderen

        ![Het dialoogvenster rol bewerken](./media/vmware-azure-multi-tenant-overview/edit-role-permissions.png)

3. Toegangsniveaus op de vCenter-account (gebruikt in de tenant-configuratieserver) voor verschillende objecten als volgt toewijzen:

>| Object | Rol | Opmerkingen |
>| --- | --- | --- |
>| vCenter | Alleen-lezen | Vereist alleen vCenter om toegang te verlenen voor het beheren van verschillende objecten. U kunt deze machtiging verwijderen als het account nooit worden opgegeven voor een tenant of gebruikt voor alle bewerkingen op de vCenter. |
>| Datacenter | Azure_Site_Recovery |  |
>| Host- en -hostcluster | Azure_Site_Recovery | Opnieuw zorgt ervoor dat toegang op objectniveau, zodat alleen toegankelijk hosts tenant-VM's voor failover- en na de failback hebben. |
>| Gegevensopslag en gegevensopslag-cluster | Azure_Site_Recovery | Hetzelfde als de voorgaande. |
>| Netwerk | Azure_Site_Recovery |  |
>| Beheerserver | Azure_Site_Recovery | Omvat toegang tot alle onderdelen (CS-, PS- en MT) buiten de CS-machine. |
>| Tenant-VM 's | Azure_Site_Recovery | Zorgt ervoor dat een nieuwe tenant-VM's van een bepaalde tenant ook deze toegang krijgen, of ze zich niet kunnen worden gedetecteerd via Azure portal. |

De toegang tot het vCenter is nu voltooid. Deze stap wordt voldaan aan de vereiste minimale machtigingen voor het failbackbewerkingen worden voltooid. U kunt deze machtigingen ook gebruiken met uw bestaande beleidsregels. Uw bestaande machtigingen zijn ingesteld op de machtigingen van de rol van stap 2, gedetailleerde eerder alleen wijzigen.

### <a name="failover-only"></a>Alleen failover
Om te beperken van bewerkingen voor noodherstel accountpagina alleen failover (dat wil zeggen, zonder failback mogelijkheden), gebruikt u de vorige procedure, met de volgende uitzonderingen:

- In plaats van het toewijzen van de *Azure_Site_Recovery* rol aan het account van de toegang tot vCenter toewijzen alleen een *alleen-lezen* rol aan dit account. Deze machtigingengroep kunt VM-replicatie en failover en failback niet mogelijk.
- Alle andere items in het vorige proces is blijft. Zorg ervoor dat de isolatie van tenants en VM-detectie te beperken, is elke machtiging nog steeds op het objectniveau van het alleen worden toegewezen en niet doorgegeven aan de onderliggende objecten.

### <a name="deploy-resources-to-the-tenant-subscription"></a>Resources implementeren op het tenantabonnement

1. Maak een resourcegroep in Azure portal, en implementeert u een Recovery Services-kluis per het normale proces.
2. Download de registratiesleutel voor de kluis.
3. Registreer de CS voor de tenant met behulp van de kluisregistratiesleutel.
4. Voer de referenties voor de twee toegangsaccounts, het account voor toegang tot de vCenter-server en het account voor toegang tot de virtuele machine.

    ![Accounts van server configuration Manager](./media/vmware-azure-multi-tenant-overview/config-server-account-display.png)

### <a name="register-servers-in-the-vault"></a>Servers in de kluis registreren

1. In de Azure-portal, in de kluis die u eerder hebt gemaakt, registreert u de vCenter-server met de configuratieserver met behulp van de vCenter-account dat u hebt gemaakt.
2. Het 'Infrastructuur voorbereiden'-proces te voltooien voor Site Recovery per het normale proces.
3. De virtuele machines zijn nu gereed om te worden gerepliceerd. Controleer of dat alleen van de tenant-VM's worden weergegeven **repliceren** > **virtuele machines selecteren**.

## <a name="dedicated-hosting-solution"></a>Toegewezen hosting oplossing

Zoals in het volgende diagram wordt weergegeven, is het architectuur verschil in een specifieke oplossing voor hosting van elke tenant infrastructuur is ingesteld voor de tenant alleen.

![architectuur-gedeeld-hsp](./media/vmware-azure-multi-tenant-overview/dedicated-hosting-scenario.png)  
**Scenario met hosting met meerdere vCenters toegewezen**

## <a name="managed-service-solution"></a>Beheerde service-oplossing

Zoals in het volgende diagram wordt weergegeven, is het architectuur verschil in een beheerde service-oplossing van elke tenant infrastructuur is ook fysiek gescheiden is van andere tenants infrastructuur. In dit scenario bestaat meestal wanneer de tenant eigenaar is van de infrastructuur en wil dat de oplossingsprovider van een voor het beheren van herstel na noodgevallen.

![architectuur-gedeeld-hsp](./media/vmware-azure-multi-tenant-overview/managed-service-scenario.png)  
**Beheerde service-scenario met meerdere vCenters**

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie](site-recovery-role-based-linked-access-control.md) over op rollen gebaseerd toegangsbeheer in Site Recovery.
- Meer informatie over het [herstel na noodgevallen van virtuele VMware-machines naar Azure instellen](vmware-azure-tutorial.md).
- Meer informatie over [multitenancy met CSP voor virtuele VMWare-machines](vmware-azure-multi-tenant-csp-disaster-recovery.md).
