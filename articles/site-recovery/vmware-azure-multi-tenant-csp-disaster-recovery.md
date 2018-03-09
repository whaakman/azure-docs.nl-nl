---
title: Instellen van de VMware-replicatie naar Azure in multi-tenancymodus-omgeving met behulp van Site Recovery en het programma Cloud Solution Provider (CSP) | Microsoft Docs
description: Hierin wordt beschreven hoe u maken en beheren van tenant-abonnementen via de CSP en Azure Site Recovery implementeert in een multitenant-instellingen
services: site-recovery
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: manayar
ms.openlocfilehash: 25591acb3f046744400f5dcf20a7ea651a7bcf54
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="set-up-vmware-replication-in-a-multi-tenancy-environment-with-the-cloud-solution-provider-csp-program"></a>De VMware-replicatie in een omgeving met meerdere tenants met het programma Cloud Solution Provider (CSP) instellen

De [CSP programma](https://partner.microsoft.com/en-US/cloud-solution-provider) bevordert beter samen artikelen voor Microsoft-cloudservices, zoals Office 365, Enterprise Mobility Suite en Microsoft Azure. Met de CSP, partners eigenaar van de end-to-end-relatie met klanten en worden de contactpunt voor de primaire relatie. Partners kunnen Azure-abonnementen voor klanten implementeren en de abonnementen, met hun eigen toegevoegde waarde, aangepaste aanbiedingen combineren.

Met [Azure Site Recovery](site-recovery-overview.md), als partners u herstel na noodgevallen voor klanten rechtstreeks via de CSP kunt beheren. U kunt ook CSP gebruiken voor het instellen van Site Recovery-omgevingen, en laat klanten hun eigen herstel na noodgevallen nodig op een manier selfservice beheren. Partners zijn in beide gevallen wordt de verbinding tussen de Site Recovery en hun klanten. Partners de klantrelatie service en facturen voor gebruik van de Site Recovery.

Dit artikel wordt beschreven hoe u als partner kunt maken en beheren van tenant-abonnementen via CSP, voor een scenario met meerdere tenants VMware-replicatie.

## <a name="prerequisites"></a>Vereisten

Als u de VMware-replicatie wilt instellen, moet u het volgende doen:

- [Voorbereiden](tutorial-prepare-azure.md) Azure-resources, zoals een Azure-abonnement, een Azure-netwerk en een opslagaccount.
- [Voorbereiden](vmware-azure-tutorial-prepare-on-premises.md) on-premises VMware-servers en virtuele machines. 
- Voor elke tenant een afzonderlijke beheerserver die kan communiceren met de tenant-VM's en de vCenter-servers te maken. Alleen hebt u als een partner toegangsrechten voor deze beheerserver. Meer informatie over [multitenant-omgevingen](vmware-azure-multi-tenant-overview.md).

## <a name="create-a-tenant-account"></a>Een tenantaccount maken

1. Via [Microsoft Partner Center](https://partnercenter.microsoft.com/), zich aanmelden bij uw account CSP.
2. Op de **Dashboard** selecteert u **klanten**.
3. Op de pagina die wordt geopend, klikt u op de **klant toevoegen** knop.
4. In **nieuwe klant** pagina, vult u de accountdetails van de informatie voor de tenant. 

    ![De pagina accountgegevens](./media/vmware-azure-multi-tenant-csp-disaster-recovery/customer-add-filled.png)

5. Klik vervolgens op **volgende: abonnementen**.
6. Selecteer op de pagina abonnementen selectie **Microsoft Azure** selectievakje. U kunt andere abonnementen nu of op elk gewenst moment toevoegen.
7. Op de **revisie** pagina, om de details van de tenant te bevestigen en klik vervolgens op **indienen**.
8. Nadat u het tenantaccount hebt gemaakt, verschijnt er een bevestigingspagina waarin de details van het standaard-account en het wachtwoord voor dat abonnement. Sla de gegevens en het wachtwoord later als nodig is, via de Azure portal-aanmeldingspagina te wijzigen.

U kunt deze informatie delen met de tenant is of u kunt maken en delen van een afzonderlijk account indien nodig.

## <a name="access-the-tenant-account"></a>Toegang tot de tenantaccount

Abonnement van de tenant is toegankelijk via het Microsoft Partner Center-Dashboard.

1. Op de **klanten** pagina, klikt u op de naam van de tenantaccount.
2. In de **abonnementen** pagina van de tenantaccount kunt u de bestaande abonnementen te controleren en meer abonnementen toevoegen zoals vereist.
3. Selecteer voor het beheren van de tenant herstel na noodgevallen operations **alle resources (Azure-portal)**. Dit biedt dat u toegang tot Azure-abonnementen van de tenant.

    ![De koppeling met alle Resources](./media/vmware-azure-multi-tenant-csp-disaster-recovery/all-resources-select.png)  

4. U kunt toegang controleren door te klikken op de Azure Active Directory-koppeling op de bovenkant van de Azure-portal.

    ![Azure Active Directory link](./media/vmware-azure-multi-tenant-csp-disaster-recovery/aad-admin-display.png)

U kunt nu uitvoeren en beheren van alle Site Recovery-bewerkingen voor de tenant in de Azure portal. Voor toegang tot het tenantabonnement via de CSP voor beheerde noodherstel, volgt u de eerder beschreven procedure.

## <a name="deploy-resources-to-the-tenant-subscription"></a>Resources aan het tenantabonnement implementeren

1. Een resourcegroep maken in de Azure portal en vervolgens implementeert u een Recovery Services-kluis per normale proces.
2. Download de registratiesleutel voor de kluis.
3. Registreer de CS voor de tenant met behulp van de sleutel van de registratie van de kluis.

4. Geef de referenties voor de twee toegangsaccounts het account voor toegang tot de vCenter-server en de acount voor toegang tot de virtuele machine.

    ![Accounts van server configuration Manager](./media/vmware-azure-multi-tenant-csp-disaster-recovery/config-server-account-display.png)

## <a name="register-servers-in-the-vault"></a>Servers in de kluis registreren

1. In de Azure-portal in de kluis die u eerder hebt gemaakt, registreert u de vCenter-server en de configuratieserver met de vCenter-account dat u hebt gemaakt. 
2. Voltooiing van het 'Infrastructuur voorbereiden'-proces voor siteherstel per normale proces.
3. De virtuele machines zijn nu gereed om te worden gerepliceerd. Zorg dat alleen virtuele machines van de tenant worden weergegeven **repliceren** > **virtuele machines selecteren**.


## <a name="assign-tenant-access-to-the-subscription"></a>Tenant toegang tot het abonnement toewijzen

1. Zorg ervoor dat het herstel na noodgevallen infrastructuur is ingesteld. Houd er rekening mee dat u moet toegang hebben tot tenant abonnementen via de CSP-portal, regarless of herstel na noodgevallen wordt beheerd of self-service. U moet uw kluis instellen en infrastructuur voor de tenant-abonnementen te registreren.
2. Geef de tenant met de [-account voor u gemaakt](#create-a-tenant-account)
3. U kunt als volgt een nieuwe gebruiker toevoegen aan de tenant-abonnement via de CSP-portal:

    een) gaat u naar de pagina van de tenant CSP-abonnement en selecteer vervolgens de **gebruikers en licenties** optie.

        ![The tenant's CSP subscription page](./media/vmware-azure-multi-tenant-csp-disaster-recovery/users-and-licences.png)

    b) Maak een nieuwe gebruiker nu door te voeren van de relevante gegevens en machtigingen te selecteren of door de lijst met gebruikers in een CSV-bestand uploaden.
    c) nadat u een nieuwe gebruiker hebt gemaakt, gaat u terug naar de Azure-portal. In de **abonnement** pagina, selecteert u het desbetreffende abonnement.
    d) Selecteer **Access Control (IAM)**, en klik vervolgens op **toevoegen**, een gebruiker met de relevante toegangsniveau toe te voegen. De gebruikers die zijn gemaakt via de portal CSP worden automatisch weergegeven op de pagina die wordt geopend nadat u een toegangsniveau op.

        ![Add a user](./media/vmware-azure-multi-tenant-csp-disaster-recovery/add-user-subscription.png)

- Voor de meeste management bewerkingen de *Inzender* rol is voldoende. Gebruikers met dit toegangsniveau kunnen doen alles op een abonnement, behalve toegangsniveaus wijzigen (waarvoor *eigenaar*-toegang is vereist).
- Site Recovery heeft ook drie [vooraf gedefinieerde gebruikersrollen](site-recovery-role-based-linked-access-control.md), die kan worden gebruikt voor de toegangsniveaus zo nodig verder te beperken.

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie](site-recovery-role-based-linked-access-control.md) over op rollen gebaseerde toegangsbeheer voor het beheren van Azure Site Recovery-implementaties.
- [Meer informatie](vmware-azure-architecture.md) over VMware naar Azure replicatie-architectuur.
- [Bekijk de zelfstudie](vmware-azure-tutorial.md) voor virtuele VMware-machines repliceren naar Azure.
