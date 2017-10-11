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
ms.date: 06/23/2017
ms.author: manayar
ms.openlocfilehash: 97edbe67c25036dc1156f0f0ca5431a617d7a004
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="multi-tenant-support-in-azure-site-recovery-for-replicating-vmware-virtual-machines-to-azure-through-csp"></a>Multitenant-ondersteuning in Azure Site Recovery voor virtuele VMware-machines repliceren naar Azure via CSP

Azure Site Recovery biedt ondersteuning voor meerdere tenants omgevingen voor abonnementen van de tenant. Het ondersteunt ook multi-tenancymodus voor tenant-abonnementen die worden gemaakt en beheerd via het programma Cloud Solution Provider (CSP). In dit artikel beschrijft de richtlijn voor de implementatie en beheer van meerdere tenants VMware naar Azure-scenario's. Het omvat tevens maken en beheren van tenant-abonnementen via de CSP.

In deze richtlijnen wordt getekend sterk van de bestaande documentatie voor de virtuele VMware-machines repliceren naar Azure. Zie voor meer informatie [repliceren VMware-machines in Azure met Site Recovery](site-recovery-vmware-to-azure.md).

## <a name="multi-tenant-environments"></a>Multitenant-omgevingen
Er zijn drie primaire modellen voor meerdere tenants:

* **Hosting-Services Provider (HSP) gedeeld**: de partner is eigenaar van de fysieke infrastructuur en maakt gebruik van gedeelde resources (vCenter, datacenters, fysieke opslag, enzovoort) voor het hosten van meerdere tenants virtuele machines op dezelfde infrastructuur. Herstel na noodgevallen management als een beheerde service krijgt u de partner of herstel na noodgevallen als een oplossing voor self-service kan eigenaar van de tenant.

* **Hosting-Services Provider-specifieke**: de partner eigenaar is van de fysieke infrastructuur, maar toegewezen resources (meerdere Vcenter, fysieke datastores, enzovoort) worden gebruikt voor het hosten van elke tenant-VM's op een aparte infrastructuur. De partner management als een beheerde service herstel na noodgevallen kunt opgeven of de tenant kan de eigenaar ervan als een oplossing voor selfservice.

* **Beheerde Provider (MSP-Services)**: de klant eigenaar van de fysieke infrastructuur die als host fungeert voor de virtuele machines en de partner biedt inschakelen voor herstel na noodgevallen en beheer.

## <a name="shared-hosting-multi-tenant-guidance"></a>Gedeeld-hosting multitenant richtlijnen
Deze sectie worden het scenario gedeeld-hosting in detail behandeld. De andere twee scenario's zijn subsets van het scenario gedeeld-hosting en ze gebruiken dezelfde principes. De verschillen worden beschreven aan het einde van de richtlijnen gedeeld-hosting.

Een eerste vereiste in een multitenant-scenario is het isoleren van de verschillende tenants. Een tenant moet niet kunnen zien wat een andere tenant is gehost. Deze vereiste is niet zo belangrijk als het zich in een omgeving met selfservice, waar dit kan essentieel zijn in een omgeving beheerd door een partner. In deze richtlijnen wordt ervan uitgegaan dat de isolatie van tenants vereist is.

De architectuur is opgenomen in het volgende diagram:

![Gedeelde HSP met één vCenter](./media/site-recovery-multi-tenant-support-vmware-using-csp/shared-hosting-scenario.png)  
**Gedeeld-hosting scenario met één vCenter**

Zoals in het voorgaande diagram kunt zien, heeft elke klant een afzonderlijke beheerserver. Deze configuratie tenant toegang beperkt tot tenantspecifieke VM's en schakelt tenantisolatie. Een VMware-replicatie voor virtuele machines scenario maakt gebruik van de configuratieserver voor het beheren van accounts voor virtuele machines detecteren en agents installeren. We volgt u dezelfde principes multitenant-omgevingen, met de toevoeging van VM-detectie via vCenter toegangsbeheer beperken.

De vereiste isolatie van gegevens vereist dat alle infrastructuur gevoelige gegevens (zoals toegangsreferenties) blijven niet openbaar te maken voor tenants. Daarom is het raadzaam dat alle onderdelen van de beheerserver onder de exclusieve controle van de partner blijven. De management server-onderdelen zijn:
* Configuratieserver (CS)
* De processerver (PS)
* Hoofddoelserver (MT) 

Een scale-out PS is ook onder het beheer van de partner.

### <a name="every-cs-in-the-multi-tenant-scenario-uses-two-accounts"></a>Elke CS in het scenario voor meerdere tenants gebruikt twee accounts

