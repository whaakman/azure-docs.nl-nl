---
title: Ingesteld op noodherstel VMware naar Azure in een omgeving met meerdere tenants met behulp van Site Recovery en het programma Cloud Solution Provider (CSP) | Microsoft Docs
description: Beschrijft hoe u voor het instellen van VMware-noodherstel in een omgeving met meerdere tenants met Azure Site Recovery.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: mayg
ms.openlocfilehash: d4eac6a2a0b4b9414fdf110c599ff557cabeede5
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52727100"
---
# <a name="set-up-vmware-disaster-recovery-in-a-multi-tenancy-environment-with-the-cloud-solution-provider-csp-program"></a>Herstel van VMware na noodgevallen in een omgeving met meerdere tenants met het programma Cloud Solution Provider (CSP) instellen

De [CSP-programma](https://partner.microsoft.com/en-US/cloud-solution-provider) bevordert beter samen verhalen voor Microsoft-cloudservices, waaronder Office 365, Enterprise Mobility Suite en Microsoft Azure. Met CSP, partners eigenaar van de end-to-end-relatie met klanten en worden de contactpunt voor de primaire relatie. Partners kunnen Azure-abonnementen voor klanten implementeren en de abonnementen, met hun eigen oplossingen met toegevoegde waarde, aangepaste combineren.

Met [Azure Site Recovery](site-recovery-overview.md), als partners kunt u herstel na noodgeval beheren voor klanten rechtstreeks via CSP. U kunt ook CSP gebruiken voor het instellen van Site Recovery-omgevingen, en laat klanten hun eigen behoeften van het herstel na noodgevallen in een self-servicegebruikers manier beheren. In beide gevallen worden de samenwerking tussen Site Recovery en hun klanten. Partners de klantrelatie-service en factureren van klanten voor gebruik met Site Recovery.

Dit artikel wordt beschreven hoe u als partner kunt maken en beheren van de tenant-abonnementen via CSP, voor een scenario met meerdere tenants VMware-replicatie.

## <a name="prerequisites"></a>Vereisten

Als u VMware-replicatie instelt, moet u het volgende doen:

- [Voorbereiden](tutorial-prepare-azure.md) Azure-resources, met inbegrip van een Azure-abonnement, een Azure-netwerk en een storage-account.
- [Voorbereiden](vmware-azure-tutorial-prepare-on-premises.md) on-premises VMware-servers en virtuele machines.
- Voor elke tenant een afzonderlijke beheerserver die kan communiceren met de tenant-VM's en uw vCenter-servers te maken. Alleen u als partner moet toegangsrechten hebben voor deze beheerserver. Meer informatie over [omgevingen met meerdere tenants](vmware-azure-multi-tenant-overview.md).

## <a name="create-a-tenant-account"></a>Een tenantaccount maken

1. Via [Microsoft Partner Center](https://partnercenter.microsoft.com/), meld u aan bij uw CSP-account.
2. Op de **Dashboard** in het menu **klanten**.
3. Op de pagina die wordt geopend, klikt u op de **klant toevoegen** knop.
4. In **nieuwe klant** pagina, vult u de accountdetails van de gegevens voor de tenant.

    ![De pagina accountgegevens](./media/vmware-azure-multi-tenant-csp-disaster-recovery/customer-add-filled.png)

5. Klik vervolgens op **volgende: abonnementen**.
6. Selecteer op de pagina Abonnementen selecteren **Microsoft Azure** selectievakje. U kunt andere abonnementen nu of op enig ander moment toevoegen.
7. Op de **revisie** pagina, bevestig de tenantdetails van de en klik vervolgens op **indienen**.
8. Nadat u de tenantaccount hebt gemaakt, verschijnt een bevestigingspagina weergeven van de details van de standaard-account en het wachtwoord voor dat abonnement. Sla de gegevens en het wachtwoord later zo nodig, via de Azure portal-aanmeldingspagina te wijzigen.

U kunt deze informatie delen met de tenant is, of u kunt maken en delen van een afzonderlijk account indien nodig.

## <a name="access-the-tenant-account"></a>Toegang tot de tenantaccount

Abonnement van de tenant is toegankelijk via het Microsoft Partner Center-Dashboard.

1. Op de **klanten** pagina, klikt u op de naam van de tenantaccount.
2. In de **abonnementen** pagina van de tenantaccount kunt u de bestaande abonnementen te controleren en meer abonnementen toevoegen zoals vereist.
3. Selecteer voor het beheren van de tenant-NOODHERSTELBEWERKINGEN, **alle resources (Azure portal)**. Dit biedt dat u toegang tot Azure-abonnementen van de tenant.

    ![De koppeling alle Resources](./media/vmware-azure-multi-tenant-csp-disaster-recovery/all-resources-select.png)  

4. U kunt toegang controleren door te klikken op de Azure Active Directory-koppeling in de rechterbovenhoek van de Azure-portal.

    ![Azure Active Directory link](./media/vmware-azure-multi-tenant-csp-disaster-recovery/aad-admin-display.png)

U kunt nu uitvoeren en beheren van alle Site Recovery-bewerkingen voor de tenant in Azure portal. Voor toegang tot de tenantabonnement via CSP voor beheerde noodherstel, volgt u de eerder beschreven procedure.

## <a name="assign-tenant-access-to-the-subscription"></a>Tenant toegang tot het abonnement toewijzen

1. Zorg ervoor dat de infrastructuur voor noodherstel is ingesteld. Partners toegang krijgen tot tenant abonnementen via de portal van de CSP, ongeacht of herstel na noodgevallen wordt beheerd of selfservice. Instellen van de kluis en -infrastructuur om de abonnementen van de tenant te registreren.
2. Geef de tenant met de [dat u hebt gemaakt](#create-a-tenant-account).
3. U kunt als volgt een nieuwe gebruiker toevoegen aan de tenantabonnement via de portal van de CSP:

    (a) Ga naar de pagina van de tenant CSP-abonnement en selecteer vervolgens de **gebruikers en licenties** optie.

      ![Pagina voor CSP-abonnement van de tenant](./media/vmware-azure-multi-tenant-csp-disaster-recovery/users-and-licences.png)

    (b) nu een nieuwe gebruiker maken door te voeren van de relevante gegevens en machtigingen selecteren of door de lijst met gebruikers in een CSV-bestand uploaden.
    
    c) nadat u een nieuwe gebruiker hebt gemaakt, Ga terug naar de Azure-portal. In de **abonnement** pagina, selecteert u het desbetreffende abonnement.

    d) Selecteer **toegangsbeheer (IAM)**, en klik vervolgens op **roltoewijzingen**.

    e) Klik op **roltoewijzing toevoegen** een gebruiker met de relevante toegangsniveau toe te voegen. De gebruikers die zijn gemaakt via de CSP-portal worden weergegeven op het tabblad rol toewijzingen.

      ![Een gebruiker toevoegen](./media/vmware-azure-multi-tenant-csp-disaster-recovery/add-user-subscription.png)

