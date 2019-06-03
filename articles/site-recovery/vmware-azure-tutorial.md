---
title: Herstel na noodgevallen naar Azure instellen voor on-premises VMware-VM’s met Azure Site Recovery | Microsoft Docs
description: Leer hoe u herstel na noodgevallen naar Azure instelt voor on-premises VMware-VM’s met Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: e93dbc072de7208927eba9e45c09143b56f52a9b
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418296"
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Herstel van on-premises VMware-VM’s naar Azure na een noodgeval instellen

In dit artikel wordt beschreven hoe u replicatie inschakelen voor on-premises VMware-VM voor herstel na noodgevallen naar Azure met de [Azure Site Recovery](site-recovery-overview.md) service.

Dit is de derde zelfstudie in een serie die laat zien u hoe u herstel na noodgevallen naar Azure instelt voor on-premises VMware-VM's. In de vorige zelfstudie we [de on-premises VMware-omgeving voorbereid](vmware-azure-tutorial-prepare-on-premises.md) voor herstel na noodgevallen naar Azure.


In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De bronreplicatie-instellingen en een on-premises Site Recovery-configuratieserver instellen.
> * Instellen van de replicatie-instellingen voor doel.
> * Een replicatiebeleid maken.
> * Schakel replicatie voor een VMware-VM.

> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario. Waar mogelijk wordt gebruikgemaakt van standaardopties en niet alle mogelijke instellingen en paden worden weergegeven. Lees het artikel in de sectie How To van de Site Recovery inhoudsopgave voor gedetailleerde instructies.

## <a name="before-you-start"></a>Voordat u begint

