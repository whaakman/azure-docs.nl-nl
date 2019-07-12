---
title: Migrate on-premises virtuele VMware-machines naar Azure met Azure Migrate servermigratie op basis van een agent | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een migratie op basis van een agent van on-premises machines naar Azure met Azure Migrate servermigratie uitvoeren
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 78edd22502bbf08fda8d1828413bbc0b0f8aa9c7
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809335"
---
# <a name="migrate-vmware-vms-to-azure-agent-based"></a>Virtuele VMware-machines migreren naar Azure (agent-indeling)

Dit artikel ziet u hoe u on-premises VMware-machines migreren naar Azure, met behulp van de migratie op basis van de agent met het hulpprogramma voor migratie van Azure-Server migreren.

[Azure Migrate](migrate-services-overview.md) biedt een centraal punt voor het volgen van detectie, beoordeling en migratie van uw on-premises toepassingen en workloads en AWS/GCP VM-instanties naar Azure. De hub biedt hulpprogramma's voor Azure Migrate voor evaluatie en migratie, evenals van derden independent software vendor (ISV)-aanbiedingen.


In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * De bronomgeving instellen en implementeren van een apparaat van de replicatie Azure Migrate voor migratie op basis van een agent.
> * Instellen van de doelomgeving voor migratie.
> * Een replicatiebeleid instellen.
> * Replicatie inschakelen.
> * Een testmigratie om te controleren of dat alles werkt zoals verwacht worden uitgevoerd.
> * Voer een een volledige migratie naar Azure.

> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario, zodat u snel een proof-of-concept kunt instellen. Zelfstudies standaardopties gebruik waar mogelijk, en niet alle mogelijke instellingen en paden weergeven. Bekijk de uitleg voor VMware-evaluatie en migratie voor gedetailleerde instructies.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.

## <a name="before-you-begin"></a>Voordat u begint

Het is raadzaam dat u nu VMware VM-evaluatie met Azure Migrate-Server-evaluatie, uitproberen voordat u virtuele machines naar Azure migreren. Stel een evaluatie als volgt in:

1. Volg de zelfstudie om te [Azure en VMware voorbereiden](tutorial-prepare-vmware.md) voor evaluatie.
2. Volg daarna [in deze zelfstudie](tutorial-assess-vmware.md) een apparaat Azure Migrate voor evaluatie, instellen en detecteren en evalueren van virtuele machines.


Hoewel u wordt aangeraden dat u een beoordeling probeert, hebt u geen een evaluatie uitvoeren voordat u virtuele machines migreren.

## <a name="migration-methods"></a>Migratiemethoden

U kunt virtuele VMware-machines migreren naar Azure met behulp van het hulpprogramma voor migratie van Azure-Server migreren. Dit hulpprogramma biedt een aantal opties voor VMware-VM-migratie:

- Zonder agent replicatie. Virtuele machines migreren zonder dat u hoeft te installeren op deze.
- De migratie op basis van een agent. of replicatie. Een agent (de Mobility-services-agent) installeren op de virtuele machine voor replicatie.

Om te beslissen of u wilt gebruiken van de migratie zonder agent of op basis van een agent, lees deze artikelen:

- [Meer informatie over](server-migrate-overview.md) de opties voor het migreren van VMware.
- [De beperkingen bekijken](server-migrate-overview.md#agentless-migration-limitations) voor migratie zonder agent.
- [Ga als volgt in dit artikel](tutorial-migrate-vmware.md) voor het uitproberen van migratie zonder agent.



## <a name="prerequisites"></a>Vereisten

Voordat u aan deze zelfstudie begint, dient u eerst:

1. [Beoordeling](migrate-architecture.md) de architectuur van de migratie van VMware.
2. Zorg ervoor dat uw Azure-account de rol Inzender voor virtuele machines is toegewezen zodat u machtigingen hebben voor:

    - Het maken van een VM in de geselecteerde resourcegroep.
    - Het maken van een VM in het geselecteerde virtuele netwerk.
    - Schrijven naar een door Azure beheerde schijf. 

3. [Instellen van een Azure-netwerk](../virtual-network/manage-virtual-network.md#create-a-virtual-network). On-premises machines worden gerepliceerd naar Azure beheerde schijven. Als u een failover naar Azure voor migratie, worden virtuele Azure-machines gemaakt op basis van deze beheerde schijven en toegevoegd aan een Azure-netwerk die u opgeeft bij het instellen van de migratie.


## <a name="prepare-azure"></a>Azure voorbereiden

Als u hebt al een evaluatie uitgevoerd met Azure Migrate-Server-evaluatie, kunt u de instructies in deze sectie overslaan omdat u al deze stappen hebt voltooid. 

Als u een evaluatie nog niet hebt uitgevoerd, moet u de Azure-machtigingen instellen voordat u met Azure Migrate-servermigratie migreren kunt.

- **Maak een project**: Uw Azure-account moet machtigingen voor het maken van een Azure Migrate-project. 
- **Registreren van het apparaat Azure Migrate replicatie**: De replicatie-toepassing maakt en registreert u een Azure Active Directory-app in uw Azure-account. U wilt het overdragen van machtigingen voor deze.
- **Key Vault maken**: Voor het migreren van virtuele VMware-machines met behulp van Azure Migrate-servermigratie, maakt Azure Migrate een Key Vault in de resourcegroep, om toegang tot sleutels naar de replicatie-storage-account in uw abonnement te beheren. Voor het maken van de kluis, moet u machtigingen voor capaciteitstoewijzingen rol maken voor de resourcegroep waarin de Azure Migrate-project zich bevindt. 


### <a name="assign-permissions-to-create-project"></a>Machtigingen toewijzen aan het project maken

1. In de Azure-portal, opent u het abonnement en selecteer **toegangsbeheer (IAM)** .
2. In **toegang controleren**, het relevante account zoeken en klikt u erop om machtigingen weer te geven.
3. U moet beschikken over **Inzender** of **eigenaar** machtigingen.
    - Als u een gratis Azure-account hebt gemaakt, u kunt de eigenaar van uw abonnement.
    - Als u niet de eigenaar bent, werkt u met de eigenaar van de rol toewijzen.

### <a name="assign-permissions-to-register-the-replication-appliance"></a>Machtigingen toewijzen aan de replicatie-apparaat registreren

Voor de migratie op basis van een agent overdragen van machtigingen voor Azure Migrate servermigratie maken en registreren van een Azure AD-app in uw account. U kunt machtigingen met behulp van een van de volgende methoden:

- Een tenant/globale beheerder kan machtigingen verlenen aan gebruikers in de tenant, maken en registreren van Azure AD-apps.
- Een tenant/globale beheerder kan de rol van ontwikkelaar van toepassing (met de machtigingen) toewijzen aan het account.

Is het vermelden waard dat:

- De apps hebt geen andere toegangsmachtigingen voor het abonnement dan de hierboven beschreven.
- Deze machtigingen hoeft u alleen een nieuwe replicatie-apparaat te registreren. Nadat de replicatie-apparaat is ingesteld, kunt u de machtigingen verwijderen. 


#### <a name="grant-account-permissions"></a>Accountmachtigingen verlenen

De tenant/globale beheerder kan als volgt machtigingen verlenen

1. De tenant/globale beheerder moet in Azure AD, navigeer naar **Azure Active Directory** > **gebruikers** > **gebruikersinstellingen**.
2. De beheerder moet ingesteld **App-registraties** naar **Ja**.

    ![Azure AD-machtigingen](./media/tutorial-prepare-vmware/aad.png)

> [!NOTE]
> Dit is een standaardinstelling die gevoelige niet. [Meer informatie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

#### <a name="assign-application-developer-role"></a>Toepassingsontwikkelaar rol toewijzen 

De tenant/globale beheerder kan de rol van ontwikkelaar toewijzen aan een account. [Meer informatie](https://docs.microsoft.comazure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

## <a name="assign-permissions-to-create-key-vault"></a>Toewijzen van machtigingen voor het maken van Key Vault

Rolmachtigingen-toewijzing voor de resourcegroep waarin de Azure Migrate-project zich, als volgt bevindt toewijzen:

1. Selecteer in de resourcegroep in Azure portal, **toegangsbeheer (IAM)** .
2. In **toegang controleren**, het relevante account zoeken en klikt u erop om machtigingen weer te geven. U moet **eigenaar** (of **Inzender** en **Administrator voor gebruikerstoegang**) machtigingen.
3. Als u niet de vereiste machtigingen hebt, kunt u deze van de eigenaar van de resource-groep aanvragen. 


## <a name="prepare-on-premises-vmware"></a>On-premises VMware voorbereiden

### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Azure Migrate-Server-migratie moet toegang hebben tot de VMware-servers:

- Virtuele machines automatisch detecteren. Er is minimaal een alleen-lezen-account vereist.
- Replicatie, failover en failback orkestreren. U hebt een account nodig dat bewerkingen kan uitvoeren als het maken en verwijderen van schijven en het inschakelen van VM’s.

Ga als volgt te werk om het account te maken:

1. Als u een vast account wilt gebruiken, maakt u een rol op vCenter-niveau. Geef de rol een naam zoals **Azure_Site_Recovery**.
2. Wijs de rol de rechten toe die worden samengevat in onderstaande tabel.
3. Maak een gebruiker op de vCenter-server of vSphere-host. Wijs de rol toe aan de gebruiker.

#### <a name="vmware-account-permissions"></a>Machtigingen voor VMware-account

**Taak** | **Rol/machtigingen** | **Details**
--- | --- | ---
**VM-detectie** | Ten minste een alleen-lezen-gebruiker<br/><br/> Datacentrumobject –> doorgeven naar onderliggend object, rol=Alleen-lezen | Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.<br/><br/> Wijs om de toegang te beperken de rol **Geen toegang** met het object **Doorgeven naar onderliggend object** toe aan de onderliggende objecten (vSphere-hosts, gegevensopslag, VM’s en netwerken).
**Volledige replicatie, failover en failback** |  Maak een rol (Azure_Site_Recovery) met de vereiste machtigingen en wijs de rol toe aan een VMware-gebruiker of -groep<br/><br/> Datacentrumobject –> Doorgeven naar onderliggend object, rol=Azure_Site_Recovery<br/><br/> Gegevensopslag –> Ruimte toewijzen, browsen in de gegevensopslag, bestandsbewerkingen op laag niveau, bestand verwijderen, bestanden van virtuele machines bijwerken<br/><br/> Netwerk –> Netwerk toewijzen<br/><br/> Resource –> VM toewijzen aan resourcegroep, uitgeschakelde VM migreren, ingeschakelde VM migreren<br/><br/> Taken –> Taak maken, taak bijwerken<br/><br/> Virtuele machine –> Configuratie<br/><br/> Virtuele machine –> Interactie –> vraag beantwoorden, apparaatverbinding, CD's configureren, diskettes configureren, uitschakelen, inschakelen, VMware-hulpprogramma's installeren<br/><br/> Virtuele machine -> Inventaris –> Maken, registreren, registratie opheffen<br/><br/> Virtuele machine –> Inrichten –> Downloaden van virtuele machine toestaan, toestaan dat bestanden van virtuele machines worden geüpload<br/><br/> Virtuele machine –> Momentopnamen -> Momentopnamen verwijderen | Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.<br/><br/> Wijs om de toegang te beperken de rol **Geen toegang** met het object **Doorgeven naar onderliggend object** toe aan de onderliggende objecten (vSphere-hosts, gegevensopslag, VM’s en netwerken).

### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet zijn geïnstalleerd op de machines die u wilt repliceren.

- Het apparaat Azure Migrate-replicatie een push-installatie van deze service kunt doen wanneer u replicatie voor een machine inschakelt, of kunt u deze handmatig installeren of met behulp van hulpprogramma's voor installatie.
- In deze zelfstudie gaat u de Mobility-service installeren met een push-installatie.
- Voor push-installatie moet u een account voorbereiden waarmee Azure Migrate-servermigratie gebruiken kunt voor toegang tot de virtuele machine.

U bereidt het account als volgt voor:

1. Bereid een domein of lokaal account met machtigingen voor om op de VM te installeren.
2. Voor Windows-VM's, als u niet een domeinaccount, schakel toegangsbeheer voor externe gebruikers op de lokale computer door de DWORD-vermelding toe te voegen **LocalAccountTokenFilterPolicy**, met de waarde in het register, onder **HKEY_ LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**
3. Bereid een root-account op de bronserver voor Linux voor virtuele Linux-machines.


### <a name="check-vmware-requirements"></a>Vereisten voor VMware controleren

Zorg ervoor dat VMware-servers en virtuele machines te voldoen aan de vereisten voor migratie naar Azure. 


> [!NOTE]
> Migratie op basis van een agent met Azure Migrate-servermigratie is gebaseerd op de functies van de Azure Site Recovery-service. Als aan bepaalde vereisten koppelen aan de documentatie voor Site Recovery.

1. [Verifieer](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements) de vereisten voor VMware-servers.
2. [Controleer of](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) VM vereisten voor migratie ondersteunen.
3. Controleer de instellingen van de virtuele machine. On-premises machines u naar Azure repliceren moeten voldoen aan [vereisten voor Azure VM's](migrate-support-matrix-vmware.md#azure-vm-requirements).



## <a name="add-the-azure-migrate-server-migration-tool"></a>Het hulpprogramma voor migratie van Azure Migrate-Server toevoegen

Als u de zelfstudie voor het evalueren van virtuele VMware-machines niet volgt, instellen van een Azure Migrate-project en voegt u het hulpprogramma voor migratie van Azure-Server migreren:

1. In Azure portal > **alle services**, zoeken naar **Azure Migrate**.
2. Onder **Services**, selecteer **Azure Migrate**.

    ![Instellen van Azure Migrate](./media/tutorial-migrate-vmware-agent/azure-migrate-search.png)

3. In **overzicht**, klikt u op **evalueren en migreren van servers**.
4. Onder **ontdekken, beoordelen en servers migreren**, klikt u op **evalueren en migreren van servers**.

    ![Detecteren en evalueren van servers](./media/tutorial-migrate-vmware-agent/assess-migrate.png)

1. In **ontdekken, beoordelen en servers migreren**, klikt u op **toevoegen van extra**.
2. In **Migrate-project**, selecteer uw Azure-abonnement en maak een resourcegroep als u dit niet hebt.
3. In **projectdetails**, geef de naam van het project en het geografische gebied waarin u wilt maken van het project en klik op **volgende**

    ![Een Azure Migrate-project maken](./media/tutorial-migrate-vmware-agent/migrate-project.png)

    U kunt een Azure Migrate-project maken in een van deze locaties.

    **Geografie** | **Regio**
    --- | ---
    Azië | Azië - zuidoost
    Europa | Europa - noord of Europa - west
    Verenigde Staten | US - oost of US - west-centraal

    De opgegeven geografie voor het project wordt alleen gebruikt om de metagegevens op te slaan die zijn verzameld van on-premises virtuele machines. U kunt een doelregio voor de daadwerkelijk migratie selecteren.
4. In **Selecteer assessment tool**, selecteer **toevoegen van een hulpprogramma voor upgradebeoordeling voorlopig overslaan** > **volgende**.
5. In **hulpprogramma voor migratie selecteren**, selecteer **Azure Migrate: Servermigratie** > **volgende**.
6. In **bekijken en toevoegen van extra**, Controleer de instellingen en klik op **hulpprogramma's toevoegen**
7. Na het toevoegen van het hulpprogramma, wordt deze weergegeven in de Azure Migrate-project > **Servers** > **hulpprogramma voor migratie van**.

## <a name="set-up-the-replication-appliance"></a>De replicatie-apparaat instellen

De eerste stap van de migratie is het instellen van het apparaat van de replicatie. Het apparaat van de replicatie is een enkele, maximaal beschikbaar, on-premises VMware-VM die als host fungeert voor deze onderdelen:

- **Configuratieserver**: De configuratieserver coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.
- **Processerver**: De processerver fungeert als replicatiegateway. Deze ontvangt replicatiegegevens; optimaliseert met caching, compressie en codering, en verzendt ze naar een cache-opslagaccount in Azure. De processerver installeert ook de Mobility-Service-agent op virtuele machines die u repliceren wilt, en detecteert automatisch on-premises virtuele VMware-machines.


Als u de replicatie-apparaat instelt, moet u een voorbereide Open Virtualization-toepassing (OVA)-sjabloon downloaden. U importeert u de sjabloon in VMware en de virtuele machine van de replicatie-toepassing maken. 

### <a name="download-the-replication-appliance-template"></a>De replicatie toestel-sjabloon downloaden

Download de sjabloon als volgt:

1. Klik in de Azure Migrate-project op **Servers** onder **migratie doelstellingen**.
2. In **Azure Migrate - Servers** > **Azure Migrate: Servermigratie**, klikt u op **ontdekken**.

    ![VM's detecteren](./media/tutorial-migrate-vmware-agent/migrate-discover.png)

3. In **machines detecteren** > **zijn de machines gevirtualiseerd?** , klikt u op **Ja, met VMWare vSphere-hypervisor**.
4. In **hoe wilt u migreren?** , selecteer **met behulp van replicatie op basis van een agent**.
5. In **doelregio**, selecteert u de Azure-regio waarnaar u wilt migreren van de machines.
6. Selecteer **bevestigen dat de doelregio voor migratie-regionaam**.
7. Klik op **resources maken**. Hiermee maakt u een Azure Site Recovery-kluis op de achtergrond.
    - U kunt de doelregio voor dit project niet wijzigen nadat u op deze knop te klikken.
    - Alle toekomstige migraties worden naar deze regio.

    ![Een Recovery Services-kluis maken](./media/tutorial-migrate-vmware-agent/create-resources.png)

8. In **wilt u een nieuwe replicatie-apparaat installeren?** , selecteer **installeren van een apparaat replicatie**.
9. Klik op **downloaden**om te downloaden van het apparaat van de replicatie. Hiermee downloadt u een OVF-sjabloon die u gebruiken voor het maken van een nieuwe VMware-VM die het apparaat wordt uitgevoerd.
    ![OVA downloaden](./media/tutorial-migrate-vmware-agent/download-ova.png)
10. Noteer de naam van de resourcegroep en de Recovery Services-kluis. U hebt deze tijdens de implementatie van het apparaat nodig.


### <a name="import-the-template-in-vmware"></a>De sjabloon in VMware importeren

Na het downloaden van de OVF-sjabloon, importeert u het in VMware om u te maken van de replicatie-toepassing op een VMware-VM met Windows Server 2016.

1. Meld u aan bij de VMware vCenter-server of vSphere ESXi-host met behulp van de VMWare vSphere-client.
2. Selecteer in het menu **Bestand** de optie **OVF-sjabloon implementeren** om de **wizard voor het implementeren van OVF-sjablonen** te starten. 
3. In **bron selecteren**, geef de locatie van de gedownloade OVF.
4. In **Beoordelingsdetails**, selecteer **volgende**.
5. In **naam en map selecteren** en **configuratie selecteren**, accepteer de standaardinstellingen.
6. In **opslag selecteren** > **indeling virtuele schijf selecteren**, voor optimale prestaties selecteert **Thick Provision Eager Zeroed**.
7. Accepteer de standaardinstellingen in de rest van de wizardpagina's.
8. In **klaar om te voltooien**, voor het instellen van de virtuele machine met de standaardinstellingen, selecteer **inschakelen na de implementatie** > **voltooien**.

   > [!TIP]
   > Als u een extra NIC wilt toevoegen, wist u **Inschakelen na de implementatie** > **Voltooien**. De sjabloon bevat standaard één NIC. Na de implementatie kunt u meer NIC’s toevoegen.

### <a name="kick-off-replication-appliance-setup"></a>Een vliegende start replicatie apparaat instellen

1. Schakel de VM in vanuit de VMWare vSphere Client-console.
2. De VM wordt opgestart in een Windows Server 2016-installatie-ervaring. Accepteer de gebruiksrechtovereenkomst en voer een Administrator-wachtwoord in.
3. Nadat de installatie is voltooid, meld u aan de virtuele machine als de beheerder, met behulp van het beheerderswachtwoord.
4. De eerste keer dat u zich aanmeldt, wordt de replicatie toestel setup-hulpprogramma (Azure Site Recovery Configuration Tool) begint binnen een paar seconden.
5. Voer een naam te gebruiken voor het registreren van het apparaat met de migratie van Azure-Server migreren. Klik op **Volgende**.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, selecteert u **Aanmelden** om u aan te melden bij uw Azure-abonnement.
7. Wacht totdat het hulpprogramma voor het registreren van een Azure AD-app voor het identificeren van het apparaat. Het apparaat opnieuw wordt opgestart.
1. Meld u opnieuw aan bij de machine. Na enkele seconden wordt de wizard voor het beheer van de configuratieserver automatisch gestart.

### <a name="register-the-replication-appliance"></a>De replicatie-apparaat registreren

Einddatum instellen en het replicatie-apparaat te registreren.

1. Selecteer in de configuratie van Wizard voor het beheer, **connectiviteit instellen**.
2. Selecteer de NIC (standaard is er slechts één NIC) dat de replicatie-apparaat gebruikt voor VM-detectie en voor het doen van een push-installatie van de Mobility-service op de bronmachines.
3. Selecteer de NIC die het replicatie-apparaat voor connectiviteit met Azure gebruikt. Selecteer vervolgens **Opslaan**. U kunt deze instelling niet wijzigen nadat deze geconfigureerd.
4. Als het apparaat zich achter een proxyserver bevindt, moet u de proxy-instellingen opgeven.
    - Geef de naam van de proxyserver als **http://ip-address** , of **http://FQDN** . HTTPS-proxy-servers worden niet ondersteund.
5. Wanneer u hierom wordt gevraagd voor het abonnement, resourcegroepen en de details van de kluis, toevoegen de details die u hebt genoteerd wanneer u de apparaat-sjabloon hebt gedownload.
6. Accepteer de gebruiksrechtovereenkomst in **Software van derden installeren**. Selecteer **Downloaden en installeren** om MySQL Server te installeren.
7. Selecteer **VMware PowerCLI installeren**. Zorg ervoor dat alle browservensters zijn gesloten voordat u dit doet. Selecteer vervolgens **Doorgaan**.
8. In **De configuratie van het apparaat valideren** worden de vereisten gecontroleerd voordat u doorgaat.
9. In **vCenter Server vSphere/ESXi-server configureren** voert u de FQDN of het IP-adres van de vCenter-server, of vSphere-host, in waar de VM's die u wilt repliceren zich bevinden. Voer de poort in waarop de server luistert. Voer een beschrijvende naam in voor de VMware-server in de kluis.
10. Voer de referenties voor het account dat u [gemaakt](#prepare-an-account-for-automatic-discovery) voor VMware-detectie. Selecteer **toevoegen** > **blijven**.
11. In **referenties voor virtuele machine configureren**, voert u de referenties u [gemaakt](#prepare-an-account-for-mobility-service-installation) voor push-installatie van de Mobility-service wanneer u replicatie voor virtuele machines inschakelt.  
    - Voor Windows-machines moet het account lokale administrator-machtigingen hebben op de machines die u wilt repliceren.
    - Geef voor Linux de details voor de superuser op.
12. Selecteer **Configuratie voltooien** om de registratie te voltooien.


Nadat de replicatie-apparaat is geregistreerd, Azure Migrate-evaluatie-Server maakt verbinding met de VMware-servers met de opgegeven instellingen en VM's worden gedetecteerd. U kunt de gedetecteerde virtuele machines in weergeven **beheren** > **zijn items gedetecteerd**, in de **andere** tabblad.


## <a name="replicate-vms"></a>Virtuele machines repliceren

1. In de Azure Migrate-project > **Servers**, **Azure Migrate: Servermigratie**, klikt u op **repliceren**.

    ![Virtuele machines repliceren](./media/tutorial-migrate-vmware-agent/select-replicate.png)

2. In **repliceren**, > **instellingen van de gegevensbron** > **zijn de machines gevirtualiseerd?** , selecteer **Ja, met VMware vSphere**.
3. In **On-premises apparaat**, selecteert u de naam van het apparaat Azure Migrate, die u hebt ingesteld.
4. In **vCenter-server**, geef de naam van de vCenter-server beheren van de VM's of de vSphere-server waarop de virtuele machines worden gehost.
5. In **processerver**, selecteert u de naam van het apparaat van de replicatie.
6. In **Gast referenties**, geef het beheerdersaccount van de virtuele machine die wordt gebruikt voor push-installatie van de Mobility-service. Klik vervolgens op **volgende: Virtuele machines**.

    ![Virtuele machines repliceren](./media/tutorial-migrate-vmware-agent/source-settings.png)

7. In **virtuele Machines**, selecteer de machines die u wilt repliceren.

    - Als u hebt een evaluatie uitgevoerd voor de virtuele machines, kunt u de grootte van de virtuele machine en de schijf type (premium of standaard) aanbevelingen van de resultaten van de evaluatie kunt toepassen. Om dit te doen, in **migratie-instellingen importeren uit een Azure Migrate-evaluatie?** , selecteer de **Ja** optie.
    - Als u een evaluatie niet hebt uitgevoerd, of als u niet wilt gebruiken van de evaluatie-instellingen, selecteert u de **Nee** opties.
    - Als u gebruik van de evaluatie hebt geselecteerd, selecteert u de VM-groep en de naam van de evaluatie.

8. Controleer elke virtuele machine die u wilt migreren. Klik vervolgens op **volgende: Instellingen voor doel**.
9. In **doel instellingen**, selecteert u het abonnement en doelregio waarnaar u migreert, en geef de resourcegroep waarin de Azure VM's worden geplaatst die na de migratie.
10. In **Virtueelnetwerk**, selecteert u het Azure VNet/subnet waarop de Azure VM's na de migratie wordt gekoppeld.
11. In **Azure Hybrid Benefit**:

    - Selecteer **Nee** als u niet wilt dat Azure Hybrid Benefit van toepassing. Klik op **Volgende**.
    - Selecteer **Ja** als u Windows Server-machines die zijn gedekt door een actieve Software Assurance- of Windows Server-abonnementen hebt en u wilt het voordeel toepassen op de computers waarnaar u migreert. Klik op **Volgende**.

12. In **Compute**, Controleer de VM-naam, de grootte, de OS-schijftype en de beschikbaarheidsset. Virtuele machines moeten voldoen aan [Azure-vereisten](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements).

    - **VM-grootte**: Als u aanbevelingen voor de evaluatie, bevat de VM-grootte vervolgkeuzelijst de aanbevolen grootte. Azure Migrate kiest anders een grootte op basis van het meest overeenkomt met in de Azure-abonnement. U kunt ook kiezen een handmatige grootte in **Azure VM-grootte**. 
    - **Besturingssysteemschijf**: Geef de besturingssysteemschijf (Start) voor de virtuele machine. De besturingssysteemschijf is de schijf met de bootloader van besturingssysteem en het installatieprogramma. 
    - **Beschikbaarheidsset**: Als de virtuele machine moet zich in een Azure-beschikbaarheidsset instellen na de migratie, geeft u de set. De set moet zich in de doelresourcegroep die u voor de migratie opgeeft.

13. In **schijven**, opgeven of de VM-schijven moeten worden gerepliceerd naar Azure en selecteer het schijftype (standaard SSD/HDD- of premium beheerde schijven) in Azure. Klik op **Volgende**.
    - U kunt schijven uitsluiten van replicatie.
    - Als u schijven uitsluit, zich niet aanwezig zijn op de virtuele Azure-machine na de migratie. 

14. In **bekijken en replicatie starten**, Controleer de instellingen en klik op **repliceren** starten van de initiële replicatie voor de servers.

> [!NOTE]
> U kunt replicatie-instellingen bijwerken voordat u begint de replicatie, **beheren** > **machines repliceren**. Instellingen kunnen niet worden gewijzigd nadat replicatie is gestart.




## <a name="track-and-monitor"></a>Bijhouden en controleren

- Wanneer u klikt op **repliceren** wordt een taak Start replicatie gestart. 
- Wanneer de taak Start replicatie voltooid is, begint de machines in de initiële replicatie naar Azure.
- Nadat de initiële replicatie is voltooid, begint de replicatie van verschillen. Incrementele wijzigingen naar on-premises schijven worden regelmatig gerepliceerd naar de replicaschijven in Azure.


U kunt de status van de taak in de portalmeldingen bijhouden.

![De taak volgen](./media/tutorial-migrate-vmware-agent/jobs.png)

U kunt de replicatiestatus controleren door te klikken op **servers repliceren** in **Azure Migrate: Servermigratie**.
![Monitor voor replicatie](./media/tutorial-migrate-vmware-agent/replicate-servers.png)

## <a name="run-a-test-migration"></a>Een testmigratie uitvoeren


Wanneer wordt begonnen met replicatie van verschillen, kunt u een testmigratie uitvoeren voor de virtuele machines, voordat u een volledige migratie naar Azure uitvoert. Het is raadzaam dat te ten minste één keer voor elke machine doen voordat u deze migreren.

- Controles zijn uitgevoerd een testmigratie die migratie werkt zoals verwacht, zonder gevolgen voor de on-premises computers die blijven werken, en doorgaan met repliceren. 
- Testmigratie simuleert de migratie door het maken van een Azure-VM met behulp van gerepliceerde gegevens (meestal migreren naar een VNet niet-productie in uw Azure-abonnement).
- U kunt de gerepliceerde test virtuele machine van Azure gebruiken om de migratie valideren, testen van de app uit te voeren en los eventuele problemen op voordat de volledige migratie.

Een testmigratie als volgt:


1. In **migratie doelstellingen** > **Servers** > **Azure Migrate: Servermigratie**, klikt u op **Test gemigreerd servers**.

     ![Gemigreerde servers testen](./media/tutorial-migrate-vmware-agent/test-migrated-servers.png)

2. Met de rechtermuisknop op de virtuele machine om te testen, en klikt u op **Test migreren**.

    ![Migratie testen](./media/tutorial-migrate-vmware-agent/test-migrate.png)

3. In **testmigratie**, selecteer het Azure VNet waarin de virtuele machine van Azure geplaatst na de migratie worden. Het is raadzaam om dat een niet-productie-VNet te gebruiken.
4. De **de testmigratie** taak wordt gestart. De taak in de portalmeldingen bewaken.
5. Nadat de migratie is voltooid, geven de gemigreerde Azure-VM in **virtuele Machines** in Azure portal. De machinenaam van de heeft een achtervoegsel **-Test**.
6. Nadat de test is voltooid, met de rechtermuisknop op de Azure-VM in **machines repliceren**, en klikt u op **opschonen van de testmigratie**.

    ![Opschonen van de migratie](./media/tutorial-migrate-vmware-agent/clean-up.png)


## <a name="migrate-vms"></a>Virtuele machines migreren

Nadat u hebt gecontroleerd dat de testmigratie werkt zoals verwacht, kunt u de on-premises machines migreren.

1. In de Azure Migrate-project > **Servers** > **Azure Migrate: Servermigratie**, klikt u op **servers repliceren**.

    ![Servers repliceren](./media/tutorial-migrate-vmware-agent/replicate-servers.png)

2. In **machines repliceren**, met de rechtermuisknop op de virtuele machine > **migreren**.
3. In **migreren** > **virtuele machines afsluiten en het uitvoeren van een geplande migratie zonder gegevensverlies**, selecteer **Ja** > **OK** .
    - Standaard Azure Migrate de on-premises VM wordt afgesloten en wordt uitgevoerd een replicatie op-verzoek om te synchroniseren van alle VM-wijzigingen die sinds de laatste replicatie is opgetreden. Dit zorgt ervoor zonder verlies van gegevens.
    - Als u niet dat de virtuele machine afsluiten wilt, selecteert u **Nee**
4. Een migratietaak start voor de virtuele machine. De taak in Azure meldingen volgen.
5. Nadat de taak is voltooid, kunt u weergeven en beheren van de virtuele machine van de **virtuele Machines** pagina.

## <a name="complete-the-migration"></a>Migratie voltooien

1. Nadat de migratie is voltooid, met de rechtermuisknop op de virtuele machine > **migratie stoppen**. Deze replicatie voor de on-premises machine stopt en opschonen van gegevens over de replicatiestatus voor de virtuele machine.
2. Installeer de Azure-VM [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) of [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) agent op de gemigreerde virtuele machines.
3. Voer correcties van de app uit na de migratie, zoals updates van de databaseverbindingsreeksen en webserverconfiguraties.
4. Voer acceptatietesten van de toepassing en de migratie uit op de gemigreerde toepassing die nu wordt uitgevoerd in Azure.
5. Overzetten van verkeer naar de gemigreerde virtuele machine van Azure-instantie.
6. Verwijder de on-premises VM's uit uw lokale VM-inventaris.
7. Verwijder de on-premises VM's uit de lokale back-ups.
8. Werk eventuele interne documentatie bij met de nieuwe locatie en het nieuwe IP-adres van de Azure VM's. 

## <a name="post-migration-best-practices"></a>Aanbevolen procedures voor na de migratie

- Voor grotere flexibiliteit:
    - Houd uw gegevens veilig door back-ups van virtuele Azure VM‘s te maken met behulp van de Azure Backup-service. [Meer informatie](../backup/quick-backup-vm-portal.md).
    - Houd workloads continu beschikbaar door Azure VM‘s naar een secundaire regio te repliceren met Site Recovery. [Meer informatie](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Voor betere beveiliging:
    - Vergrendelen en beperken van toegang tot het binnenkomende verkeer met [Azure Security Center - Just-in-time-beheer](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).
    - Beperk het netwerkverkeer naar beheereindpunten met [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview).
    - Implementeer [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) om schijven te beveiligen en gegevens te beschermen tegen diefstal en onbevoegde toegang.
    - Lees meer informatie over [IaaS-resources beveiligen](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) en bezoek het [Azure Security Center](https://azure.microsoft.com/services/security-center/).
- Voor controle en beheer:
-  Overweeg de implementatie van [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) om uw resourcegebruik en uitgaven te bewaken.


## <a name="next-steps"></a>Volgende stappen

Onderzoek de [migratie naar de cloud](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) in Azure Cloud Adoption Framework.
