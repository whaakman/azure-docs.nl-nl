---
title: "Herstel na noodgevallen naar Azure instellen voor on-premises VMware-VM’s met Azure Site Recovery | Microsoft Docs"
description: "Leer hoe u herstel na noodgevallen naar Azure instelt voor on-premises VMware-VM’s met de Azure Site Recovery-service."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 3d9248d2501c7fea0492bad2687b6bdfb0b903e8
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Herstel van on-premises VMware-VM’s naar Azure na een noodgeval instellen

In deze zelfstudie leert u hoe u herstel na noodgevallen naar Azure instelt voor on-premises VMware-VM’s waarop Windows wordt uitgevoerd. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De replicatiebron en het replicatiedoel opgeven.
> * De bronreplicatieomgeving, waaronder on-premises Site Recovery-onderdelen, en de replicatiedoelomgeving instellen.
> * Een replicatiebeleid maken
> * Replicatie inschakelen voor een VM

Dit is de derde zelfstudie in een reeks. In deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid:

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises VMware voorbereiden](tutorial-prepare-on-premises-vmware.md)

Voordat u begint, is het handig om [de architectuur](concepts-vmware-to-azure-architecture.md) voor noodherstelscenario’s te bekijken.


## <a name="select-a-replication-goal"></a>Een replicatiedoel selecteren

1. Klik in **Recovery Services-kluizen** op de kluisnaam, **ContosoVMVault**.
2. Klik in **Aan de slag** op Site Recovery. Klik vervolgens op **Infrastructuur voorbereiden**.
3. In **Beveiligingsdoel** > **Waar bevinden de machines zich**, selecteert u **On-premises**.
4. In **Waarnaartoe wilt u de machines repliceren** selecteert u **Naar Azure**.
5. In **Zijn de machines gevirtualiseerd** selecteert **Ja, met VMware vSphere Hypervisor**. Klik vervolgens op **OK**.

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Voor het instellen van de bronomgeving hebt u één maximaal beschikbare on-premises computer nodig om de on-premises Site Recovery-onderdelen te hosten. Onderdelen zijn de configuratieserver, de processerver en de hoofddoelserver.

- De configuratieserver coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.
- De processerver fungeert als replicatiegateway. Dit onderdeel ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en codering, en verzendt ze naar de Azure-opslag. De processerver installeert ook de Mobility-service op VM’s die u wilt repliceren, en detecteert automatisch on-premises VMware-VM’s.
- Op de hoofddoelserver worden de replicatiegegevens tijdens de failback vanuit Azure afgehandeld.

Om de configuratieserver in te stellen als een maximaal beschikbare VMware-VM, downloadt u een voorbereide OVF-sjabloon en importeert u de sjabloon in VMware om de VM te maken. Na het instellen van de configuratieserver registreert u deze in de kluis. Na de registratie detecteert de Site Recovery on-premises VMware-VM’s.

### <a name="download-the-vm-template"></a>De VM-sjabloon downloaden