- **vCenter toegangsaccount**: dit account gebruiken voor het detecteren van tenant-VM's. Hieraan toegangsmachtigingen vCenter toegewezen (zoals beschreven in de volgende sectie). Om te voorkomen van toegang onbedoeld lekken, is het raadzaam dat partners in het configuratiehulpprogramma van deze referenties zelf invoert.

- **Virtuele machine toegangsaccount**: dit account gebruiken voor het installeren van de mobility-agent op de tenant-VM's via een automatische push. Dit is meestal een domeinaccount dat een tenant aan een partner bieden kan of dat u kunt ook de partner kan rechtstreeks beheren. Als een tenant niet delen van de details met de partner rechtstreeks wilt, kunt hij of zij mogen worden voert u de referenties met de tijdelijke toegang aan de CS of met hulp bij de partner handmatig mobility agents installeren.

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
>| Beheerserver | Azure_Site_Recovery | Toegang tot alle onderdelen (CS PS en MT) bevat als een buiten de CS-machine. |
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

## <a name="csp-program-overview"></a>Overzicht van de CSP-programma
De [CSP programma](https://partner.microsoft.com/en-US/cloud-solution-provider) bevordert beter samen artikelen die worden geboden door partners alle Microsoft-cloudservices, zoals Office 365, Enterprise Mobility Suite en Microsoft Azure. Met de CSP, onze partners eigenaar van de end-to-end-relatie met klanten en worden de contactpunt voor de primaire relatie. Partners kunnen Azure-abonnementen voor klanten implementeren en de abonnementen, met hun eigen toegevoegde waarde, aangepaste aanbiedingen combineren.

Partners kunnen de volledige oplossing voor herstel na noodgevallen voor klanten rechtstreeks via CSP beheren met Azure Site Recovery. Of ze CSP instellen Site Recovery-omgevingen en laat klanten hun eigen behoeften voor herstel na noodgevallen beheren op een wijze die Self-service kunnen gebruiken. Partners zijn in beide gevallen wordt de verbinding tussen de Site Recovery en hun klanten. Partners de klantrelatie service en facturen voor gebruik van de Site Recovery.

## <a name="create-and-manage-tenant-accounts"></a>Maken en beheren van tenantaccounts

### <a name="step-0-prerequisite-check"></a>Stap 0: Controle van vereisten

De VM-vereisten zijn hetzelfde zoals beschreven in de [documentatie van Azure Site Recovery](site-recovery-vmware-to-azure.md). Naast deze vereisten, u moet beschikken over de eerder genoemde toegangscontrole voordat u met tenant-beheer via de CSP verdergaat. Voor elke tenant, maakt u een afzonderlijke beheerserver die kan communiceren met de tenant-VM's en vCenter van partners. Alleen de partner heeft toegangsrechten voor deze server.

### <a name="step-1-create-a-tenant-account"></a>Stap 1: Een tenantaccount maken

1. Via [Microsoft Partner Center](https://partnercenter.microsoft.com/), zich aanmelden bij uw account CSP. 
 
2. Op de **Dashboard** selecteert u **klanten**.

    ![De klanten van Microsoft-Partner Center-koppeling](./media/site-recovery-multi-tenant-support-vmware-using-csp/csp-dashboard-display.png)

3. Op de pagina die wordt geopend, klikt u op de **klant toevoegen** knop.

    ![De knop klant toevoegen](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-new-customer.png)

4. Op de **nieuwe klant** pagina, vult u de accountdetails van de informatie voor de tenant en klik vervolgens op **volgende: abonnementen**.

    ![De pagina accountgegevens](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-add-filled.png)

5. Selecteer op de pagina abonnementen het **Microsoft Azure** selectievakje. U kunt andere abonnementen nu of op elk gewenst moment toevoegen.

    ![Het selectievakje van de Microsoft Azure-abonnement](./media/site-recovery-multi-tenant-support-vmware-using-csp/azure-subscription-selection.png)

6. Op de **revisie** pagina, om de details van de tenant te bevestigen en klik vervolgens op **indienen**.

    ![De pagina controleren](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-summary-page.png)  

    Nadat u het tenantaccount hebt gemaakt, verschijnt er een bevestigingspagina waarin de details van het standaard-account en het wachtwoord voor dat abonnement. 

7. Sla de gegevens en het wachtwoord later als nodig is via de Azure portal-aanmeldingspagina te wijzigen.  
 
    U kunt deze informatie delen met de tenant is of u kunt maken en delen van een afzonderlijk account indien nodig.

### <a name="step-2-access-the-tenant-account"></a>Stap 2: Toegang tot de tenantaccount

U kunt toegang tot de tenant abonnement via het Microsoft Partner Center-Dashboard zoals beschreven in ' stap 1: een tenantaccount maken. " 

1. Ga naar de **klanten** pagina en klik vervolgens op de naam van de tenantaccount.

2. Op de **abonnementen** pagina van de tenantaccount kunt u de bestaande abonnementen te controleren en meer abonnementen toevoegen zoals vereist. Selecteer voor het beheren van de tenant herstel na noodgevallen operations **alle resources (Azure-portal)**.

    ![De koppeling met alle Resources](./media/site-recovery-multi-tenant-support-vmware-using-csp/all-resources-select.png)  
    
    Te klikken op **alle resources** biedt u toegang tot Azure-abonnementen van de tenant. U kunt toegang controleren door te klikken op de Azure Active Directory-koppeling aan de bovenkant van de Azure-portal.

    ![Azure Active Directory-koppeling](./media/site-recovery-multi-tenant-support-vmware-using-csp/aad-admin-display.png)

U kunt nu alle site-en herstelbewerkingen op voor de tenant via de Azure portal uitvoeren en beheren van de bewerkingen voor herstel na noodgevallen. Voor toegang tot het tenantabonnement via de CSP voor beheerde noodherstel, volgt u de eerder beschreven procedure.

### <a name="step-3-deploy-resources-to-the-tenant-subscription"></a>Stap 3: Resources aan het tenantabonnement implementeren
1. Een resourcegroep maken in de Azure portal en vervolgens implementeert u een Recovery Services-kluis per normale proces. 
 
2. Download de kluisregistratiesleutel.

3. Registreer de CS voor de tenant met behulp van de sleutel van de registratie van de kluis.

4. Voer de referenties voor de twee toegangsaccounts: toegang tot account toegangsaccount vCenter en de VM.

    ![Accounts van server configuration Manager](./media/site-recovery-multi-tenant-support-vmware-using-csp/config-server-account-display.png)

### <a name="step-4-register-site-recovery-infrastructure-to-the-recovery-services-vault"></a>Stap 4: Register Site Recovery-infrastructuur naar de Recovery Services-kluis
1. In de Azure-portal op de kluis die u eerder hebt gemaakt, registreert u de vCenter-server aan de CS die u hebt geregistreerd in ' stap 3: resources implementeren op de tenantabonnement. " Gebruik de vCenter-account voor toegang voor dit doel.
2. Voltooiing van het 'Infrastructuur voorbereiden'-proces voor siteherstel per normale proces.
3. De virtuele machines zijn nu gereed om te worden gerepliceerd. Zorg dat alleen virtuele machines van de tenant worden weergegeven op de **virtuele machines selecteren** blade onder de **repliceren** optie.

    ![Lijst met tenant-VM's op de blade Selecteer virtuele machines](./media/site-recovery-multi-tenant-support-vmware-using-csp/tenant-vm-display.png)

### <a name="step-5-assign-tenant-access-to-the-subscription"></a>Stap 5: Toewijzen tenant toegang tot het abonnement

Voor herstel na noodgevallen selfservice bieden aan de tenant de accountdetails zoals vermeld in stap 6 van de ' stap 1: Maak een tenantaccount ' sectie. Deze actie niet uitvoeren nadat de partner u de infrastructuur voor herstel na noodgevallen stelt. Of het type herstel na noodgevallen beheerd of selfservice is, partners moeten toegang hebben tot tenant abonnementen via de CSP-portal. Ze de kluis die eigendom zijn van partner instellen en infrastructuur voor de tenant-abonnementen te registreren.

Partners kunnen ook een nieuwe gebruiker toevoegen aan het tenantabonnement via de CSP-portal als volgt:

1. Ga naar de pagina van de tenant CSP-abonnement en selecteer vervolgens de **gebruikers en licenties** optie.

    ![Pagina van de tenant CSP-abonnement](./media/site-recovery-multi-tenant-support-vmware-using-csp/users-and-licences.png)

    U kunt nu een nieuwe gebruiker maken door te voeren van de relevante gegevens en machtigingen te selecteren of door de lijst met gebruikers in een CSV-bestand uploaden.

2. Nadat u hebt gemaakt dat een nieuwe gebruiker, gaat u terug naar de Azure portal en klik vervolgens op de **abonnement** blade, selecteert u het desbetreffende abonnement.

3. Selecteer op de blade die wordt geopend, **Access Control (IAM)**, en klik vervolgens op **toevoegen** een gebruiker met de relevante toegangsniveau toe te voegen.      
    De gebruikers die zijn gemaakt via de portal CSP worden automatisch weergegeven op de blade die wordt geopend nadat u een toegangsniveau op.

    ![Een gebruiker toevoegen](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-user-subscription.png)

    Voor de meeste management bewerkingen de *Inzender* rol is voldoende. Gebruikers met dit toegangsniveau kunnen doen alles op een abonnement, behalve toegangsniveaus wijzigen (waarvoor *eigenaar*-toegang is vereist). U kunt ook de toegangsniveaus zo nodig aanpassen.
