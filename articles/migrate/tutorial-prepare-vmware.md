---
title: Virtuele VMware-machines voor evaluatie en migratie naar Azure met Azure Migrate voorbereiden | Microsoft Docs
description: Beschrijft hoe u om voor te bereiden voor evaluatie en migratie van on-premises VMware-machines naar Azure met Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 10f559295ff0598dea26fb30b089f020e2985889
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840345"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>Virtuele VMware-machines voorbereiden voor evaluatie en migratie naar Azure

In dit artikel wordt beschreven hoe u om voor te bereiden voor evaluatie en migratie van on-premises VMware-machines naar Azure, met behulp van [Azure Migrate](migrate-services-overview.md).

[Azure Migrate](migrate-overview.md) biedt een hub van hulpprogramma's die u helpen te detecteren, beoordelen en apps, infrastructuur en workloads migreren naar Microsoft Azure. De hub bevat Azure Migrate-hulpprogramma's en aanbiedingen van derden independent software vendor (ISV). 


In deze zelfstudie is de eerste in een serie die laat zien u hoe evalueren en migreren van virtuele VMware-machines. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Azure voorbereiden. Instellen van machtigingen voor uw Azure-account en resources voor het werken met Azure Migrate.
> * On-premises VMware-servers en virtuele machines voorbereiden voor de evaluatie van de virtuele machine.
> * On-premises VMware-servers en virtuele machines voorbereiden voor VM-migratie.

> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario, zodat u snel een proof-of-concept kunt instellen. Zelfstudies standaardopties gebruik waar mogelijk, en niet alle mogelijke instellingen en paden weergeven. Lees de instructies voor VMware-evaluatie en migratie voor gedetailleerde instructies.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.


## <a name="prepare-azure"></a>Azure voorbereiden

U hebt deze Azure-machtigingen nodig:

- Uw Azure-account moet machtigingen voor het maken van een Azure Migrate-project voor evaluatie en migratie. 
- Voor evaluatie en zonder agents migratie van virtuele VMware-machines voert Azure Migrate een lichtgewicht apparaat dat VM's worden gedetecteerd en de gegevens van de metagegevens en prestaties van de virtuele machine naar Azure Migrate wordt verzonden. In Azure moet u machtigingen voor het registreren van het apparaat Azure Migrate.
- Voor het migreren van virtuele VMware-machines met behulp van Azure Migrate-servermigratie, maakt Azure Migrate een Key Vault in de resourcegroep, om toegang tot sleutels naar de replicatie-storage-account in uw abonnement te beheren. Voor het maken van de kluis, moet u machtigingen voor capaciteitstoewijzingen rol maken voor de resourcegroep waarin de Azure Migrate-project zich bevindt. 


### <a name="assign-permissions-to-create-project"></a>Machtigingen toewijzen aan het project maken

1. In de Azure-portal, opent u het abonnement en selecteer **toegangsbeheer (IAM)** .
2. In **toegang controleren**, het relevante account zoeken en klikt u erop om machtigingen weer te geven.
3. U moet beschikken over **Inzender** of **eigenaar** machtigingen.
    - Als u een gratis Azure-account hebt gemaakt, u kunt de eigenaar van uw abonnement.
    - Als u niet de eigenaar bent, werkt u met de eigenaar van de rol toewijzen.

### <a name="assign-permissions-to-register-the-appliance"></a>Machtigingen toewijzen aan het apparaat registreren

Als u het apparaat Azure Migrate beoordelen of het uitvoeren van een zonder agent migratie van virtuele machines implementeert, moet u deze te registreren.

- Tijdens de registratie van het toestel maakt Azure Migrate twee apps in Azure Active Directory (Azure AD) die unieke identificatie van het apparaat
    - De eerste app communiceert met Azure Migrate-service-eindpunten.
    - De tweede app toegang heeft tot een Azure Key Vault gemaakt tijdens de registratie voor het opslaan van Azure AD-app-info en toestel configuratie-instellingen.
- U kunt machtigingen voor Azure Migrate te maken van deze Azure AD-apps met behulp van een van de volgende methoden:
    - Een tenant/globale beheerder kan machtigingen verlenen aan gebruikers in de tenant, maken en registreren van Azure AD-apps.
    - Een tenant/globale beheerder kan de rol van ontwikkelaar van toepassing (met de machtigingen) toewijzen aan het account.

Is het vermelden waard dat:

- De apps hebt geen andere toegangsmachtigingen voor het abonnement dan de hierboven beschreven.
- Deze machtigingen hoeft u alleen een nieuw apparaat te registreren. Nadat het apparaat is ingesteld, kunt u de machtigingen verwijderen. 


#### <a name="grant-account-permissions"></a>Accountmachtigingen verlenen

De tenant/globale beheerder kan als volgt machtigingen verlenen

1. De tenant/globale beheerder moet in Azure AD, navigeer naar **Azure Active Directory** > **gebruikers** > **gebruikersinstellingen**.
2. De beheerder moet ingesteld **App-registraties** naar **Ja**.

    ![Azure AD-machtigingen](./media/tutorial-prepare-vmware/aad.png)

> [!NOTE]
> Dit is een standaardinstelling die gevoelige niet. [Meer informatie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).



#### <a name="assign-application-developer-role"></a>Toepassingsontwikkelaar rol toewijzen 

