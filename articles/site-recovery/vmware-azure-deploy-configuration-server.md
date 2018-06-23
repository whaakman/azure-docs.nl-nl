---
title: De configuratieserver voor noodherstel van VMware met Azure Site Recovery implementeren | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een configuratieserver voor noodherstel van VMware met Azure Site Recovery implementeert
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 05/06/2018
ms.author: raynew
ms.openlocfilehash: 841176d8c5f215d18edf25b1f191792b37555fa9
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318116"
---
# <a name="deploy-a-configuration-server"></a>Een configuratieserver implementeren

U implementeert een on-premises configuratieserver wanneer u [Azure Site Recovery](site-recovery-overview.md) voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure. De configuratie server coördinaten communicatie tussen on-premises VMware en Azure. Hiermee gegevensreplicatie ook worden beheerd. Dit artikel begeleidt u bij de stappen die nodig zijn voor het implementeren van de configuratieserver wanneer u virtuele VMware-machines naar Azure repliceert. [Ga als volgt in dit artikel](physical-azure-set-up-source.md) als u nodig hebt voor het instellen van een configuratieserver voor replicatie van de fysieke server.

>[!TIP]
U kunt meer informatie over de rol van de configuratieserver als onderdeel van Azure Site Recovery-architectuur [hier](vmware-azure-architecture.md).

## <a name="deployment-of-configuration-server-through-ova-template"></a>Implementatie van configuratieserver via eicellen sjabloon

Configuratieserver moet worden ingesteld als een maximaal beschikbare VMware-VM met bepaalde minimale hardware- en sizing vereisten. Site Recovery biedt voor de implementatie van gemakkelijk en een sjabloon downloaden eicellen (Open Virtualization-toepassing) voor het instellen van de configuratieserver die aan de verplichte vereisten die voldoet hieronder worden vermeld.

## <a name="prerequisites"></a>Vereisten

Minimale hardwarevereisten voor een configuratieserver worden samengevat in de volgende tabel.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="capacity-planning"></a>Capaciteitsplanning

De sizing vereisten voor de configuratieserver is afhankelijk van de mogelijke snelheid waarmee u gegevens wijzigt. Deze tabel als richtlijn gebruiken.

| **CPU** | **Geheugen** | **Cachegrootte van de schijf** | **Snelheid van de gegevens wijzigen** | **Beveiligde machines** |
| --- | --- | --- | --- | --- |
| 8 vcpu's (2-sockets * @ 2,5 GHz-4 kernen) |16 GB |300 GB |500 GB of minder |Minder dan 100 machines repliceren. |
| 12 vcpu's (2-sockets * @ 2,5 GHz-6 kernen) |18 GB |600 GB |500 GB tot 1 TB |100 tot 150-machines repliceren. |
| 16 vcpu's (2-sockets * @ 2,5 GHz-8 kernen) |32 GB |1 TB |1 TB tot 2 TB |150 tot 200-machines repliceren. |

Als u meer dan één VMware VM repliceert, leest u [overwegingen bij capaciteitsplanning](/site-recovery-plan-capacity-vmware.md). Voer de [planner implementatieprogramma](site-recovery-deployment-planner.md) voor VMWare-replicatie.

## <a name="download-the-template"></a>De sjabloon downloaden

1. Ga in de kluis naar **Infrastructuur voorbereiden** > **Bron**.
2. Selecteer **+Configuratieserver** in **Bron voorbereiden**.
3. Controleer in **Server toevoegen** of **Configuratieserver voor VMware** wordt weergegeven in **Servertype**.
4. Download de sjabloon Open Virtualization-toepassing (eicellen) voor de configuratieserver.

  > [!TIP]
