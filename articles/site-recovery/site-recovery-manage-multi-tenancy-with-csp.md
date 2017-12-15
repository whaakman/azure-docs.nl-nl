---
title: Beheren van multi-tenancymodus in Azure Site Recovery met het programma Cloud Solution Provider (CSP) | Microsoft Docs
description: Hierin wordt beschreven hoe u maken en beheren van tenant-abonnementen via de CSP en Azure Site Recovery implementeert in een multitenant-instellingen
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
ms.openlocfilehash: a49da33c8038ad467389c66e59727c7e195baf82
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="manage-multi-tenancy-with-the-cloud-solution-provider-csp-program"></a>Multi-tenancymodus met het programma Cloud Solution Provider (CSP) beheren

De [CSP programma](https://partner.microsoft.com/en-US/cloud-solution-provider) bevordert beter samen artikelen voor Microsoft-cloudservices, zoals Office 365, Enterprise Mobility Suite en Microsoft Azure. Met de CSP, partners eigenaar van de end-to-end-relatie met klanten en worden de contactpunt voor de primaire relatie. Partners kunnen Azure-abonnementen voor klanten implementeren en de abonnementen, met hun eigen toegevoegde waarde, aangepaste aanbiedingen combineren.

Partners kunnen de volledige oplossing voor herstel na noodgevallen voor klanten rechtstreeks via CSP beheren met Azure Site Recovery. Of ze CSP instellen Site Recovery-omgevingen en laat klanten hun eigen behoeften voor herstel na noodgevallen beheren op een wijze die Self-service kunnen gebruiken. Partners zijn in beide gevallen wordt de verbinding tussen de Site Recovery en hun klanten. Partners de klantrelatie service en facturen voor gebruik van de Site Recovery.

Dit artikel wordt beschreven hoe een partner maken en beheren van de tenant abonnementen via CSP, voor een VMware-configuratie voor meerdere tenants.

## <a name="prerequisites"></a>Vereisten

- [Voorbereiden](tutorial-prepare-azure.md) Azure-resources, zoals een Azure-abonnement, een Azure-netwerk en een opslagaccount.
- [Voorbereiden](tutorial-prepare-on-premises-vmware.md) VMware on-premises VMware-servers en virtuele machines.
- Voor elke tenant, maakt u een afzonderlijke beheerserver die kan communiceren met de tenant-VM's en vCenter van partners. Alleen de partner heeft toegangsrechten voor deze server. Voor meer informatie over de verschillende omgevingen met meerdere tenants naar verwijzen de [multitenant VMware](site-recovery-multi-tenant-support-vmware-using-csp.md) richtlijnen.

## <a name="create-a-tenant-account"></a>Een tenantaccount maken

1. Via [Microsoft Partner Center](https://partnercenter.microsoft.com/), zich aanmelden bij uw account CSP.

2. Op de **Dashboard** selecteert u **klanten**.

    ![De klanten van Microsoft-Partner Center-koppeling](./media/site-recovery-manage-multi-tenancy-with-csp/csp-dashboard-display.png)

3. Op de pagina die wordt geopend, klikt u op de **klant toevoegen** knop.

    ![De knop klant toevoegen](./media/site-recovery-manage-multi-tenancy-with-csp/add-new-customer.png)

4. Op de **nieuwe klant** pagina, vult u de accountdetails van de informatie voor de tenant en klik vervolgens op **volgende: abonnementen**.

    ![De pagina accountgegevens](./media/site-recovery-manage-multi-tenancy-with-csp/customer-add-filled.png)

5. Selecteer op de pagina abonnementen het **Microsoft Azure** selectievakje. U kunt andere abonnementen nu of op elk gewenst moment toevoegen.

    ![Het selectievakje van de Microsoft Azure-abonnement](./media/site-recovery-manage-multi-tenancy-with-csp/azure-subscription-selection.png)

6. Op de **revisie** pagina, om de details van de tenant te bevestigen en klik vervolgens op **indienen**.

    ![De pagina controleren](./media/site-recovery-manage-multi-tenancy-with-csp/customer-summary-page.png)  

    Nadat u het tenantaccount hebt gemaakt, verschijnt er een bevestigingspagina waarin de details van het standaard-account en het wachtwoord voor dat abonnement.

7. Sla de gegevens en het wachtwoord later als nodig is via de Azure portal-aanmeldingspagina te wijzigen.  

    U kunt deze informatie delen met de tenant is of u kunt maken en delen van een afzonderlijk account indien nodig.

## <a name="access-the-tenant-account"></a>Toegang tot de tenantaccount

U kunt toegang tot de tenant abonnement via het Microsoft Partner Center-Dashboard zoals beschreven in ' stap 1: een tenantaccount maken. "

1. Ga naar de **klanten** pagina en klik vervolgens op de naam van de tenantaccount.

2. Op de **abonnementen** pagina van de tenantaccount kunt u de bestaande abonnementen te controleren en meer abonnementen toevoegen zoals vereist. Selecteer voor het beheren van de tenant herstel na noodgevallen operations **alle resources (Azure-portal)**.

    ![De koppeling met alle Resources](./media/site-recovery-manage-multi-tenancy-with-csp/all-resources-select.png)  

    Te klikken op **alle resources** biedt u toegang tot Azure-abonnementen van de tenant. U kunt toegang controleren door te klikken op de Azure Active Directory-koppeling aan de bovenkant van de Azure-portal.

    ![Azure Active Directory-koppeling](./media/site-recovery-manage-multi-tenancy-with-csp/aad-admin-display.png)

U kunt nu alle site-en herstelbewerkingen op voor de tenant via de Azure portal uitvoeren en beheren van de bewerkingen voor herstel na noodgevallen. Voor toegang tot het tenantabonnement via de CSP voor beheerde noodherstel, volgt u de eerder beschreven procedure.

## <a name="deploy-resources-to-the-tenant-subscription"></a>Resources aan het tenantabonnement implementeren
1. Een resourcegroep maken in de Azure portal en vervolgens implementeert u een Recovery Services-kluis per normale proces.

2. Download de kluisregistratiesleutel.

3. Registreer de CS voor de tenant met behulp van de sleutel van de registratie van de kluis.

4. Voer de referenties voor de twee toegangsaccounts: toegang tot account toegangsaccount vCenter en de VM.

    ![Accounts van server configuration Manager](./media/site-recovery-manage-multi-tenancy-with-csp/config-server-account-display.png)

## <a name="register-site-recovery-infrastructure-to-the-recovery-services-vault"></a>Site Recovery-infrastructuur om de Recovery Services-kluis te registreren
1. In de Azure-portal op de kluis die u eerder hebt gemaakt, registreert u de vCenter-server aan de CS die u hebt geregistreerd in ' stap 3: resources implementeren op de tenantabonnement. " Gebruik de vCenter-account voor toegang voor dit doel.
2. Voltooiing van het 'Infrastructuur voorbereiden'-proces voor siteherstel per normale proces.
3. De virtuele machines zijn nu gereed om te worden gerepliceerd. Zorg dat alleen virtuele machines van de tenant worden weergegeven op de **virtuele machines selecteren** blade onder de **repliceren** optie.

    ![Lijst met tenant-VM's op de blade Selecteer virtuele machines](./media/site-recovery-manage-multi-tenancy-with-csp/tenant-vm-display.png)

## <a name="assign-tenant-access-to-the-subscription"></a>Tenant toegang tot het abonnement toewijzen

Voor herstel na noodgevallen selfservice bieden aan de tenant de accountdetails zoals vermeld in stap 6 van de ' stap 1: Maak een tenantaccount ' sectie. Deze actie niet uitvoeren nadat de partner u de infrastructuur voor herstel na noodgevallen stelt. Of het type herstel na noodgevallen beheerd of selfservice is, partners moeten toegang hebben tot tenant abonnementen via de CSP-portal. Ze de kluis die eigendom zijn van partner instellen en infrastructuur voor de tenant-abonnementen te registreren.

Partners kunnen ook een nieuwe gebruiker toevoegen aan het tenantabonnement via de CSP-portal als volgt:

1. Ga naar de pagina van de tenant CSP-abonnement en selecteer vervolgens de **gebruikers en licenties** optie.

    ![Pagina van de tenant CSP-abonnement](./media/site-recovery-manage-multi-tenancy-with-csp/users-and-licences.png)

    U kunt nu een nieuwe gebruiker maken door de relevante gegevens en machtigingen te selecteren of door de lijst met gebruikers in een CSV-bestand uploaden.

2. Nadat u hebt gemaakt dat een nieuwe gebruiker, gaat u terug naar de Azure portal en klik vervolgens op de **abonnement** blade, selecteert u het desbetreffende abonnement.

3. Selecteer op de blade die wordt geopend, **Access Control (IAM)**, en klik vervolgens op **toevoegen** een gebruiker met de relevante toegangsniveau toe te voegen.      
    De gebruikers die zijn gemaakt via de portal CSP worden automatisch weergegeven op de blade die wordt geopend nadat u een toegangsniveau op.

    ![Een gebruiker toevoegen](./media/site-recovery-manage-multi-tenancy-with-csp/add-user-subscription.png)

    Voor de meeste management bewerkingen de *Inzender* rol is voldoende. Gebruikers met dit toegangsniveau kunnen doen alles op een abonnement, behalve toegangsniveaus wijzigen (waarvoor *eigenaar*-toegang is vereist).

  Azure Site Recovery heeft ook drie [vooraf gedefinieerde gebruikersrollen](site-recovery-role-based-linked-access-control.md) kunnen worden gebruikt voor het verder beperken toegangsniveaus zoals vereist.

## <a name="next-steps"></a>Volgende stappen
  [Meer informatie](site-recovery-role-based-linked-access-control.md) over op rollen gebaseerde toegangsbeheer voor het beheren van Azure Site Recovery-implementaties.

  [Multitenant VMware-omgevingen beheren](site-recovery-multi-tenant-support-vmware-using-csp.md)
