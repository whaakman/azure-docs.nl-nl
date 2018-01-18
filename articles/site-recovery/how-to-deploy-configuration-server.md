---
title: " De configuratieserver voor noodherstel van VMware met Azure Site Recovery implementeren | Microsoft Docs"
description: Dit artikel wordt beschreven hoe u een configuratieserver voor noodherstel van VMware met Azure Site Recovery implementeert
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2018
ms.author: anoopkv
ms.openlocfilehash: e257ede08ac46ad863b4883b10399058e6f59f1f
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="deploy-a-configuration-server"></a>Een configuratieserver implementeren

U implementeert een on-premises configuratieserver wanneer u de [Azure Site Recovery](site-recovery-overview.md) service voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure. De configuratieserver WThe coördineert de communicatie tussen de on-premises VMware en Azure en beheert gegevensreplicatie. Dit artikel begeleidt u bij de stappen die nodig zijn voor het implementeren van de configuratieserver.

## <a name="prerequisites"></a>Vereisten

Het is raadzaam dat u de configuratieserver als een maximaal beschikbare VMware VM implementeren. Voor replicatie van de fysieke server, worden de configuratieserver ingesteld op een fysieke machine. Minimale hardwarevereisten worden samengevat in de volgende tabel.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]




## <a name="capacity-planning"></a>Capaciteitsplanning

De sizing vereisten voor de configuratieserver is afhankelijk van de mogelijke snelheid waarmee u gegevens wijzigt. Deze tabel als richtlijn gebruiken.

| **CPU** | **Geheugen** | **Cachegrootte van de schijf** | **Snelheid van de gegevens wijzigen** | **Beveiligde machines** |
| --- | --- | --- | --- | --- |
| 8 vcpu's (2-sockets * @ 2,5 GHz-4 kernen) |16 GB |300 GB |500 GB of minder |Minder dan 100 machines repliceren. |
| 12 vcpu's (2-sockets * @ 2,5 GHz-6 kernen) |18 GB |600 GB |500 GB tot 1 TB |100 tot 150-machines repliceren. |
| 16 vcpu's (2-sockets * @ 2,5 GHz-8 kernen) |32 GB |1 TB |1 TB tot 2 TB |Repliceren tussen 150 tot 200-machines. |


Als u virtuele VMware-machines repliceert, lees meer over [overwegingen bij capaciteitsplanning](/site-recovery-plan-capacity-vmware.md), en voer de [planner implementatieprogramma](site-recovery-deployment-planner.md) voor VMWare-replicatie.



## <a name="download-the-template"></a>De sjabloon downloaden

Site Recovery biedt een sjabloon downloaden voor het instellen van de configuratieserver als een maximaal beschikbare VMware-VM. 

