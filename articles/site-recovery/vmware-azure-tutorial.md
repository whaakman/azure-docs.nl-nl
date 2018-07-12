---
title: Herstel na noodgevallen naar Azure instellen voor on-premises VMware-VM’s met Azure Site Recovery | Microsoft Docs
description: Leer hoe u herstel na noodgevallen naar Azure instelt voor on-premises VMware-VM’s met Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/06/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 93626e6a8f199841b285fb8a6e302e6c3054db0d
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37918030"
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Herstel van on-premises VMware-VM’s naar Azure na een noodgeval instellen

[Azure Site Recovery](site-recovery-overview.md) draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel (BCDR) door te zorgen dat uw zakelijke apps actief blijven tijdens geplande en ongeplande uitval. Site Recovery beheert en orkestreert noodherstel van on-premises machines en virtuele Azure-machines (VM's), met inbegrip van replicatie, failover en herstel.


In deze zelfstudie laten we u zien hoe u replicatie van een VMware-VM naar Azure met Azure Site Recovery kunt instellen en inschakelen. Zelfstudies zijn ontworpen om u te laten zien hoe u Site Recovery implementeert met basisinstellingen. In zelfstudies wordt de eenvoudigste methode gebruikt. Niet alle opties komen erin aan de orde. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De replicatiebron en het replicatiedoel invoeren.
> * De replicatiebronomgeving, waaronder on-premises Site Recovery-onderdelen, en de replicatiedoelomgeving instellen.
> * Een replicatiebeleid maken.
> * Replicatie voor een VM inschakelen.

## <a name="before-you-start"></a>Voordat u begint

Voordat u begint, is het handig om:

- [De architectuur te bekijken](vmware-azure-architecture.md) voor dit noodherstelscenario.
- Als u precies wilt weten hoe u herstel na noodgevallen voor virtuele VMware-machines kunt instellen, kunt u de volgende bronnen raadplegen:
    - [Veelgestelde vragen](vmware-azure-common-questions.md)te lezen over herstel na noodgevallen voor VMware.
    - [Uit te vinden](vmware-physical-azure-support-matrix.md) wat er door VMWare wordt ondersteund en wat er is vereist.
-  Onze **handleidingen** te lezen voor gedetailleerde instructies voor alle implementatieopties voor VMware:
    - Stel de [replicatiebron](vmware-azure-set-up-source.md) en de [configuratieserver](vmware-azure-deploy-configuration-server.md) in.
    - Stel het [replicatiedoel](vmware-azure-set-up-target.md) in.
    - Configureer een [replicatiebeleid](vmware-azure-set-up-replication.md) en schakel [replicatie in](vmware-azure-enable-replication.md).


## <a name="select-a-protection-goal"></a>Een beveiligingsdoel selecteren

1. Selecteer de naam van de kluis in **Recovery Services-kluizen**. We gebruiken **ContosoVMVault** voor dit scenario.
2. Selecteer in **Aan de slag** Site Recovery. Selecteer vervolgens **Infrastructuur voorbereiden**.
3. In **Beveiligingsdoel** > **Waar bevinden de machines zich**, selecteert u **On-premises**.
4. In **Waarnaartoe wilt u de machines repliceren** selecteert u **Naar Azure**.
5. In **Zijn de machines gevirtualiseerd** selecteert **Ja, met VMware vSphere Hypervisor**. Selecteer vervolgens **OK**.


## <a name="plan-your-deployment"></a>Uw implementatie plannen

In deze zelfstudie wordt getoond hoe u één virtuele machine kunt repliceren, en in **Implementatieplanning** selecteert u **Ja, ik dit hebt gedaan**. Als u meerdere virtuele machines implementeert, wordt u aangeraden deze stap niet over te slaan. We leveren het [hulpprogramma Deployment Planner Tool](https://aka.ms/asr-deployment-planner) om u daarbij te helpen. [Meer informatie](site-recovery-deployment-planner.md) over dit hulpprogramma.

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Als eerste stap bij een implementatie moet u uw bronomgeving instellen. Hiervoor hebt u één maximaal beschikbare on-premises computer nodig om de on-premises Site Recovery-onderdelen te hosten. Onderdelen zijn de configuratieserver, de processerver en de hoofddoelserver:

- De configuratieserver coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.
- De processerver fungeert als replicatiegateway. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag. De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.
- Op de hoofddoelserver worden de replicatiegegevens tijdens de failback vanuit Azure afgehandeld.

Om de configuratieserver in te stellen als een maximaal beschikbare VMware-VM, downloadt u een voorbereide OVA-sjabloon (Open Virtualization Application) en importeert u de sjabloon in VMware om de VM te maken. Nadat u de configuratieserver heb ingesteld, registreert u deze in de kluis. Na de registratie detecteert de Site Recovery on-premises VMware-VM’s.

> [!TIP]
> Deze zelfstudie gebruikt een OVA-sjabloon om de VM van de VMware-configuratieserver te maken. Als dit niet lukt, kunt u [de configuratieserver handmatig instellen](physical-manage-configuration-server.md).

> [!TIP]
> In deze zelfstudie wordt MySQL met behulp van Site Recovery op de configuratieserver gedownload en geïnstalleerd. Als u niet wilt dat Site Recovery dit doet, kunt dit handmatig installeren. [Meer informatie](vmware-azure-deploy-configuration-server.md#configure-settings).


### <a name="download-the-vm-template"></a>De VM-sjabloon downloaden

1. Ga in de kluis naar **Infrastructuur voorbereiden** > **Bron**.
2. Selecteer **+Configuratieserver** in **Bron voorbereiden**.
3. Controleer in **Server toevoegen** of **Configuratieserver voor VMware** wordt weergegeven in **Servertype**.
4. Download de OVF-sjabloon voor de configuratieserver.

 > [!TIP]
 >U kunt de nieuwste versie van de configuratieserversjabloon rechtstreeks downloaden vanuit het [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

>[!NOTE]
De licentie die wordt geleverd bij de OVF-sjabloon is een evaluatielicentie die 180 dagen geldig is. De klant moet Windows activeren met een gekochte licentie.

## <a name="import-the-template-in-vmware"></a>De sjabloon in VMware importeren

1. Meld u aan bij de VMware vCenter-server of vSphere ESXi-host met behulp van de VMWare vSphere-client.
2. Selecteer in het menu **Bestand** de optie **OVF-sjabloon implementeren** om de **wizard voor het implementeren van OVF-sjablonen** te starten. 

     ![OVF-sjabloon](./media/vmware-azure-tutorial/vcenter-wizard.png)

3. Voer in **Bron selecteren** de locatie van de gedownloade OVF in.
4. Selecteer **Volgende** in **Beoordelingsdetails**.
5. Accepteer de standaardinstellingen in **Naam en map selecteren** en **Configuratie selecteren**.
6. Selecteer in **Opslag selecteren****Thick Provision Eager Zeroed** in **Indeling virtuele schijf selecteren**.
7. Accepteer de standaardinstellingen in de rest van de wizardpagina's.
8. Bij **Gereed om te voltooien** selecteert u **Inschakelen na de implementatie** > **Voltooien** om de virtuele machine in te stellen met de standaardinstellingen.

    > [!TIP]
  Als u een extra NIC wilt toevoegen, wist u **Inschakelen na de implementatie** > **Voltooien**. De sjabloon bevat standaard één NIC. Na de implementatie kunt u meer NIC’s toevoegen.

## <a name="add-an-additional-adapter"></a>Een extra adapter toevoegen

Als u een extra NIC aan de configuratieserver wilt toevoegen, moet u dit doen voordat u de server in de kluis registreert. Het toevoegen van extra adapters wordt niet ondersteund na registratie.

1. Klik in de vSphere Client-inventaris met de rechtermuisknop op de VM en selecteer **Instellingen bewerken**.
2. Selecteer **Toevoegen** > **Ethernet-adapter** bij **Hardware**. Selecteer vervolgens **Volgende**.
3. Selecteer een adaptertype en een netwerk. 
4. Als u verbinding wilt maken met de virtuele NIC verbinding wanneer de VM wordt ingeschakeld, selecteert u **Verbinding maken bij inschakelen**. Selecteer **Volgende** > **voltooien**. Selecteer vervolgens **OK**.


## <a name="register-the-configuration-server"></a>De configuratieserver registreren 

1. Schakel de VM in vanuit de VMWare vSphere Client-console.
2. De VM wordt opgestart in een Windows Server 2016-installatie-ervaring. Accepteer de gebruiksrechtovereenkomst en voer een Administrator-wachtwoord in.
3. Meld u nadat de installatie is voltooid bij de virtuele machine aan als de administrator.
4. De eerste keer dat u zich aanmeldt, wordt het configuratieprogramma van Azure Site Recovery binnen enkele seconden gestart.
5. Voer een naam in die wordt gebruikt voor het registreren van de configuratieserver bij Site Recovery. Selecteer vervolgens **Volgende**.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, selecteert u **Aanmelden** om u aan te melden bij uw Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u de configuratieserver wilt registreren.
7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
8. Meld u opnieuw aan bij de machine. Na enkele seconden wordt de wizard voor het beheer van de configuratieserver automatisch gestart.

### <a name="configure-settings-and-add-the-vmware-server"></a>Instellingen configureren en de VMware-server toevoegen

1. Selecteer in de wizard voor het beheer van de configuratieserver **Connectiviteit instellen** en selecteer vervolgens de NIC die door de processerver wordt gebruikt voor het ontvangen van replicatieverkeer van VM's. Selecteer vervolgens **Opslaan**. U kunt deze instelling niet wijzigen nadat deze is geconfigureerd.
2. Selecteer in **Selecteer Recovery Services-kluis** uw Azure-abonnement en de relevante resourcegroep en kluis.
3. Accepteer de gebruiksrechtovereenkomst in **Software van derden installeren**. Selecteer **Downloaden en installeren** om MySQL Server te installeren. Als u MySQL in het pad hebt geplaatst, wordt deze stap overgeslagen.
4. Selecteer **VMware PowerCLI installeren**. Zorg ervoor dat alle browservensters zijn gesloten voordat u dit doet. Selecteer vervolgens **Doorgaan**.
5. In **De configuratie van het apparaat valideren** worden de vereisten gecontroleerd voordat u doorgaat.
6. In **vCenter Server vSphere/ESXi-server configureren** voert u de FQDN of het IP-adres van de vCenter-server, of vSphere-host, in waar de VM's die u wilt repliceren zich bevinden. Voer de poort in waarop de server luistert. Voer een beschrijvende naam in voor de VMware-server in de kluis.
7. Voer referenties in die door de configuratieserver moeten worden gebruikt voor verbinding met de VMware-server. Site Recovery gebruikt deze referenties voor het automatisch detecteren van VMware-VM’s die beschikbaar zijn voor replicatie. Selecteer **Toevoegen** en vervolgens **Doorgaan**.
8. Voer in **Referenties voor virtuele machine configureren** de gebruikersnaam en het wachtwoord in die worden gebruikt om Mobility Service automatisch op virtuele machines te installeren wanneer replicatie wordt ingeschakeld.
    - Voor Windows-machines moet het account lokale administrator-machtigingen hebben op de machines die u wilt repliceren.
    - Geef voor Linux de details voor de superuser op.
9. Selecteer **Configuratie voltooien** om de registratie te voltooien.
10. Controleer nadat de registratie is voltooid in Azure Portal of de configuratieserver en de VMware-server worden weergegeven op de pagina **Bron** in de kluis. Selecteer vervolgens **OK** om de doelinstellingen te configureren.


Site Recovery maakt verbinding met de VMware-servers met behulp van de opgegeven instellingen en de VM's worden gedetecteerd.

> [!NOTE]
> Het duurt 15 minuten of langer voordat de accountnaam wordt weergegeven in de portal. Selecteer **Configuratieservers** > ***servernaam*** > **Server vernieuwen** om direct bij te werken.

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen.

1. Selecteer **Infrastructuur voorbereiden** > **Doel**. Selecteer het Azure-abonnement dat u wilt gebruiken. Er wordt gebruikgemaakt van een Resource Manager-model.
2. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt. U zou deze moeten hebben als u de Azure-onderdelen in het [eerste deel](tutorial-prepare-azure.md) van deze reeks zelfstudies hebt ingesteld.

   ![Doeltabblad](./media/vmware-azure-tutorial/storage-network.png)

## <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

1. Open [Azure Portal](https://portal.azure.com) en selecteer **Alle resources**.
2. Selecteer de Recovery Services-kluis (**ContosoVMVault** in deze zelfstudie).
3. Selecteer **Infrastructuur voor Site Recovery** > **Herstelbeleid** > **+Herstelbeleid** om een replicatiebeleid te maken.
4. Voer bij **Replicatiebeleid maken** de naam van het beleid in. Hiervoor wordt **VMwareRepPolicy** gebruikt.
5. Gebruik in **RPO-drempelwaarde** de standaardwaarde van 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
6. Bij **Bewaarperiode van het herstelpunt** geeft u op hoe lang elk herstelpunt moet worden bewaard. Voor deze zelfstudie wordt 72 uur gebruikt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
7. Bij **Frequentie van de app-consistente momentopname** geeft u op hoe vaak app-consistente momentopnamen moeten worden gemaakt. Gebruik de standaardwaarde van 60 minuten. Selecteer **OK** om het beleid te maken.

   ![Replicatiebeleid maken](./media/vmware-azure-tutorial/replication-policy.png)

- Het beleid wordt automatisch gekoppeld aan de configuratieserver.
- Standaard wordt automatisch een bijbehorend beleid voor failback gemaakt. Als het replicatiebeleid bijvoorbeeld**repbeleid** is, is het failbackbeleid **repbeleid-failback**. Dit beleid wordt pas gebruikt als u een failback initieert vanuit Azure.

## <a name="enable-replication"></a>Replicatie inschakelen

U kunt replicatie als volgt inschakelen:

1. Selecteer **Toepassing repliceren** > **Bron**.
2. Bij **Bron** selecteert u **On-premises** en selecteert u de configuratieserver bij **Bronlocatie**.
3. Selecteer **Virtuele machines** in **Type machine**.
4. Selecteer bij **vCenter/vSphere-hypervisor** de vSphere-host of de vCenter-server waarmee de host wordt beheerd.
5. Selecteer de processerver (standaard geïnstalleerd op de VM met de rol van configuratieserver). Selecteer vervolgens **OK**.
6. Selecteer in **Doel** het abonnement en de resourcegroep waarin u de failover-VM's wilt maken. Het implementatiemodel van Resource Manager wordt gebruikt. 
7. Selecteer het Azure-opslagaccount dat u wilt gebruiken om gegevens te repliceren, en het Azure-netwerk en -subnet waarmee virtuele Azure-machines verbinding maken wanneer ze na een failover worden gemaakt.
8. Selecteer **Nu configureren voor geselecteerde machines** om de netwerkinstelling toe te passen op alle virtuele machines waarop u replicatie inschakelt. Selecteer **Later configureren** om per machine een Azure-netwerk te selecteren.
9. Selecteer in **Virtuele machines** > **Virtuele machines selecteren** alle machines die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Selecteer vervolgens **OK**.
10. Selecteer in **Eigenschappen** > **Eigenschappen configureren** het account dat door de processerver moet worden gebruikt om automatisch Mobility Service op de computer te installeren.
11. Controleer of het juiste replicatiebeleid is geselecteerd in **Replicatie-instellingen** > **Replicatie-instellingen configureren**.
12. Selecteer **Replicatie inschakelen**. Site Recovery installeert de Mobility-service wanneer replicatie wordt ingeschakeld voor een VM.
13. U kunt de voortgang van de taak **Beveiliging inschakelen** volgen via **Instellingen** > **Taken** > **Site Recovery-taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.
- Het kan 15 minuten of langer duren voordat wijzigingen zijn doorgevoerd en in de portal worden weergegeven.
- Voor het bewaken van virtuele machines die u toevoegt, controleert u de laatste detectietijd voor VM's in **Configuratieservers** > **Laatst contact om**. Als u VM's wilt toevoegen zonder te wachten op de geplande detectie, markeert u de configuratieserver (zonder deze te selecteren), en selecteert u **Vernieuwen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Noodherstelanalyse uitvoeren](site-recovery-test-failover-to-azure.md)