1. Ga in de kluis naar **Infrastructuur voorbereiden** > **Bron**.
2. Klik in **Bron voorbereiden** op **+Configuratieserver**.
3. Controleer in **Server toevoegen** of **Configuratieserver voor VMware** wordt weergegeven in **Servertype**.
4. Download de OVF-sjabloon (Open Virtualization Format) voor de configuratieserver.

  > [!TIP]
  De nieuwste versie van de configuratieserversjabloon kan rechtstreeks vanuit het [Microsoft Download Center](https://aka.ms/asrconfigurationserver) worden gedownload.

## <a name="import-the-template-in-vmware"></a>De sjabloon in VMware importeren

1. Meld u bij de VMware vCenter-server of vSphere ESXi-host met behulp van de VMWare vSphere-client.
2. Selecteer in het menu **Bestand** de optie **OVF-sjabloon implementeren** om de wizard voor het implementeren van OVF-sjablonen te starten.  

     ![OVF-sjabloon](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. Geef in **Bron selecteren** de locatie van de gedownloade OVF op.
4. Klik in **Details bekijken** op **Volgende**.
5. Accepteer de standaardinstellingen in **Naam en map selecteren** en **Configuratie selecteren**.
6. Selecteer in **Opslag selecteren****Thick Provision Eager Zeroed** in **Indeling virtuele schijf selecteren**.
4. Accepteer de standaardinstellingen in de rest van de wizardpagina’s.
5. In **Klaar om te voltooien**:
  - Selecteer **Inschakelen na de implementatie** > **Voltooien** om de VM in te stellen met de standaardinstellingen.
  - Als u een extra netwerkinterface wilt toevoegen, wist u **Inschakelen na de implementatie** en selecteert u **Voltooien**. Standaard wordt de configuratieserversjabloon met één NIC geïmplementeerd, maar na de implementatie kunt u extra NIC’s toevoegen.

  
## <a name="add-an-additional-adapter"></a>Een extra adapter toevoegen

Als u een extra NIC aan de configuratieserver wilt toevoegen, moet u dit doen voordat u de server in de kluis registreert. Het toevoegen van extra adapters wordt niet ondersteund na registratie.

1. Klik in de vSphere Client-inventaris met de rechtermuisknop op de VM en selecteer **Instellingen bewerken**.
2. Klik in **Hardware** op **Toevoegen** > **Ethernet-adapter**. Klik op **Volgende**.
3. Selecteer een adaptertype en een netwerk. 
4. Als u verbinding wilt maken met de virtuele NIC verbinding wanneer de VM wordt ingeschakeld, selecteert u **Verbinding maken bij inschakelen**. Klik op **Volgende** > **Voltooien**, en klik vervolgens op **OK**.


## <a name="register-the-configuration-server"></a>De configuratieserver registreren 

1. Schakel de VM in vanuit de VMWare vSphere Client-console.
2. De VM wordt opgestart in een Windows Server 2016-installatie-ervaring. Accepteer de gebruiksrechtovereenkomst en geef een Administrator-wachtwoord op.
3. Meld u nadat de installatie is voltooid bij de virtuele machine aan als de administrator.
4. De eerste keer dat u zich aanmeldt, wordt het configuratieprogramma van Azure Site Recovery gestart.
5. Geef een naam die wordt gebruikt voor het registreren van de configuratieserver bij Site Recovery. Klik op **Volgende**.
6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, klikt u op **Aanmelden** om u aan te melden bij uw Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u de configuratieserver wilt registreren.
7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
8. Meld u weer aan bij de computer. De wizard voor het beheer van de configuratieserver wordt automatisch gestart.

### <a name="configure-settings-and-connect-to-vmware"></a>Instellingen configureren en verbinding maken met VMware

1. Selecteer in de wizard voor het beheer van de configuratieserver > **Connectiviteit instellen** de NIC die replicatieverkeer gaat ontvangen. Klik vervolgens op **Opslaan**. U kunt deze instelling niet wijzigen nadat deze geconfigureerd.
2. Selecteer in **Selecteer Recovery Services-kluis** uw Azure-abonnement en de relevante resourcegroep en kluis.
3. Accepteer de gebruiksrechtovereenkomst in **Software van derden installeren** en klik op **Downloaden en installeren** om MySQL-Server te installeren.
4. Klik op **VMware PowerCLI installeren**. Zorg ervoor dat alle browservensters zijn gesloten voordat u dit doet. Klik vervolgens op **Doorgaan**
5. In **De configuratie van het apparaat valideren** worden de vereisten gecontroleerd voordat u doorgaat.
6. In **vCenter Server vSphere/ESXi-server configureren**, geeft u de FQDN of het IP-adres van de vCenter-server, of vSphere-host, op waar de VM’s die u wilt repliceren zich bevinden. Geef de poort waarop de server luistert, en geef een beschrijvende naam op die moet worden gebruikt voor de VMware-server in de kluis.
7. Geef referenties op die door de configuratieserver worden gebruikt voor verbinding met de VMware-server. Site Recovery gebruikt deze referenties voor het automatisch detecteren van VMware-VM’s die beschikbaar zijn voor replicatie. Klik op **Toevoegen** en vervolgens op **Doorgaan**.
8. Voer in **Referenties voor virtuele machine configureren** de gebruikersnaam en het wachtwoord in die worden gebruikt om de Mobility-service automatisch op computers te installeren wanneer replicatie wordt ingeschakeld. Voor Windows-machines moet het account lokale administrator-machtigingen hebben op de machines die u wilt repliceren. Geef voor Linux de details voor de superuser op.
9. Klik op **Configuratie voltooien**  om de registratie te voltooien. 
10. Controleer nadat de registratie is voltooid in Azure Portal of de configuratieserver en de VMware-server worden weergegeven op de pagina **Bron** in de kluis. Klik vervolgens op **OK** om de doelinstellingen te configureren.


Site Recovery maakt verbinding met de VMware-servers met de opgegeven instellingen en de VM’s worden gedetecteerd.

> [!NOTE]
> Het duurt 15 minuten of langer voordat de accountnaam wordt weergegeven in de portal. Klik op **Configuratieservers** > ***servernaam*** > **Server vernieuwen** om direct bij te werken.

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen.

1. Klik op **Infrastructuur voorbereiden** > **Doel** en selecteer het Azure-abonnement dat u wilt gebruiken.
2. Geef op of uw doelimplementatiemodel gebaseerd is op Resource Manager of klassiek is.
3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.

   ![Doel](./media/tutorial-vmware-to-azure/storage-network.png)

## <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

1. Open [Azure Portal](https://portal.azure.com) en klik op **Alle resources**.
2. Klik op de Recovery Service-kluis met de naam **ContosoVMVault**.
3. Klik op **Infrastructuur voor Site Recovery** > **Herstelbeleid** > **+Herstelbeleid** om een replicatiebeleid te maken.
4. Geef in **Replicatiebeleid maken** de beleidsnaam **VMwareRepPolicy** op.
5. Gebruik in **RPO-drempelwaarde** de standaardwaarde van 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
6. Gebruik in **Bewaarperiode van het herstelpunt** de standaardwaarde van 24 uur als de bewaarperiode voor elk herstelpunt. Voor deze zelfstudie selecteren we 72 uur. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
7. Gebruik in **Frequentie van de app-consistente momentopname** de standaardwaarde van 60 minuten voor de frequentie waarmee app-consistente momentopnamen worden gemaakt. Klik op **OK** om het beleid te maken.

   ![Beleid](./media/tutorial-vmware-to-azure/replication-policy.png)

Het beleid wordt automatisch gekoppeld aan de configuratieserver. Standaard wordt automatisch een bijbehorend beleid gemaakt voor failback. Als het replicatiebeleid bijvoorbeeld**repbeleid** is wordt het failbackbeleid **repbeleid-failback** gemaakt. Dit beleid wordt pas gebruikt als u een failback initieert vanuit Azure.

## <a name="enable-replication"></a>Replicatie inschakelen

Site Recovery installeert de Mobility-service wanneer replicatie wordt ingeschakeld voor een VM. Het kan 15 minuten of langer duren voordat wijzigingen zijn doorgevoerd en in de portal worden weergegeven.

Schakel als volgt replicatie in:

1. Klik op **Toepassing repliceren** > **Bron**.
2. Selecteer in **Bron** de configuratieserver.
3. Selecteer **Virtuele machines** in **Type machine**.
4. Selecteer in **vCenter/vSphere-hypervisor** de vCenter-server waarmee de vSphere-host wordt beheerd, of selecteer de host.
5. Selecteer de processerver (configuratieserver). Klik vervolgens op **OK**.
6. Selecteer in **Doel** en selecteer het abonnement en de resourcegroep waarin u de failover-VM’s wilt maken. Kies het implementatiemodel dat u wilt gebruiken in Azure (klassiek of resourcebeheer) voor de failover-VM's.
7. Selecteer het Azure-opslagaccount dat u wilt gebruiken voor het repliceren van gegevens.
8. Selecteer het Azure-netwerk en -subnet waarmee virtuele Azure-machines verbinding maken wanneer ze na een failover worden gemaakt.
9. Selecteer **Nu configureren voor geselecteerde machines** om de netwerkinstelling toe te passen op alle machines die u voor beveiliging selecteert. Selecteer **Later configureren** om per machine een Azure-netwerk te selecteren.
10. Selecteer in **Virtuele machines** > **Virtuele machines selecteren** alle machines die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**.
11. Selecteer in **Eigenschappen** > **Eigenschappen configureren** het account dat door de processerver zal worden gebruikt om automatisch de Mobility-service op de computer te installeren.
12. Controleer of het juiste replicatiebeleid is geselecteerd in **Replicatie-instellingen** > **Replicatie-instellingen configureren**.
13. Klik op **Replicatie inschakelen**.

U kunt de voortgang van de taak **Beveiliging inschakelen** volgen via **Instellingen** > **Taken** > **Site Recovery-taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.

Voor het bewaken van virtuele machines die u toevoegt, kunt u de laatste detectietijd voor VM’s controleren in **Configuratieservers**
> **Laatste contact om**. Als u VM’s wilt toevoegen zonder te wachten op de geplande detectie, markeert u de configuratieserver (klik er niet op), en klikt u op **Vernieuwen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Noodherstelanalyse uitvoeren](site-recovery-test-failover-to-azure.md)