1. In de kluis, gaat u naar **infrastructuur voorbereiden** > **bron**.
2. In **bron voorbereiden**, klikt u op **+ configuratieserver**.
3. In **Server toevoegen**, controleert u of **configuratieserver voor VMware** wordt weergegeven in **servertype**.
4. Download de sjabloon Open Virtualization Format (OVF) voor de configuratieserver.

  > [!TIP]
  De nieuwste versie van de configuratie van server-sjabloon rechtstreeks vanuit kan worden gedownload [Microsoft Download Center](https://aka.ms/asrconfigurationserver)


## <a name="import-the-template-in-vmware"></a>Importeer de VMware-sjabloon


1. Meld u bij de VMware vCenter-server of vSphere ESXi-host, met behulp van de VMWare vSphere Client.
2. Op de **bestand** selecteert u **OVF-sjabloon implementeren**, om de wizard OVF-sjabloon implementeren te starten.  

     ![OVF-sjabloon](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. In **Select bron**, geef de locatie van de gedownloade OVF.
4. In **Bekijk de details**, klikt u op **volgende**.
5. In **naam en een map selecteren**, en **Select configuration**, accepteer de standaardinstellingen.
6. In **opslag selecteren**, voor het selecteren van de beste prestaties **Thick inrichten enthousiaste nul** in **Selecteer virtuele schijf-indeling**.
4. Accepteer de standaardinstellingen in de rest van de wizardpagina's.
5. In **klaar om te voltooien**:
  - Als u de virtuele machine met de standaardinstellingen instelt, selecteert u **na de implementatie inschakelen** > **voltooien**.
  - Als u een extra netwerkinterface toevoegen wilt, schakelt u **na de implementatie inschakelen**, en selecteer vervolgens **voltooien**. De sjabloon van de server configuratie met één NIC wordt geïmplementeerd, maar u kunt extra netwerkinterfacekaarten toevoegen na de implementatie.


## <a name="add-an-additional-adapter"></a>Voeg een extra adapter

Als u een extra NIC en de configuratieserver toevoegen wilt, dat doen voordat u de server in de kluis registreren. Na de registratie wordt niet voor het toevoegen van extra adapters ondersteund.

1. In de vSphere Client voorraad, met de rechtermuisknop op de virtuele machine en selecteer **instellingen bewerken**.
2. In **Hardware**, klikt u op **toevoegen** > **Ethernet-Adapter**. Klik op **Volgende**.
3. Selecteer en adaptertype en een netwerk. 
4. Selecteer voor de virtuele NIC verbinding als de virtuele machine is ingeschakeld, **verbinding maken met power op**. Klik op **volgende** > **voltooien**, en klik vervolgens op **OK**.
 

## <a name="register-the-configuration-server"></a>Registreer de configuratieserver 

1. Schakel op de virtuele machine uit de VMWare vSphere Client-console.
2. De virtuele machine opnieuw opgestart in een Windows Server 2016-installatie-ervaring. Accepteer de gebruiksrechtovereenkomst en geef een administrator-wachtwoord.
3. Nadat de installatie is voltooid, meld u aan bij de virtuele machine als de beheerder.
4. De eerste keer dat u zich aanmeldt, de Azure Site Recovery Configuration Tool wordt gestart.
5. Geef een naam die wordt gebruikt voor het registreren van de configuratieserver met Site Recovery. Klik op **Volgende**.
6. Het hulpprogramma controleert of de virtuele machine verbinding met Azure maken kan. Nadat de verbinding is gemaakt, klikt u op **aanmelden**, aan te melden bij uw Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u wilt registreren van de configuratieserver.
7. Het hulpprogramma voert enkele configuratietaken en vervolgens opnieuw opgestart.
8. Meld u bij de computer opnieuw. De configuratiewizard voor server management wordt automatisch gestart.

### <a name="configure-settings"></a>Instellingen configureren

1. In de configuratiewizard voor server management > **Setup-connectiviteit**, selecteer de NIC die replicatieverkeer ontvangt. Klik vervolgens op **Opslaan**. U kunt deze instelling niet wijzigen nadat deze geconfigureerd.
2. In **Selecteer Recovery Services-kluis**, selecteert u uw Azure-abonnement en de betreffende resourcegroep en -kluis.
3. In **software van derden installeren**, accepteert u de licentie agreeemtn en klikt u op **downloaden en installeren**, MySQL-Server te installeren.
4. Klik op **installeren VMware PowerLCI**. Zorg ervoor dat alle browservensters zijn gesloten voordat u dit doen. Klik vervolgens op **doorgaan**
5. In **configuratie valideren**, vereisten kunnen worden gecontroleerd voordat u doorgaat.
6. In **vCenter Server vSphere/ESXi-server configureren**, geef de FQDN of IP-adres van de vCenter-server of vSphere-host, op welke virtuele machines die u wilt repliceren zich bevinden. Geef de poort waarop de server luistert en een beschrijvende naam moet worden gebruikt voor de VMware-server in de kluis.
7. Geef referenties op die door de configuratieserver worden gebruikt voor verbinding met de VMware-server. Site Recovery gebruikt deze referenties voor het automatisch detecteren VMware-machines die beschikbaar voor replicatie zijn. Klik op **toevoegen**, en klik vervolgens op **doorgaan**.
8. In **referenties voor de virtuele machine configureren**, de gebruikersnaam en wachtwoord invoeren die wordt gebruikt voor de Mobility-service automatisch wordt geïnstalleerd op computers, wanneer replicatie is ingeschakeld. Voor Windows-machines moet het account dat lokale administrator-bevoegdheden op de machines die u wilt repliceren. Geef details voor het hoofdaccount voor Linux.
9. Klik op **Finalize configuratie** om inschrijving te voltooien. 
10. Nadat de registratie is voltooid, wordt in de Azure portal controleren dat de configuratieserver en de VMware-server worden weergegeven op de **bron** pagina in de kluis. Klik vervolgens op **OK** doelinstellingen configureren.


## <a name="troubleshoot-deployment-issues"></a>Oplossen van implementatieproblemen

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Volgende stappen

Bekijk de zelfstudies voor het instellen van herstel na noodgevallen van [virtuele VMware-machines](tutorial-vmware-to-azure.md) en [fysieke servers](tutorial-physical-to-azure.md) naar Azure.