Voltooi de vorige zelfstudies:
1. Zorg ervoor dat u hebt [Azure instellen](tutorial-prepare-azure.md) voor noodherstel van on-premises VMware naar Azure.
2. Ga als volgt [stappen](vmware-azure-tutorial-prepare-on-premises.md) naar uw on-premises VMware-implementatie voorbereiden voor herstel na noodgevallen naar Azure.
3. In deze zelfstudie laten we zien u hoe u een enkele virtuele machine repliceert. Als u meerdere VMware-VM's implementeert moet u de [Tool voor implementatieplanning](https://aka.ms/asr-deployment-planner). [Meer informatie](site-recovery-deployment-planner.md) over dit hulpprogramma.
4. In deze zelfstudie wordt een aantal opties die u wilt mogelijk anders doen:
    - De zelfstudie maakt gebruik van een OVA-sjabloon te maken van de configuratieserver VMware-VM. Als dit niet mogelijk om een bepaalde reden, voert u de [deze instructies](physical-manage-configuration-server.md) handmatig instellen van de configuratieserver.
    - In deze zelfstudie Site Recovery automatisch gedownload en geïnstalleerd MySQL met de configuratieserver. Als u liever, kunt u dit handmatig instellen in plaats daarvan. [Meer informatie](vmware-azure-deploy-configuration-server.md#configure-settings).




## <a name="select-a-protection-goal"></a>Een beveiligingsdoel selecteren

1. Selecteer de naam van de kluis in **Recovery Services-kluizen**. We gebruiken **ContosoVMVault** voor dit scenario.
2. Selecteer in **Aan de slag** Site Recovery. Selecteer vervolgens **Infrastructuur voorbereiden**.
3. In **Beveiligingsdoel** > **Waar bevinden de machines zich**, selecteert u **On-premises**.
4. In **Waarnaartoe wilt u de machines repliceren** selecteert u **Naar Azure**.
5. In **Zijn de machines gevirtualiseerd** selecteert **Ja, met VMware vSphere Hypervisor**. Selecteer vervolgens **OK**.



## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

In uw bronomgeving moet u een enkele, maximaal beschikbare on-premises computer voor het hosten van deze on-premises Site Recovery-onderdelen:

- **Configuratieserver**: De configuratieserver coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.
- **Processerver**: De processerver fungeert als replicatiegateway. Deze ontvangt replicatiegegevens; optimaliseert met caching, compressie en codering, en verzendt ze naar een cache-opslagaccount in Azure. De processerver installeert ook de Mobility-Service-agent op virtuele machines die u repliceren wilt, en detecteert automatisch on-premises virtuele VMware-machines.
- **Hoofddoelserver**: Op de hoofddoelserver worden de replicatiegegevens tijdens de failback vanuit Azure afgehandeld.


Al deze onderdelen samen zijn geïnstalleerd op de één on-premises virtuele machines die bekend staat als de *configuratieserver*. Voor herstel na noodgevallen VMware, wordt standaard ingesteld we van de configuratieserver als een maximaal beschikbare VMware-VM. Om dit te doen, kunt u een voorbereide Open Virtualization-toepassing (OVA)-sjabloon downloaden en importeren van de sjabloon in VMware om de VM te maken. 

- De meest recente versie van de configuratieserver is beschikbaar in de portal. U kunt ook downloaden deze rechtstreeks vanuit de [Microsoft Download Center](https://aka.ms/asrconfigurationserver).
- Als u niet voor een of andere reden een OVA-sjabloon gebruiken voor het instellen van een virtuele machine, voert u de [deze instructies](physical-manage-configuration-server.md) handmatig instellen van de configuratieserver.
- De licentie die is opgegeven met behulp van OVF-sjabloon is een van de evaluatielicentie is geldig gedurende 180 dagen. Windows die wordt uitgevoerd op de virtuele machine moet worden geactiveerd met de vereiste licentie. 


### <a name="download-the-vm-template"></a>De VM-sjabloon downloaden

1. Ga in de kluis naar **Infrastructuur voorbereiden** > **Bron**.
2. Selecteer **+Configuratieserver** in **Bron voorbereiden**.
3. Controleer in **Server toevoegen** of **Configuratieserver voor VMware** wordt weergegeven in **Servertype**.
4. Download de OVF-sjabloon voor de configuratieserver.



## <a name="import-the-template-in-vmware"></a>De sjabloon in VMware importeren


1. Meld u aan bij de VMware vCenter-server of vSphere ESXi-host met behulp van de VMWare vSphere-client.
2. Selecteer in het menu **Bestand** de optie **OVF-sjabloon implementeren** om de **wizard voor het implementeren van OVF-sjablonen** te starten. 

     ![OVF-sjabloon](./media/vmware-azure-tutorial/vcenter-wizard.png)

3. Voer in **Bron selecteren** de locatie van de gedownloade OVF in.
4. Selecteer **Volgende** in **Beoordelingsdetails**.
5. Accepteer de standaardinstellingen in **Naam en map selecteren** en **Configuratie selecteren**.
6. Selecteer in **Opslag selecteren** **Thick Provision Eager Zeroed** in **Indeling virtuele schijf selecteren**.
7. Accepteer de standaardinstellingen in de rest van de wizardpagina's.
8. Bij **Gereed om te voltooien** selecteert u **Inschakelen na de implementatie** > **Voltooien** om de virtuele machine in te stellen met de standaardinstellingen.

   > [!TIP]
   > Als u een extra NIC wilt toevoegen, wist u **Inschakelen na de implementatie** > **Voltooien**. De sjabloon bevat standaard één NIC. Na de implementatie kunt u meer NIC’s toevoegen.

## <a name="add-an-additional-adapter"></a>Een extra adapter toevoegen

Als u toevoegen van een extra NIC aan de configuratieserver wilt, moet u het toevoegen voordat u de server in de kluis registreert. Het toevoegen van extra adapters wordt niet ondersteund na registratie.

1. Klik in de vSphere Client-inventaris met de rechtermuisknop op de VM en selecteer **Instellingen bewerken**.
2. Selecteer **Toevoegen** > **Ethernet-adapter** bij **Hardware**. Selecteer vervolgens **Volgende**.
3. Selecteer een adaptertype en een netwerk. 
4. Als u verbinding wilt maken met de virtuele NIC verbinding wanneer de VM wordt ingeschakeld, selecteert u **Verbinding maken bij inschakelen**. Selecteer **Volgende** > **voltooien**. Selecteer vervolgens **OK**.


## <a name="register-the-configuration-server"></a>De configuratieserver registreren 

Nadat de configuratieserver is ingesteld, registreert u deze in de kluis.

1. Schakel de VM in vanuit de VMWare vSphere Client-console.
2. De VM wordt opgestart in een Windows Server 2016-installatie-ervaring. Accepteer de gebruiksrechtovereenkomst en voer een Administrator-wachtwoord in.
3. Meld u nadat de installatie is voltooid bij de virtuele machine aan als de administrator.
4. De eerste keer dat u zich aanmeldt, wordt het configuratieprogramma van Azure Site Recovery binnen enkele seconden gestart.
5. Voer een naam in die wordt gebruikt voor het registreren van de configuratieserver bij Site Recovery. Selecteer vervolgens **Volgende**.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, selecteert u **Aanmelden** om u aan te melden bij uw Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u de configuratieserver wilt registreren.
7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
8. Meld u opnieuw aan bij de machine. Na enkele seconden wordt de wizard voor het beheer van de configuratieserver automatisch gestart.


### <a name="configure-settings-and-add-the-vmware-server"></a>Instellingen configureren en de VMware-server toevoegen

Einddatum instellen en de configuratieserver te registreren. 


1. Selecteer in de configuratie van wizard voor het beheer, **connectiviteit instellen**. In de vervolgkeuzelijsten, selecteert u eerst de NIC die gebruikmaakt van de ingebouwde processerver voor detectie en push-installatie van de mobility-service op bronmachines en selecteer vervolgens de NIC die gebruikmaakt van de configuratieserver voor connectiviteit met Azure. Selecteer vervolgens **Opslaan**. U kunt deze instelling niet wijzigen nadat deze geconfigureerd.
2. Selecteer in **Selecteer Recovery Services-kluis** uw Azure-abonnement en de relevante resourcegroep en kluis.
3. Accepteer de gebruiksrechtovereenkomst in **Software van derden installeren**. Selecteer **Downloaden en installeren** om MySQL Server te installeren. Als u MySQL in het pad hebt geplaatst, wordt deze stap overgeslagen.
4. Selecteer **VMware PowerCLI installeren**. Zorg ervoor dat alle browservensters zijn gesloten voordat u dit doet. Selecteer vervolgens **Doorgaan**.
5. In **De configuratie van het apparaat valideren** worden de vereisten gecontroleerd voordat u doorgaat.
6. In **vCenter Server vSphere/ESXi-server configureren** voert u de FQDN of het IP-adres van de vCenter-server, of vSphere-host, in waar de VM's die u wilt repliceren zich bevinden. Voer de poort in waarop de server luistert. Voer een beschrijvende naam in voor de VMware-server in de kluis.
7. Voer gebruikersreferenties in die door de configuratieserver moeten worden gebruikt voor verbinding met de VMware-server. Zorg ervoor dat de gebruikersnaam en het wachtwoord juist zijn en deel uitmaken van de groep Administrators van de virtuele machine die moet worden beveiligd. Site Recovery gebruikt deze referenties voor het automatisch detecteren van VMware-VM’s die beschikbaar zijn voor replicatie. Selecteer **Toevoegen** en vervolgens **Doorgaan**.
8. Voer in **Referenties voor virtuele machine configureren** de gebruikersnaam en het wachtwoord in die worden gebruikt om Mobility Service automatisch op virtuele machines te installeren wanneer replicatie wordt ingeschakeld.
    - Voor Windows-machines moet het account lokale administrator-machtigingen hebben op de machines die u wilt repliceren.
    - Geef voor Linux de details voor de superuser op.
9. Selecteer **Configuratie voltooien** om de registratie te voltooien.
10. Controleer nadat de registratie is voltooid in Azure Portal of de configuratieserver en de VMware-server worden weergegeven op de pagina **Bron** in de kluis. Selecteer vervolgens **OK** om de doelinstellingen te configureren.


Nadat de configuratieserver is geregistreerd, Site Recovery maakt verbinding met de VMware-servers met behulp van de opgegeven instellingen en VM's worden gedetecteerd.

> [!NOTE]
> Het duurt 15 minuten of langer voordat de accountnaam wordt weergegeven in de portal. Selecteer **Configuratieservers** > ***servernaam*** > **Server vernieuwen** om direct bij te werken.

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen.

1. Selecteer **Infrastructuur voorbereiden** > **Doel**. Selecteer het Azure-abonnement dat u wilt gebruiken. Er wordt gebruikgemaakt van een Resource Manager-model.
2. Site Recovery controleert of dat u een of meer virtuele netwerken hebt. U zou deze moeten hebben als u de Azure-onderdelen in het [eerste deel](tutorial-prepare-azure.md) van deze reeks zelfstudies hebt ingesteld.

   ![Doeltabblad](./media/vmware-azure-tutorial/storage-network.png)

## <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

1. Open [Azure Portal](https://portal.azure.com) en selecteer **Alle resources**.
2. Selecteer de Recovery Services-kluis (**ContosoVMVault** in deze zelfstudie).
3. Selecteer **Infrastructuur voor Site Recovery** > **Herstelbeleid** >  **+Herstelbeleid** om een replicatiebeleid te maken.
4. Voer bij **Replicatiebeleid maken** de naam van het beleid in. Hiervoor wordt **VMwareRepPolicy** gebruikt.
5. Gebruik in **RPO-drempelwaarde** de standaardwaarde van 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
6. Bij **Bewaarperiode van het herstelpunt** geeft u op hoe lang elk herstelpunt moet worden bewaard. Voor deze zelfstudie wordt 72 uur gebruikt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
7. Bij **Frequentie van de app-consistente momentopname** geeft u op hoe vaak app-consistente momentopnamen moeten worden gemaakt. Gebruik de standaardwaarde van 60 minuten. Selecteer **OK** om het beleid te maken.

   ![Replicatiebeleid maken](./media/vmware-azure-tutorial/replication-policy.png)

- Het beleid wordt automatisch gekoppeld aan de configuratieserver.
- Standaard wordt automatisch een bijbehorend beleid voor failback gemaakt. Als het replicatiebeleid bijvoorbeeld**repbeleid** is, is het failbackbeleid **repbeleid-failback**. Dit beleid wordt pas gebruikt als u een failback initieert vanuit Azure.

## <a name="enable-replication"></a>Replicatie inschakelen

Replicatie voor virtuele machines als volgt inschakelen:

1. Selecteer **Toepassing repliceren** > **Bron**.
1. Bij **Bron** selecteert u **On-premises** en selecteert u de configuratieserver bij **Bronlocatie**.
1. Selecteer **Virtuele machines** in **Type machine**.
1. Selecteer bij **vCenter/vSphere-hypervisor** de vSphere-host of de vCenter-server waarmee de host wordt beheerd.
1. Selecteer de processerver (standaard geïnstalleerd op de VM met de rol van configuratieserver). Selecteer vervolgens **OK**. Status van de processerver wordt aangegeven aan de hand van aanbevolen limieten en andere parameters. Kies een processerver in orde. Een [kritieke](vmware-physical-azure-monitor-process-server.md#process-server-alerts) processerver kan niet worden gekozen. U kunt ofwel [problemen op te lossen](vmware-physical-azure-troubleshoot-process-server.md) de fouten **of** instellen van een [uitbreidbare processerver](vmware-azure-set-up-process-server-scale.md).
1. Selecteer in **Doel** het abonnement en de resourcegroep waarin u de failover-VM's wilt maken. Het implementatiemodel van Resource Manager wordt gebruikt. 
1. Selecteer het Azure-netwerk en -subnet waarmee virtuele Azure-machines verbinding maken wanneer ze na een failover worden gemaakt.
1. Selecteer **Nu configureren voor geselecteerde machines** om de netwerkinstelling toe te passen op alle virtuele machines waarop u replicatie inschakelt. Selecteer **Later configureren** om per machine een Azure-netwerk te selecteren.
1. Selecteer in **Virtuele machines** > **Virtuele machines selecteren** alle machines die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Selecteer vervolgens **OK**. Als u niet kunt weergeven of een bepaalde virtuele machine te selecteren [meer](https://aka.ms/doc-plugin-VM-not-showing) over het oplossen van het probleem.
1. Selecteer in **Eigenschappen** > **Eigenschappen configureren** het account dat door de processerver moet worden gebruikt om automatisch Mobility Service op de computer te installeren.
1. Controleer of het juiste replicatiebeleid is geselecteerd in **Replicatie-instellingen** > **Replicatie-instellingen configureren**.
1. Selecteer **Replicatie inschakelen**. Site Recovery installeert de Mobility-service wanneer replicatie wordt ingeschakeld voor een VM.
1. U kunt de voortgang van de taak **Beveiliging inschakelen** volgen via **Instellingen** > **Taken** > **Site Recovery-taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.
1. Het kan 15 minuten of langer duren voordat wijzigingen zijn doorgevoerd en in de portal worden weergegeven.
1. Voor het bewaken van virtuele machines die u toevoegt, controleert u de laatste detectietijd voor VM's in **Configuratieservers** > **Laatst contact om**. Als u VM's wilt toevoegen zonder te wachten op de geplande detectie, markeert u de configuratieserver (zonder deze te selecteren), en selecteert u **Vernieuwen**.

## <a name="next-steps"></a>Volgende stappen
Voer na het inschakelen van replicatie, een detailanalyse om te controleren of dat alles werkt zoals verwacht.
> [!div class="nextstepaction"]
> [Noodherstelanalyse uitvoeren](site-recovery-test-failover-to-azure.md)
