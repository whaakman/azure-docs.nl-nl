---
title: De configuratieserver voor noodherstel van VMware met Azure Site Recovery implementeren | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een configuratieserver voor noodherstel van VMware met Azure Site Recovery implementeert
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: 99b368ca364bd7c5bebfc00c2df0f04333293388
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="deploy-a-configuration-server"></a>Een configuratieserver implementeren

U implementeert een on-premises configuratieserver wanneer u [Azure Site Recovery](site-recovery-overview.md) voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure. De configuratie server coördinaten communicatie tussen on-premises VMware en Azure. Hiermee gegevensreplicatie ook worden beheerd. Dit artikel begeleidt u bij de stappen die nodig zijn voor het implementeren van de configuratieserver wanneer u virtuele VMware-machines naar Azure repliceert. [Ga als volgt in dit artikel](physical-azure-set-up-source.md) als u nodig hebt voor het instellen van een configuratieserver voor replicatie van de fysieke server.

## <a name="prerequisites"></a>Vereisten

Het is raadzaam dat u de configuratieserver als een maximaal beschikbare VMware VM implementeren. Minimale hardwarevereisten worden samengevat in de volgende tabel.

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
2. Selecteer **+Configuratieserver** in **Bron voorbereiden**.
3. Controleer in **Server toevoegen** of **Configuratieserver voor VMware** wordt weergegeven in **Servertype**.
4. Download de OVF-sjabloon (Open Virtualization Format) voor de configuratieserver.

  > [!TIP]
  U kunt de nieuwste versie van de configuratie van server-sjabloon rechtstreeks vanuit downloaden [Microsoft Download Center](https://aka.ms/asrconfigurationserver).


## <a name="import-the-template-in-vmware"></a>De sjabloon in VMware importeren


1. Meld u bij de VMware vCenter-server of vSphere ESXi-host met behulp van de VMWare vSphere-client.
2. Selecteer in het menu **Bestand** de optie **OVF-sjabloon implementeren** om de wizard voor het implementeren van OVF-sjablonen te starten.

     ![OVF-sjabloon](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. In **Select bron**, geef de locatie van de gedownloade OVF.
4. In **Bekijk de details**, selecteer **volgende**.
5. In **naam en een map selecteren** en **Select configuration**, accepteer de standaardinstellingen.
6. Selecteer in **Opslag selecteren****Thick Provision Eager Zeroed** in **Indeling virtuele schijf selecteren**.
4. Accepteer de standaardinstellingen in de rest van de wizardpagina’s.
5. In **Klaar om te voltooien**:

    * Selecteer **Inschakelen na de implementatie** > **Voltooien** om de VM in te stellen met de standaardinstellingen.

    * Als u wilt een extra netwerkinterface toevoegen, schakelt u **na de implementatie inschakelen**, en selecteer vervolgens **voltooien**. Standaard wordt de configuratieserversjabloon met één NIC geïmplementeerd. Na de implementatie kunt u meer NIC’s toevoegen.


## <a name="add-an-additional-adapter"></a>Een extra adapter toevoegen

Als u een extra NIC en de configuratieserver toevoegen wilt, moet u deze toevoegen voordat u de server in de kluis registreren. Het toevoegen van extra adapters wordt niet ondersteund na registratie.

1. Klik in de vSphere Client-inventaris met de rechtermuisknop op de VM en selecteer **Instellingen bewerken**.
2. Selecteer **Toevoegen** > **Ethernet-adapter** bij **Hardware**. Selecteer vervolgens **Volgende**.
3. Selecteer een adaptertype en een netwerk. 
4. Als u verbinding wilt maken met de virtuele NIC verbinding wanneer de VM wordt ingeschakeld, selecteert u **Verbinding maken bij inschakelen**. Selecteer vervolgens **volgende** > **voltooien** > **OK**.
 

## <a name="register-the-configuration-server"></a>De configuratieserver registreren 

1. Schakel de VM in vanuit de VMWare vSphere Client-console.
2. De VM wordt opgestart in een Windows Server 2016-installatie-ervaring. Accepteer de gebruiksrechtovereenkomst en voer een Administrator-wachtwoord in.
3. Meld u nadat de installatie is voltooid bij de virtuele machine aan als de administrator.
4. De eerste keer dat u zich aanmeldt, wordt het configuratieprogramma van Azure Site Recovery gestart.
5. Voer een naam in die wordt gebruikt voor het registreren van de configuratieserver bij Site Recovery. Selecteer vervolgens **Volgende**.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, selecteert u **Aanmelden** om u aan te melden bij uw Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u de configuratieserver wilt registreren.
7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
8. Meld u opnieuw aan bij de machine. De wizard voor het beheer van de configuratieserver wordt automatisch gestart.

### <a name="configure-settings"></a>Instellingen configureren

1. Selecteer in de wizard Configuratie server **Setup-connectiviteit**. Selecteer de NIC moet binnenkomend replicatieverkeer en selecteer vervolgens **opslaan**. U kunt deze instelling niet wijzigen nadat deze geconfigureerd.
2. Selecteer in **Selecteer Recovery Services-kluis** uw Azure-abonnement en de relevante resourcegroep en kluis.
3. Accepteer de gebruiksrechtovereenkomst in **Software van derden installeren**. Selecteer **Downloaden en installeren** om MySQL Server te installeren.
4. Selecteer **installeren VMware PowerLCI**. Zorg ervoor dat alle browservensters zijn gesloten voordat u deze stap. Selecteer vervolgens **Doorgaan**.
5. In **De configuratie van het apparaat valideren** worden de vereisten gecontroleerd voordat u doorgaat.
6. In **vCenter Server vSphere/ESXi-server configureren** voert u de FQDN of het IP-adres van de vCenter-server, of vSphere-host, in waar de VM's die u wilt repliceren zich bevinden. Geef de poort op die de server luistert en een beschrijvende naam voor de VMware-server in de kluis.
7. Voer referenties in die door de configuratieserver moeten worden gebruikt voor verbinding met de VMware-server. Site Recovery gebruikt deze referenties voor het automatisch detecteren van VMware-VM’s die beschikbaar zijn voor replicatie. Selecteer **Toevoegen** en vervolgens **Doorgaan**.
8. In **referenties voor de virtuele machine configureren**, geef de gebruikersnaam en wachtwoord moeten worden gebruikt voor de Mobility-Service van Azure Site Recovery automatisch wordt geïnstalleerd op computers, wanneer replicatie is ingeschakeld. Voor Windows-machines moet het account lokale administrator-machtigingen hebben op de machines die u wilt repliceren. Geef voor Linux de details voor de superuser op.
9. Selecteer **Finalize configuratie** inschrijving voltooien. 
10. Controleer nadat de registratie is voltooid in Azure Portal of de configuratieserver en de VMware-server worden weergegeven op de pagina **Bron** in de kluis. Selecteer vervolgens **OK** om de doelinstellingen te configureren.


## <a name="troubleshoot-deployment-issues"></a>Oplossen van implementatieproblemen

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Volgende stappen

Instellen van herstel na noodgevallen van [virtuele VMware-machines](vmware-azure-tutorial.md) naar Azure.
