---
title: Hyper-V virtuele machines voor evaluatie en migratie naar Azure met Azure Migrate voorbereiden | Microsoft Docs
description: Beschrijft hoe u om voor te bereiden voor evaluatie en migratie van Hyper-V-machines naar Azure met behulp van Azure Migrate.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 9e0d29770aa36f8e79bf08b7c5435ea2dbc4ae38
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840375"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>Voorbereiden voor evaluatie en migratie van Hyper-V-machines naar Azure

In dit artikel wordt beschreven hoe u om voor te bereiden voor evaluatie en migratie van on-premises Hyper-V-machines naar Azure met [Azure Migrate](migrate-services-overview.md).

[Azure Migrate](migrate-overview.md) biedt een hub van hulpprogramma's die u helpen te detecteren, beoordelen en apps, infrastructuur en workloads migreren naar Microsoft Azure. De hub bevat Azure Migrate-hulpprogramma's en aanbiedingen van derden independent software vendor (ISV). 

In deze zelfstudie is de eerste van een serie die laat zien u hoe u om te beoordelen en Hyper-V-machines migreren naar Azure. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Azure voorbereiden. Instellen van machtigingen voor uw Azure-account en resources voor het werken met Azure Migrate.
> * On-premises Hyper-V-hosts en virtuele machines voorbereiden voor server-evaluatie.
> * On-premises Hyper-V-hosts en virtuele machines voorbereiden voor servermigratie.


> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario, zodat u snel een proof-of-concept kunt instellen. Zelfstudies standaardopties gebruik waar mogelijk, en niet alle mogelijke instellingen en paden weergeven. Lees de instructies voor Hyper-V-evaluatie en migratie voor gedetailleerde instructies.


Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.


## <a name="prepare-azure"></a>Azure voorbereiden

### <a name="azure-permissions"></a>Azure-machtigingen

U moet een aantal toestemming voor het implementeren van Azure Migrate:

- Uw Azure-account moet machtigingen voor het maken van een Azure Migrate-project voor evaluatie en migratie. 
- Uw Azure-account moet machtigingen voor het registreren van het apparaat Azure Migrate.
    - Voor evaluatie voert Azure Migrate een lichtgewicht apparaat dat Hyper-V VM's worden gedetecteerd en de gegevens van de metagegevens en prestaties van de virtuele machine naar Azure Migrate wordt verzonden.
    - Tijdens de registratie van apparaat maakt u twee apps in Azure Active Directory (Azure AD) die unieke identificatie van het apparaat Azure Migrate:
        - De eerste app communiceert met Azure Migrate-service-eindpunten.
        - De tweede app toegang heeft tot een Azure Key Vault gemaakt tijdens de registratie voor het opslaan van Azure AD-app-info en toestel configuratie-instellingen.
    - U kunt machtigingen voor Azure Migrate te maken van deze Azure AD-apps met behulp van een van de volgende methoden:
        - Een tenant/globale beheerder kan machtigingen verlenen aan gebruikers in de tenant, maken en registreren van Azure AD-apps.
        - Een tenant/globale beheerder kan de rol van ontwikkelaar van toepassing (met de machtigingen) toewijzen aan het account.
    - Is het vermelden waard dat:
        - De apps hebt geen andere toegangsmachtigingen voor het abonnement dan de hierboven beschreven.
        - Deze machtigingen hoeft u alleen een nieuw apparaat te registreren. Nadat het apparaat is ingesteld, kunt u de machtigingen verwijderen. 


### <a name="assign-permissions-to-create-project"></a>Machtigingen toewijzen aan het project maken

Controleer machtigingen voor het maken van een Azure Migrate-project.

1. In de Azure-portal, opent u het abonnement en selecteer **toegangsbeheer (IAM)** .
2. In **toegang controleren**, het relevante account zoeken en klikt u erop om machtigingen weer te geven.
3. U moet beschikken over **Inzender** of **eigenaar** machtigingen.
    - Als u een gratis Azure-account hebt gemaakt, u kunt de eigenaar van uw abonnement.
    - Als u niet de eigenaar bent, werkt u met de eigenaar van de rol toewijzen.


### <a name="assign-permissions-to-register-the-appliance"></a>Machtigingen toewijzen aan het apparaat registreren

Als u het apparaat Azure Migrate voor de evaluatie van virtuele machines implementeert, moet u deze te registreren.

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

De tenant/globale beheerder kan machtigingen als volgt:

1. De tenant/globale beheerder moet in Azure AD, navigeer naar **Azure Active Directory** > **gebruikers** > **gebruikersinstellingen**.
2. De beheerder moet ingesteld **App-registraties** naar **Ja**.

    ![Azure AD-machtigingen](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Dit is een standaardinstelling die gevoelige niet. [Meer informatie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).



#### <a name="assign-application-developer-role"></a>Toepassingsontwikkelaar rol toewijzen 

De tenant/globale beheerder kan de rol van ontwikkelaar toewijzen aan een account. [Meer informatie](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).


## <a name="prepare-for-hyper-v-assessment"></a>Evaluatie van de Hyper-V voorbereiden

Voorbereiden voor de evaluatie van de Hyper-V, controleert u of Hyper-V-host en VM-instellingen en controleer de instellingen voor de implementatie van het apparaat.

### <a name="verify-hyper-v-host-settings"></a>Controleer de instellingen voor Hyper-V-host

1. Controleer of [vereisten voor Hyper-V-host](migrate-support-matrix-hyper-v.md#assessment-hyper-v-host-requirements) voor server-evaluatie.
2. Zorg ervoor dat de [poorten vereist](migrate-support-matrix-hyper-v.md#assessment-port-requirements) op Hyper-V-hosts zijn geopend.

### <a name="enable-powershell-remoting-on-hosts"></a>PowerShell voor externe toegang op hosts inschakelen

PowerShell voor externe toegang op elke host als volgt instellen:

1. Open op elke host een PowerShell-console als beheerder.
2. Voer deze opdracht uit:

    ```
    Enable-PSRemoting -force
    ```

### <a name="enable-credssp-on-hosts"></a>CredSSP inschakelen op hosts

Als VM-schijven zich op SMB-shares bevinden, voltooien van deze stap op elke relevante Hyper-V-host. Deze stap wordt gebruikt voor het detecteren van configuratie-informatie voor Hyper-V-machines met schijven op SMB-shares. Als u geen VM-schijven op SMB-shares, kunt u de stap overslaan.

1. Identificeer de Hyper-V-hosts waarop Hyper-V-machines met schijven op SMB-shares.
2. Voer de volgende opdracht uit op elke geïdentificeerde Hyper-V-host:

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

- CredSSP-verificatie kunt de Hyper-V-host om te delegeren referenties namens de client Azure Migrate.
- U kunt deze opdracht op afstand uitvoeren op alle Hyper-V-hosts.
- Als u nieuwe hostknooppunten toevoegen op een cluster worden ze automatisch toegevoegd voor detectie, maar moet u handmatig CredSSP inschakelen op de nieuwe knooppunten, indien nodig.

### <a name="verify-appliance-settings"></a>Controleer of de instellingen van het toestel

Voordat het instellen van het apparaat Azure Migrate en evaluatie van het begin in de volgende zelfstudie, bereid u voor de implementatie van het apparaat.

1. [Controleer of](migrate-support-matrix-hyper-v.md#assessment-appliance-requirements) toestel vereisten.
2. [Beoordeling](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access) de Azure-URL's die het apparaat toegang nodig hebben.
3. Controleer de gegevens waarmee het apparaat worden verzameld tijdens de detectie en evaluatie.
4. [Houd er rekening mee](migrate-support-matrix-hyper-v.md#assessment-port-requirements) poort toegangsvereisten voor het apparaat.


### <a name="set-up-an-account-for-vm-discovery"></a>Instellen van een account voor VM-detectie

Azure Migrate moet toegangsmachtigingen voor het detecteren van on-premises VM's.

- Instellen van een domein of een lokaal gebruikersaccount met beheerdersmachtigingen op de Hyper-V-hosts/het cluster.

    - U hebt één account nodig voor alle hosts en clusters die u wilt opnemen in de detectie.
    - Het account kan een lokaal account of domeinaccount zijn. Het is raadzaam om de beheerdersmachtigingen op de Hyper-V-hosts of clusters.
    - U kunt ook als u niet wilt dat de beheerdersmachtigingen toe te wijzen, zijn de volgende machtigingen nodig:
        - Gebruikers van extern beheer
        - Hyper-V-Administrators
        - Prestatiemetergebruikers

### <a name="enable-integration-services-on-vms"></a>Integratieservices op virtuele machines inschakelen

Integratieservices moeten zijn ingeschakeld op elke virtuele machine, zodat Azure Migrate kunt u informatie over het besturingssysteem op de virtuele machine vastleggen.

- Schakel op de virtuele machines die u wilt detecteren en evalueren, [Hyper-V-integratieservices](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) op elke virtuele machine. 

## <a name="prepare-for-hyper-v-migration"></a>Voorbereiden voor migratie van Hyper-V

1. [Beoordeling](migrate-support-matrix-hyper-v.md#migration-hyper-v-host-requirements) Hyper-V-host-vereisten voor migratie.
2. [Beoordeling](migrate-support-matrix-hyper-v.md#migration-hyper-v-vm-requirements) de vereisten voor Hyper-V-machines die u wilt migreren naar Azure.
3. [Houd er rekening mee](migrate-support-matrix-hyper-v.md#migration-hyper-v-host-url-access) de Azure-URL's naar welke Hyper-V-hosts en clusters toegang nodig voor VM-migratie.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u:
 
> [!div class="checklist"] 
> * Instellen van machtigingen voor Azure-account.
> * Voorbereide Hyper-V-hosts en virtuele machines voor evaluatie en migratie.

Doorgaan naar de volgende zelfstudie voor het maken van een Azure Migrate-project, Hyper-V virtuele machines en beoordelen voor migratie naar Azure

> [!div class="nextstepaction"] 
> [Hyper-V VM's beoordelen](./tutorial-assess-hyper-v.md) 
