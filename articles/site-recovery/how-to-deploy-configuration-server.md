---
title: De configuratieserver voor noodherstel van VMware met Azure Site Recovery implementeren | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een configuratieserver voor noodherstel van VMware met Azure Site Recovery implementeert
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2018
ms.author: anoopkv
ms.openlocfilehash: 3b09c11d76d5c88b904afa3c6d256bc475992339
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
# <a name="deploy-a-configuration-server"></a>Een configuratieserver implementeren

U implementeert een on-premises configuratieserver wanneer u [Azure Site Recovery](site-recovery-overview.md) voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure. De configuratie server coördinaten communicatie tussen on-premises VMware en Azure. Hiermee gegevensreplicatie ook worden beheerd. Dit artikel begeleidt u bij de stappen die nodig zijn voor het implementeren van de configuratieserver.

## <a name="prerequisites"></a>Vereisten

Het is raadzaam dat u de configuratieserver als een maximaal beschikbare VMware VM implementeren. Voor replicatie van de fysieke server, worden de configuratieserver ingesteld op een fysieke machine. Minimale hardwarevereisten worden samengevat in de volgende tabel.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]




## <a name="capacity-planning"></a>Capaciteitsplanning

De sizing vereisten voor de configuratieserver is afhankelijk van de mogelijke snelheid waarmee u gegevens wijzigt. Deze tabel als richtlijn gebruiken.

| **CPU** | **Geheugen** | **Cachegrootte van de schijf** | **Snelheid van de gegevens wijzigen** | **Beveiligde machines** |
| --- | --- | --- | --- | --- |
| 8 vcpu's (2-sockets * @ 2,5 GHz-4 kernen) |16 GB |300 GB |500 GB of minder |Minder dan 100 machines repliceren. |
| 12 vcpu's (2-sockets * @ 2,5 GHz-6 kernen) |18 GB |600 GB |500 GB tot 1 TB |100 tot 150-machines repliceren. |
| 16 vcpu's (2-sockets * @ 2,5 GHz-8 kernen) |32 GB |1 TB |1 TB tot 2 TB |150 tot 200-machines repliceren. |


Als u virtuele VMware-machines repliceert, lees meer over [overwegingen bij capaciteitsplanning](/site-recovery-plan-capacity-vmware.md). Voer de [planner implementatieprogramma](site-recovery-deployment-planner.md) voor VMWare-replicatie.



## <a name="download-the-template"></a>De sjabloon downloaden

Site Recovery biedt een sjabloon downloaden voor het instellen van de configuratieserver als een maximaal beschikbare VMware-VM. 