- Voor de meeste beheerbewerkingen, de *Inzender* rol is voldoende. Gebruikers met dit toegangsniveau kunnen doen alles op een abonnement, behalve toegangsniveaus wijzigen (waarvoor *eigenaar*-niveau toegang is vereist).
- Site Recovery heeft drie [vooraf gedefinieerde gebruikersrollen](site-recovery-role-based-linked-access-control.md), die kunnen worden gebruikt voor het verder beperken toegangsniveaus zoals vereist.

## <a name="multi-tenant-environments"></a>Omgevingen met meerdere tenants

Er zijn drie belangrijke modellen voor meerdere tenants:

* **Gedeeld die als host fungeert Services serviceprovider (HSP)**: de partner is eigenaar van de fysieke infrastructuur, en maakt gebruik van gedeelde resources (vCenter, datacenters, fysieke opslag, enzovoort) voor het hosten van meerdere tenant-VM's op dezelfde infrastructuur. De partner herstel na noodgevallen management als een beheerde service kunt opgeven of de tenant eigenaar kan herstel na noodgevallen als een oplossing voor de selfservice.

* **Services-Provider die als host fungeert toegewezen**: de partner is eigenaar van de fysieke infrastructuur, maar worden toegewezen resources (meerdere vCenters, fysieke gegevensopslag, enzovoort) gebruikt voor het hosten van elke tenant-VM's op een aparte infrastructuur. De partner herstel na noodgevallen management als een beheerde service kunt opgeven of de tenant eigenaar kan het als een oplossing voor de selfservice.

* **Beheerde Services Provider (MSP)**: de klant is eigenaar van de fysieke infrastructuur die als host fungeert voor de virtuele machines en de partner biedt inschakelen voor herstel na noodgevallen en beheer.

Door het instellen van tenant abonnementen zoals beschreven in dit artikel, kunt u snel starten zodat klanten in een van de relevante multitenant-modellen. U kunt meer informatie over de verschillende modellen voor meerdere tenants en inschakelen van on-premises toegangsbeheer [hier](vmware-azure-multi-tenant-overview.md).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [op rollen gebaseerd toegangsbeheer](site-recovery-role-based-linked-access-control.md) voor het beheren van Azure Site Recovery-implementaties.
- Meer informatie over VMware naar Azure [replicatiearchitectuur](vmware-azure-architecture.md).
- [Bekijk de zelfstudie](vmware-azure-tutorial.md) voor het repliceren van virtuele VMware-machines naar Azure.
Meer informatie over [omgevingen met meerdere tenants](vmware-azure-multi-tenant-overview.md) voor het repliceren van virtuele VMware-machines naar Azure.