>U kunt ook de nieuwste versie van de configuratie van server-sjabloon rechtstreeks vanuit downloaden [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

>[!NOTE]
Het certificaat dat is opgegeven met eicellen sjabloon is een evaluatie van 180 dagen geldig certificaat. Na moet deze periode klant de windows activeren bij een geleverde certificaat.

## <a name="import-the-template-in-vmware"></a>De sjabloon in VMware importeren

1. Meld u bij de VMware vCenter-server of vSphere ESXi-host met behulp van de VMWare vSphere-client.
2. Selecteer in het menu **Bestand** de optie **OVF-sjabloon implementeren** om de wizard voor het implementeren van OVF-sjablonen te starten.

     ![OVF-sjabloon](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. In **Select bron**, geef de locatie van de gedownloade OVF.
4. In **Bekijk de details**, selecteer **volgende**.
5. In **naam en een map selecteren** en **Select configuration**, accepteer de standaardinstellingen.
6. Selecteer in **Opslag selecteren****Thick Provision Eager Zeroed** in **Indeling virtuele schijf selecteren**.
7. Accepteer de standaardinstellingen in de rest van de wizardpagina’s.
8. In **Klaar om te voltooien**:

    * Selecteer **Inschakelen na de implementatie** > **Voltooien** om de VM in te stellen met de standaardinstellingen.

    * Als u wilt een extra netwerkinterface toevoegen, schakelt u **na de implementatie inschakelen**, en selecteer vervolgens **voltooien**. Standaard wordt de configuratieserversjabloon met één NIC geïmplementeerd. Na de implementatie kunt u meer NIC’s toevoegen.

## <a name="add-an-additional-adapter"></a>Een extra adapter toevoegen

Als u een extra NIC en de configuratieserver toevoegen wilt, moet u deze toevoegen voordat u de server in de kluis registreren. Het toevoegen van extra adapters wordt niet ondersteund na registratie.

1. Klik in de vSphere Client-inventaris met de rechtermuisknop op de VM en selecteer **Instellingen bewerken**.
2. Selecteer **Toevoegen** > **Ethernet-adapter** bij **Hardware**. Selecteer vervolgens **Volgende**.
3. Selecteer een adaptertype en een netwerk. 
4. Selecteer voor de virtuele NIC verbinding als de virtuele machine is ingeschakeld, **Connect bij opstarten**. Selecteer vervolgens **volgende** > **voltooien** > **OK**.

## <a name="register-the-configuration-server-with-azure-site-recovery-services"></a>De configuratieserver registreren met Azure Site Recovery-services

1. Schakel de VM in vanuit de VMWare vSphere Client-console.
2. De VM wordt opgestart in een Windows Server 2016-installatie-ervaring. Accepteer de gebruiksrechtovereenkomst en voer een Administrator-wachtwoord in.
3. Meld u nadat de installatie is voltooid bij de virtuele machine aan als de administrator.
4. De eerste keer dat u zich aanmeldt, binnen een paar seconden de Azure Site Recovery Configuration Tool wordt gestart.
5. Voer een naam in die wordt gebruikt voor het registreren van de configuratieserver bij Site Recovery. Selecteer vervolgens **Volgende**.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, selecteert u **Aanmelden** om u aan te melden bij uw Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u de configuratieserver wilt registreren.
7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
8. Meld u opnieuw aan bij de machine. De configuratiewizard voor server management begint **automatisch** in enkele seconden.

### <a name="configure-settings"></a>Instellingen configureren

1. Selecteer in de wizard voor het beheer van de configuratieserver **Connectiviteit instellen** en selecteer vervolgens de NIC die door de processerver wordt gebruikt voor het ontvangen van replicatieverkeer van VM's. Selecteer vervolgens **Opslaan**. U kunt deze instelling niet wijzigen nadat deze is geconfigureerd.
2. In **Selecteer Recovery Services-kluis**, zich aanmelden bij Microsoft Azure, selecteert u uw Azure-abonnement en de betreffende resourcegroep en de kluis.
    >[!NOTE]
    > Na de registratie is er geen flexibiliteit voor het wijzigen van de recovery services-kluis.
3. In **software van derden installeren**,

    |Scenario   |Volgende stappen uitvoeren  |
    |---------|---------|
    |Kan ik & downloaden MySQL handmatig installeren?     |  Ja. MySQL-toepassing downloaden en plaats deze in de map **C:\Temp\ASRSetup**, klikt u vervolgens handmatig installeren. Nu, wanneer u de voorwaarden accepteert > klikt u op **downloaden en installeren**, de portal zegt *al geïnstalleerd*. U kunt doorgaan met de volgende stap.       |
    |Voorkom ik downloaden van MySQL online?     |   Ja. Plaats uw MySQL installer-toepassing in de map **C:\Temp\ASRSetup**. De voorwaarden accepteren > Klik op **downloaden en installeren**, de portal gebruikt het installatieprogramma die u hebt toegevoegd, en installeert de toepassing. U kunt doorgaan met de volgende stap na de installatie.    |
    |Ik wil downloaden en installeren van MySQL via Azure Site Recovery     |  Accepteer de gebruiksrechtovereenkomst en klik op **downloaden en installeren**. Vervolgens kunt u doorgaan met de volgende stap na de installatie.       |
4. In **De configuratie van het apparaat valideren** worden de vereisten gecontroleerd voordat u doorgaat.
5. In **vCenter Server vSphere/ESXi-server configureren** voert u de FQDN of het IP-adres van de vCenter-server, of vSphere-host, in waar de VM's die u wilt repliceren zich bevinden. Voer de poort in waarop de server luistert. Voer een beschrijvende naam in voor de VMware-server in de kluis.
6. Voer referenties in die door de configuratieserver moeten worden gebruikt voor verbinding met de VMware-server. Site Recovery gebruikt deze referenties voor het automatisch detecteren van VMware-VM’s die beschikbaar zijn voor replicatie. Selecteer **toevoegen**, en vervolgens **gaan**. De referenties die u hier opgeeft, worden lokaal opgeslagen.
7. In **referenties voor de virtuele machine configureren**, geef de gebruikersnaam en wachtwoord van de Mobility-Service automatisch geïnstalleerd tijdens de replicatie voor virtuele machines. Voor **Windows** machines, het account moet lokale beheerrechten op de machines die u wilt repliceren. Voor **Linux**, informatie opgeven voor het root-account.
8. Selecteer **Configuratie voltooien** om de registratie te voltooien.
9. Nadat de registratie is voltooid, Azure portal openen, Controleer of de configuratieserver en de VMware-server worden vermeld op **Recovery Services-kluis** > **beheren**  >  **Site Recovery-infrastructuur** > **configuratieservers**.

## <a name="faq"></a>Veelgestelde vragen

1. Kan ik de virtuele machine waarop de configuratie-server is geïnstalleerd voor verschillende doeleinden gebruiken? **Geen**, configuratie-server moet een server met één doeleinde en gebruiken als een gedeelde server wordt niet ondersteund.
2. Kan ik overschakelen van de kluis die al zijn geregistreerd in de configuratieserver met een nieuwe kluis? **Geen**, zodra een kluis is geregistreerd met de configuratieserver, kan niet worden gewijzigd.
3. Kan ik dezelfde configuratieserver gebruiken voor het beveiligen van zowel fysieke en virtuele machines? **Ja**, configuratieserver met dezelfde kan worden gebruikt voor het repliceren van fysieke en virtuele machines. Echter, de failback naar een fysieke computer wordt niet ondersteund.
4. Waar worden configuratieserver gebruikt? Raadpleeg onze Azure Site Recovery-architectuur [hier](vmware-azure-architecture.md) voor meer informatie over de configuratieserver en de functionaliteiten.
5. Waar vind ik de nieuwste versie van de configuratieserver U kunt rechtstreeks downloaden van [Microsoft Download Center](https://aka.ms/asrconfigurationserver). Raadpleeg het artikel stappen om de upgrade configuratieserver [hier](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server).

## <a name="upgrade-the-configuration-server"></a>Upgrade de configuratieserver

Lees de stappen voor het upgraden van de configuratieserver naar de nieuwste versie [hier](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server)

## <a name="troubleshoot-deployment-issues"></a>Oplossen van implementatieproblemen

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Volgende stappen

Instellen van herstel na noodgevallen van [virtuele VMware-machines](vmware-azure-tutorial.md) naar Azure.