1. Ga in de kluis naar **Infrastructuur voorbereiden** > **Bron**.
2. In **bron voorbereiden**, selecteer **+ configuratieserver**.
3. Controleer in **Server toevoegen** of **Configuratieserver voor VMware** wordt weergegeven in **Servertype**.
4. Download de OVF-sjabloon (Open Virtualization Format) voor de configuratieserver.

  > [!TIP]
  U kunt de nieuwste versie van de configuratie van server-sjabloon rechtstreeks vanuit downloaden [Microsoft Download Center](https://aka.ms/asrconfigurationserver).


## <a name="import-the-template-in-vmware"></a>De sjabloon in VMware importeren


1. Meld u aan de VMware vCenter-server of de vSphere ESXi-host met behulp van de VMWare vSphere Client.
2. Op de **bestand** selecteert u **OVF-sjabloon implementeren** om de wizard OVF-sjabloon implementeren te starten.

     ![OVF-sjabloon](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. In **Select bron**, geef de locatie van de gedownloade OVF.
4. In **Bekijk de details**, selecteer **volgende**.
5. In **naam en een map selecteren** en **Select configuration**, accepteer de standaardinstellingen.
6. Selecteer in **Opslag selecteren****Thick Provision Eager Zeroed** in **Indeling virtuele schijf selecteren**.
4. Accepteer de standaardinstellingen in de rest van de wizardpagina’s.
5. In **Klaar om te voltooien**:

    * Selecteer **Inschakelen na de implementatie** > **Voltooien** om de VM in te stellen met de standaardinstellingen.

    * Als u wilt een extra netwerkinterface toevoegen, schakelt u **na de implementatie inschakelen**, en selecteer vervolgens **voltooien**. Standaard wordt de configuratie van server-sjabloon geïmplementeerd met een enkele netwerkinterfacekaart. U kunt extra netwerkinterfacekaarten toevoegen na de implementatie.


## <a name="add-an-additional-adapter"></a>Een extra adapter toevoegen

Als u een extra NIC en de configuratieserver toevoegen wilt, moet u deze toevoegen voordat u de server in de kluis registreren. Het toevoegen van extra adapters wordt niet ondersteund na registratie.

1. Klik in de vSphere Client-inventaris met de rechtermuisknop op de VM en selecteer **Instellingen bewerken**.
2. In **Hardware**, selecteer **toevoegen** > **Ethernet-Adapter**. Selecteer vervolgens **Volgende**.
3. Selecteer een type netwerkadapter en een netwerk. 
4. Als u verbinding wilt maken met de virtuele NIC verbinding wanneer de VM wordt ingeschakeld, selecteert u **Verbinding maken bij inschakelen**. Selecteer vervolgens **volgende** > **voltooien** > **OK**.
 

## <a name="register-the-configuration-server"></a>De configuratieserver registreren 

1. Schakel de VM in vanuit de VMWare vSphere Client-console.
2. De VM wordt opgestart in een Windows Server 2016-installatie-ervaring. Accepteer de gebruiksrechtovereenkomst en voer een administrator-wachtwoord.
3. Nadat de installatie is voltooid, meld u aan de virtuele machine als de beheerder.
4. De eerste keer dat u zich aanmeldt, de Azure Site Recovery Configuration Tool wordt gestart.
5. Voer een naam die wordt gebruikt voor het registreren van de configuratieserver met Site Recovery. Selecteer vervolgens **Volgende**.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding is gemaakt, selecteert u **aanmelden** aan te melden bij uw Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u de configuratieserver wilt registreren.
7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
8. Meld u aan de machine opnieuw. De configuratiewizard voor server management wordt automatisch gestart.

### <a name="configure-settings"></a>Instellingen configureren

1. Selecteer in de wizard Configuratie server **Setup-connectiviteit**. Selecteer de NIC moet binnenkomend replicatieverkeer en selecteer vervolgens **opslaan**. U kunt deze instelling niet wijzigen nadat deze geconfigureerd.
2. In **Selecteer Recovery Services-kluis**, selecteer uw Azure-abonnement en de betreffende resourcegroep en -kluis.
3. In **software van derden installeren**, accepteer de gebruiksrechtovereenkomst. Selecteer **downloaden en installeren** MySQL-Server te installeren.
4. Selecteer **installeren VMware PowerLCI**. Zorg ervoor dat alle browservensters zijn gesloten voordat u deze stap. Selecteer vervolgens **doorgaan**.
5. In **configuratie valideren**, vereisten zijn gecontroleerd voordat u doorgaat.
6. In **vCenter Server vSphere/ESXi-server configureren**, voer de FQDN-naam of IP-adres van de vCenter-server of vSphere-host, waarin de virtuele machines die u wilt repliceren bevinden. Geef de poort op die de server luistert en een beschrijvende naam voor de VMware-server in de kluis.
7. Geef de referenties voor verbinding maken met de VMware-server worden gebruikt door de configuratieserver. Site Recovery gebruikt deze referenties voor het automatisch detecteren van VMware-VM’s die beschikbaar zijn voor replicatie. Selecteer **toevoegen**, en selecteer vervolgens **doorgaan**.
8. In **referenties voor de virtuele machine configureren**, geef de gebruikersnaam en wachtwoord moeten worden gebruikt voor de Mobility-Service van Azure Site Recovery automatisch wordt geïnstalleerd op computers, wanneer replicatie is ingeschakeld. Voor Windows-machines moet het account lokale administrator-machtigingen hebben op de machines die u wilt repliceren. Geef voor Linux de details voor de superuser op.
9. Selecteer **Finalize configuratie** inschrijving voltooien. 
10. Controleer nadat de registratie is voltooid in Azure Portal of de configuratieserver en de VMware-server worden weergegeven op de pagina **Bron** in de kluis. Selecteer vervolgens **OK** doelinstellingen configureren.


## <a name="troubleshoot-deployment-issues"></a>Oplossen van implementatieproblemen

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Volgende stappen

Bekijk de zelfstudies over het instellen van herstel na noodgevallen van [virtuele VMware-machines](tutorial-vmware-to-azure.md) en [fysieke servers](tutorial-physical-to-azure.md) naar Azure.