De tenant/globale beheerder kan de rol van ontwikkelaar toewijzen aan een account. [Meer informatie](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

## <a name="assign-role-assignment-permissions"></a>Machtigingen voor capaciteitstoewijzingen rol toewijzen

Rolmachtigingen-toewijzing voor de resourcegroep waarin de Azure Migrate-project zich, als volgt bevindt toewijzen:

1. Selecteer in de resourcegroep in Azure portal, **toegangsbeheer (IAM)** .
2. In **toegang controleren**, het relevante account zoeken en klikt u erop om machtigingen weer te geven.

    - Om uit te voeren van server-evaluatie, **Inzender** machtigingen zijn voldoende.
    - Om uit te voeren zonder agent servermigratie, hebt u **eigenaar** (of **Inzender** en **Administrator voor gebruikerstoegang**) machtigingen.

3. Als u niet de vereiste machtigingen hebt, kunt u deze van de eigenaar van de resource-groep aanvragen. 



## <a name="prepare-for-vmware-vm-assessment"></a>Voorbereiden voor de evaluatie van VMware-VM

Om voor te bereiden voor de evaluatie van VMware-VM, moet u controleren of Hyper-V-host en VM-instellingen en controleer de instellingen voor de implementatie van het apparaat.

### <a name="verify-vmware-settings"></a>Controleer de instellingen voor VMware

1. [Controleer of](migrate-support-matrix-vmware.md#assessment-vmware-server-requirements) VMware-serververeisten voor evaluatie van de virtuele machine.
2. [Zorg ervoor dat](migrate-support-matrix-vmware.md#assessment-port-requirements) dat de vereiste poorten geopend op de vCenter-servers zijn.


### <a name="set-up-an-account-for-assessment"></a>Instellen van een account voor de beoordeling

Azure Migrate moet toegang hebben tot de vCenter-Server voor het detecteren van virtuele machines voor evaluatie en migratie zonder agent. Voor evaluatie moet u een alleen-lezen-account voor de vCenter-Server.

Als u een URL-gebaseerde firewall.proxy, toegang tot de vereiste toestaan [Azure-URL's](migrate-support-matrix-vmware.md#assessment-url-access-requirements).

Zorg ervoor dat de proxy wordt omgezet een CNAME-records hebt ontvangen tijdens het opzoeken van de URL's.


### <a name="verify-appliance-settings-for-assessment"></a>Controleer of de instellingen voor evaluatie van het toestel

Voordat het instellen van het apparaat Azure Migrate en evaluatie van het begin in de volgende zelfstudie, bereid u voor de implementatie van het apparaat.

1. Controleer of [controleren](migrate-support-matrix-vmware.md#assessment-appliance-requirements) vereisten voor het instellen van het apparaat Azure Migrate in VMware.
2. [Beoordeling](migrate-support-matrix-vmware.md#assessment-url-access-requirements) de Azure-URL's die het apparaat toegang nodig hebben.
3. Controleer de gegevens waarmee het apparaat worden verzameld tijdens de detectie en evaluatie.
4. [Houd er rekening mee](migrate-support-matrix-vmware.md#assessment-port-requirements) poort toegangsvereisten voor het apparaat.
5. U kunt het apparaat Azure Migrate implementeren als een VMware-VM met behulp van een OVA-bestand. Controleer of uw account heeft machtigingen voor het maken van een virtuele machine met behulp van een OVA-bestand op de vCenter-Server.


## <a name="prepare-for-agentless-vmware-migration"></a>Voorbereiden voor zonder agent VMware-migratie

Bekijk de vereisten voor de zonder agent migratie van virtuele VMware-machines.

1. [Beoordeling](migrate-support-matrix-vmware.md#agentless-migration-vmware-server-requirements) VMware-serververeisten voor migratie zonder agent.
2. Instellen van een account voor toegang tot de vCenter-Server met de [vereiste machtigingen](migrate-support-matrix-vmware.md#agentless-migration-vcenter-server-permissions) voor migratie zonder agent.
3. [Houd er rekening mee](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements) de vereisten voor VMware-VM's die u wilt migreren naar Azure met behulp van de migratie van de zonder agent.
4. [Beoordeling](migrate-support-matrix-vmware.md#agentless-migration-appliance-requirements) toestel vereisten voor migratie zonder agent.]
5. Houd er rekening mee toestel [tot URL's](migrate-support-matrix-vmware.md#agentless-migration-url-access-requirements) en [toegang tot poort](migrate-support-matrix-vmware.md#agentless-migration-port-requirements) vereisten voor migratie zonder agent.


## <a name="prepare-for-agent-based-vmware-migration"></a>Voorbereiden voor migratie van VMware op basis van een agent

Bekijk de vereisten voor migratie van virtuele VMware-machines op basis van een agent.

1. [Beoordeling](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements) VMware-serververeisten voor migratie zonder agent. 
2. Instellen van een account voor toegang tot de vCenter-Server met de [vereiste machtigingen](migrate-support-matrix-vmware.md#agent-based-migration-vcenter-server-permissions) voor migratie zonder agent.
3. [Houd er rekening mee](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) de vereisten voor VMware-VM's die u migreren naar Azure met behulp van de migratie op basis van een agent wilt, inclusief de installatie van de Mobility-service op elke virtuele machine die u wilt migreren.
4. Houd er rekening mee [tot URL's](migrate-support-matrix-vmware.md#agent-based-migration-url-access-requirements).
5. Beoordeling [toegang tot poort](migrate-support-matrix-vmware.md#agent-based-migration-port-requirements) vereisten voor de Mobility-service die wordt uitgevoerd op elke virtuele machine, en de configuratieserver Azure Migrate.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u:
 
> [!div class="checklist"] 
> * Azure-machtigingen instellen.
> * VMware voorbereid voor evaluatie en migratie.


Ga verder met de tweede zelfstudie voor het instellen van een Azure Migrate-project en VMware-VM's beoordelen voor migratie naar Azure.

> [!div class="nextstepaction"] 
> [VMware-VM's beoordelen](./tutorial-migrate-vmware.md) 

