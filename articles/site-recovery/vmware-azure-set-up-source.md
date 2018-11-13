---
title: De bron omgeving en de configuratie beveiligingsstandaards herstel na noodgevallen instellen van virtuele VMware-machines naar Azure met Azure Site Recovery | Microsoft-Docs
description: Dit artikel wordt beschreven hoe u uw on-premises omgeving en de configuratieserver voor herstel na noodgevallen van virtuele VMware-machines naar Azure met Azure Site Recovery kunt instellen.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: e09e93ef85449c51e35b8da7ad93ee7088a0e7b4
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51566309"
---
# <a name="set-up-the-source-environment-and-configuration-server"></a>Instellen van de omgeving en de configuratie van een bronserver

In dit artikel wordt beschreven hoe u voor het instellen van uw gegevensbron on-premises omgeving voor herstel na noodgevallen van virtuele VMware-machines naar Azure met [Azure Site Recovery](site-recovery-overview.md). Dit omvat stappen voor het instellen van een on-premises machine als de Site Recovery-configuratieserver en automatisch detecteren van on-premises VM's. 

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat u hebt het volgende gedaan

- Geplande uw disaster recovery-implementatie gebruikt de [Azure Site Recovery Deployment Planner](site-recovery-deployment-planner.md). De Planner kunt u ervoor zorgen er voldoende bronnen en bandbreedte voor herstel na noodgevallen en het herstel wijst objective (RPO) in uw organisatie.
- [Instellen van Azure-resources](tutorial-prepare-azure.md) in de [Azure-portal](http://portal.azure.com).
- [Instellen van on-premises VMware](vmware-azure-tutorial-prepare-on-premises.md) resources, met inbegrip van een toegewezen account voor automatische detectie van on-premises VMware-VM's.
- U kunt [Veelgestelde vragen bekijken](vmware-azure-common-questions.md#configuration-server) over de configuratie van server-implementatie en beheer.


## <a name="choose-protection-goals"></a>Beveiligingsdoelstellingen kiezen

1. Selecteer de naam van de kluis in **Recovery Services-kluizen**. 
2. Selecteer in **Aan de slag** Site Recovery. Selecteer vervolgens **Infrastructuur voorbereiden**.
3. In **Beveiligingsdoel** > **Waar bevinden de machines zich**, selecteert u **On-premises**.
4. In **Waarnaartoe wilt u de machines repliceren** selecteert u **Naar Azure**.
5. In **Zijn de machines gevirtualiseerd** selecteert **Ja, met VMware vSphere Hypervisor**. Selecteer vervolgens **OK**.

## <a name="about-the-configuration-server"></a>Over de configuratieserver

U implementeert een on-premises configuratieserver bij het instellen van herstel na noodgevallen van virtuele VMware-machines naar Azure.

- De configuratie van server coördinaten communicatie tussen on-premises VMware en Azure. Deze beheert de gegevensreplicatie.
- U kunt de configuratieserver implementeren als een VMware-VM.
- Site Recovery biedt een OVA-sjabloon die u downloaden vanuit de Azure-portal en importeren in de vCenter-Server voor het instellen van de virtuele machine van de configuratieserver.
- [Meer informatie](vmware-azure-architecture.md) over de onderdelen van de configuratieserver en processen.


>[!NOTE]
Gebruik geen deze instructies als u de configuratieserver voor herstel na noodgevallen van on-premises fysieke machines naar Azure implementeert. Ga als volgt voor dit scenario [in dit artikel](physical-azure-set-up-source.md).


## <a name="before-you-deploy-the-configuration-server"></a>Voordat u de configuratieserver implementeren

Als u de configuratieserver als een VMware-VM met behulp van het OVA-sjabloon installeren, wordt de virtuele machine met alle vereiste onderdelen worden geïnstalleerd. Als u bekijken van de vereisten wilt, gebruikt u de volgende artikelen.

- [Meer informatie over](vmware-azure-configuration-server-requirements.md) de hardware, software en de vereiste capaciteit voor de configuratieserver.
- Als u meerdere virtuele VMware-machines repliceert, wordt u aangeraden de [overwegingen voor capaciteitsplanning](site-recovery-plan-capacity-vmware.md), en voer de [Azure Site Recovery Deployment Planner](site-recovery-deployment-planner.md) hulpprogramma voor VMWare-replicatie.
- De Windows-licentie voor het OVA-sjabloon is een evaluatielicentie is geldig gedurende 180 dagen. Na deze tijd moet u Windows activeren met een geldige licentie. 
- Het OVA-sjabloon biedt een eenvoudige manier voor het instellen van de configuratieserver als een VMware-VM. Als u de VMware-VM zonder dat de sjabloon instellen voor een of andere reden wilt, aan de vereisten controleren en volg de handmatige instructies in [in dit artikel](physical-azure-set-up-source.md).
- Uw Azure-account moet machtigingen voor het maken van Azure AD-apps.


>[!IMPORTANT]
De configuratieserver moet worden geïmplementeerd, zoals beschreven in dit artikel. Kopiëren of klonen van een bestaande configuratieserver wordt niet ondersteund.

### <a name="set-up-azure-account-permissions"></a>Azure-accountmachtigingen instellen

Een tenant/globale beheerder kan machtigingen toewijzen aan Azure AD-apps aan het account te maken of de toepassingsontwikkelaar-rol (met de machtigingen) toewijzen aan het account.


De tenant/globale beheerder kan machtigingen verlenen voor het account als volgt:

1. De tenant/globale beheerder moet in Azure AD, navigeer naar **Azure Active Directory** > **gebruikers** > **gebruikersinstellingen**.
2. De beheerder moet ingesteld **App-registraties** naar **Ja**.

 > [!NOTE]
 > Dit is een standaardinstelling die gevoelige niet. [Meer informatie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

U kunt ook toegangsmachtigingen de tenant/globale beheerder voor de rol toewijzen aan het account. [Meer informatie](https://docs.microsoft.comazure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

 


## <a name="download-the-ova-template"></a>Het OVA-sjabloon downloaden

1. Ga in de kluis naar **Infrastructuur voorbereiden** > **Bron**.
2. Selecteer **+Configuratieserver** in **Bron voorbereiden**.
3. Controleer in **Server toevoegen** of **Configuratieserver voor VMware** wordt weergegeven in **Servertype**.
4. Download de Open Virtualization-toepassing (OVA)-sjabloon voor de configuratieserver.

  > [!TIP]
>U kunt ook downloaden de nieuwste versie van de configuratieserversjabloon uit de [Microsoft Download Center](https://aka.ms/asrconfigurationserver).


## <a name="import-the-ova-template-in-vmware"></a>Het OVA-sjabloon in VMware importeren

1. Meld u aan bij de VMware vCenter-server of vSphere ESXi-host met behulp van de VMWare vSphere-client.
2. Selecteer in het menu **Bestand** de optie **OVF-sjabloon implementeren** om de wizard voor het implementeren van OVF-sjablonen te starten.

     ![OVF-sjabloon](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. In **bron selecteren**, geef de locatie van de gedownloade OVF.
4. In **Beoordelingsdetails**, selecteer **volgende**.
5. In **naam en map selecteren** en **configuratie selecteren**, accepteer de standaardinstellingen.
6. Selecteer in **Opslag selecteren** **Thick Provision Eager Zeroed** in **Indeling virtuele schijf selecteren**.
7. Accepteer de standaardinstellingen in de rest van de wizardpagina’s.
8. In **klaar om te voltooien**, voor het instellen van de virtuele machine met de standaardinstellingen, selecteer **inschakelen na de implementatie** > **voltooien**.
9. Standaard wordt de virtuele machine geïmplementeerd met een enkele NIC. Als u een aanvulling NIC toevoegen wilt, schakelt u **inschakelen na de implementatie**, en klikt u op **voltooien**. Volg vervolgens de volgende procedure.

## <a name="add-an-adapter-to-the-configuration-server"></a>Een adapter toevoegen aan de configuratieserver

Als u toevoegen van een extra NIC aan de configuratieserver wilt, moet u het toevoegen voordat u de server in de kluis registreert. Het toevoegen van extra adapters wordt niet ondersteund na registratie.

1. Klik in de vSphere Client-inventaris met de rechtermuisknop op de VM en selecteer **Instellingen bewerken**.
2. Selecteer **Toevoegen** > **Ethernet-adapter** bij **Hardware**. Selecteer vervolgens **Volgende**.
3. Selecteer een adaptertype en een netwerk.
4. Selecteer voor de virtuele NIC verbinding wanneer de virtuele machine is ingeschakeld, **verbinding maken met power-on**. Selecteer vervolgens **volgende** > **voltooien** > **OK**.

## <a name="register-the-configuration-server"></a>De configuratieserver registreren 
Schakel op de virtuele machine en de configuratieserver registreren in de Site Recovery-kluis.

1. Schakel de VM in vanuit de VMWare vSphere Client-console.
2. De virtuele machine wordt opgestart in Windows Server 2016 installatie-ervaring. Accepteer de gebruiksrechtovereenkomst en voer een Administrator-wachtwoord in.
3. Meld u nadat de installatie is voltooid bij de virtuele machine aan als de administrator.



## <a name="set-up-the-configuration-server"></a>Instellen van de configuratieserver

Als onderdeel van de implementatie moet u MySQL installeren op de virtuele machine van de configuratieserver. U kunt dit op verschillende manieren doen:

- Site Recovery downloaden en installeren van MySQL kunt. Als u wilt dat deze optie wilt gebruiken, moet u niet verder niets te doen voordat u de installatie van de configuratie begint.
- Handmatig downloaden en installeren MySQL: voordat u de configuratieserver instellen, download het installatieprogramma van MySQL en plaatsen de **C:\Temp\ASRSetup** map. MySQL installeren. 
- Handmatig downloaden en laat u Site Recovery installeert. Voordat u de configuratieserver instellen, download het installatieprogramma van MySQL en plaatst deze in de **C:\Temp\ASRSetup** map.


1. De eerste keer dat u zich bij de virtuele machine aanmeldt, de Azure Site Recovery Configuration Tool wordt gestart.
2. Geef een naam die wordt gebruikt voor het registreren van de configuratieserver in de Site Recovery-kluis. Selecteer vervolgens **Volgende**.
3. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, selecteert u **Aanmelden** om u aan te melden bij uw Azure-abonnement. Houd er rekening mee dat het account moet toegang hebben tot de kluis waarin u wilt registreren van de configuratieserver.
4. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
5. Meld u opnieuw aan bij de machine. Wizard beheer van de configuratieserver wordt binnen een paar seconden automatisch gestart.
6. Selecteer in de wizard **connectiviteit instellen**.
7. Selecteer de NIC die gebruikmaakt van de processerver (standaard uitgevoerd op de configuratieserver) voor het ontvangen van replicatieverkeer van virtuele machines.
8. Selecteer vervolgens **Opslaan**. Houd er rekening mee dat u vault-instellingen niet wijzigen nadat de configuratieserver is geregistreerd. 
9. In **Selecteer Recovery Services-kluis**, zich aanmelden bij Microsoft Azure, selecteer uw Azure-abonnement en de relevante resourcegroep en kluis. 
10. In **software van derden installeren**, MySQL installeren:
     - Als u Site Recovery verwerkt het MySQL downloaden en installeren, klikt u op **downloadt en installeert u**. Wacht tot de installatie te voltooien en ga daarna verder met de wizard.
     - Als u MySQL hebt gedownload en Site Recovery wilt installeren, klikt u op **Download en installeer**. Wacht totdat de installatie is voltooid en gaat u verder met de wizard.
     - Als u handmatig gedownload en MySQL heeft geïnstalleerd, klikt u op **Download en installeer**. De app weergegeven als **al geïnstalleerd**. Ga verder met de wizard.
11. In **configuratie van het apparaat valideren**, setup controleert of de vereisten voordat u doorgaat. 
12. In **vCenter Server/vSphere ESXi-server configureren**, voer de FQDN-naam of IP-adres van de vCenter-server of vSphere-host, op welke virtuele machines die u wilt repliceren zich bevinden. Geef de poort waarop de server luistert, en geef een beschrijvende naam voor de VMware-server in de kluis.
13. Voer de referenties die worden gebruikt door de configuratieserver verbinding maken met de VMware-server en VMware-VM's die beschikbaar voor replicatie zijn automatisch detecteren. Selecteer **toevoegen** >  **blijven**. De referenties worden lokaal opgeslagen.
14. In **referenties voor virtuele machine configureren**, geef de referenties op die Site Recovery gebruiken voor de Mobility-Service automatisch geïnstalleerd wanneer u replicatie voor een virtuele machine inschakelt.
    - Voor Windows-machines moet het account lokale administrator-machtigingen hebben op de machines die u wilt repliceren.
    - Geef voor Linux de details voor de superuser op.
15. Selecteer **Configuratie voltooien** om de registratie te voltooien.
16. Nadat de registratie is voltooid, opent u Azure portal en controleer of de configuratieserver en de VMware-server worden weergegeven **Recovery Services-kluis** > **beheren**  >   **Infrastructuur voor site Recovery** > **configuratieservers**.


## <a name="exclude-antivirus-on-the-configuration-server"></a>Antivirussoftware op de configuratieserver uitsluiten

Als antivirussoftware op de virtuele machine van de configuratieserver wordt uitgevoerd, ervoor zorgen dat de volgende mappen zijn uitgesloten van antivirus bewerkingen. Dit zorgt ervoor dat replicatie en connectiviteit werken zoals verwacht: 

- C:\Program Files\Microsoft Azure Recovery Services-Agent
- C:\Program Files\Microsoft Azure Site Recovery Provider
- C:\Program Files\Microsoft Azure Site Recovery Configuration Manager
- C:\Program Files\Microsoft Azure Site Recovery fout verzameling Tool
- C:\thirdparty
- C:\Temp
- C:\strawberry
- C:\ProgramData\MySQL
- C:\Program bestanden (x86) \MySQL
- C:\ProgramData\ASR
- C:\ProgramData\Microsoft Azure Site Recovery
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\Inetpub
- Site Recovery-installatiemap. Bijvoorbeeld: E:\Program bestanden (x86) \Microsoft Azure Site Recovery


## <a name="next-steps"></a>Volgende stappen
- Als u problemen ondervindt, raadpleegt u [Veelgestelde vragen over](vmware-azure-common-questions.md#configuration server) en [probleemoplossing](vmware-azure-troubleshoot-configuration-server.md) voor de configuratieserver.
- Nu [uw doelomgeving instellen](./vmware-azure-set-up-target.md) in Azure.
