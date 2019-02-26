---
title: SQL Server-beschikbaarheidsgroepen - virtuele Machines van Azure - zelfstudie | Microsoft Docs
description: Deze zelfstudie laat zien hoe u een SQL Server AlwaysOn-beschikbaarheidsgroep maken op Azure Virtual Machines.
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 08a00342-fee2-4afe-8824-0db1ed4b8fca
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/30/2018
ms.author: mikeray
ms.openlocfilehash: 61a05f912e8b8e0b954dc43faeb7d6e35a08b657
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56820141"
---
# <a name="configure-always-on-availability-group-in-azure-vm-manually"></a>Configure AlwaysOn-beschikbaarheidsgroep in Azure VM handmatig

Deze zelfstudie laat zien hoe u een SQL Server AlwaysOn-beschikbaarheidsgroep maken op Azure Virtual Machines. De volledige zelfstudie maakt u een beschikbaarheidsgroep toevoegen met een databasereplica op twee SQL-Servers.

**Geschatte tijd**: Duurt circa 30 minuten om te voltooien als aan de vereisten is voldaan.

Het diagram ziet wat u in de zelfstudie maken.

![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="prerequisites"></a>Vereisten

De zelfstudie wordt ervan uitgegaan dat u een basiskennis hebben van SQL Server Always On Availability Groups. Als u meer informatie, Zie [overzicht van de Always On Availability Groups (SQL Server)](https://msdn.microsoft.com/library/ff877884.aspx).

De volgende tabel bevat de vereisten die u nodig hebt om te voltooien voordat u deze zelfstudie begint:

|  |Vereiste |Description |
|----- |----- |----- |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png) | Twee SQL-Servers | -In een Azure-beschikbaarheidsset <br/> -In één domein <br/> -Met de functie Failover Clustering geïnstalleerd |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)| Windows Server | File share voor een cluster witness |  
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server-serviceaccount | Domeinaccount |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server Agent-serviceaccount | Domeinaccount |  
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Firewall-poorten openen | -SQL Server: **1433** voor standaardexemplaar <br/> -Eindpunt voor databasespiegeling: **5022** of een beschikbare poort <br/> -Beschikbaarheidsgroep load balancer-statustest voor IP-adres: **59999** of een beschikbare poort <br/> -Statustest core load balancer IP-adres van cluster: **58888** of een beschikbare poort |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Toevoegen van de functie Failoverclustering | Deze functie nodig hebben voor zowel SQL-Servers |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Domeinaccount voor installatie | -Lokale beheerder zijn op elke SQL-Server <br/> -Lid is van SQL Server vaste serverrol sysadmin voor elk exemplaar van SQL Server  |


Voordat u met de zelfstudie begint, moet u [voldoen aan vereisten voor het maken van Always On Availability Groups in Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md). Als u deze vereisten zijn al voltooid, kunt u springen naar [Cluster maken](#CreateCluster).

  >[!NOTE]
  > Veel van de stappen in deze zelfstudie kunnen nu worden geautomatiseerd met [Azure SQL VM CLI](virtual-machines-windows-sql-availability-group-cli.md) en [Azure-Snelstartsjablonen](virtual-machines-windows-sql-availability-group-quickstart-template.md).


<!--**Procedure**: *This is the first “step”. Make titles H2’s and short and clear – H2’s appear in the right pane on the web page and are important for navigation.*-->

<a name="CreateCluster"></a>
## <a name="create-the-cluster"></a>Het cluster maken

Nadat de vereiste onderdelen zijn voltooid, wordt de eerste stap is het maken van een Windows Server-failovercluster met twee SQL-servers en een witness-server.

1. RDP-verbinding de eerste SQL-Server met behulp van een domeinaccount dat een beheerder is op zowel SQL-Servers en de witness-server.

   >[!TIP]
   >Als u hebt gevolgd de [document met vereiste onderdelen](virtual-machines-windows-portal-sql-availability-group-prereq.md), u hebt gemaakt met een account met de naam **CORP\Install**. Dit account gebruiken.

2. In de **Serverbeheer** dashboard, selecteer **extra**, en klik vervolgens op **Failoverclusterbeheer**.
3. In het linkerdeelvenster met de rechtermuisknop op **Failoverclusterbeheer**, en klik vervolgens op **maken van een Cluster**.
   ![Cluster maken](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)
4. Maak een cluster met één knooppunt te doorlopen via de pagina's met de instellingen in de volgende tabel in de Wizard Cluster maken:

   | Pagina | Instellingen |
   | --- | --- |
   | Voordat u begint |Standaardinstellingen gebruiken |
   | Selecteer Servers |Typ de naam van de eerste SQL-Server in **Enter servernaam** en klikt u op **toevoegen**. |
   | Van validatiewaarschuwing |Selecteer **Nee. ik niet ondersteuning van Microsoft nodig hebt voor dit cluster, en daarom niet wilt de validatietests uit te voeren. Wanneer ik op Volgende klikt, gaat u door het maken van het cluster**. |
   | Toegangspunt voor beheer van het Cluster |Typ de naam van een cluster, zoals **SQLAGCluster1** in **clusternaam**.|
   | Bevestiging |Gebruik de standaardinstellingen, tenzij u van opslagruimten gebruikmaakt. Zie de opmerking onder deze tabel. |

### <a name="set-the-windows-server-failover-cluster-ip-address"></a>Windows server failover cluster-IP-adres instellen

1. In **Failoverclusterbeheer**, schuif omlaag naar **Clusterkernresources** en vouwt u de clusterdetails van de. Ziet u zowel de **naam** en de **IP-adres** bronnen in de **mislukt** staat. De resource van het IP-adres kan niet online worden gebracht omdat het cluster is toegewezen hetzelfde IP-adres als de machine zelf, daarom is het een dubbel adres.

2. Met de rechtermuisknop op de mislukte **IP-adres** resource en klik vervolgens op **eigenschappen**.

   ![Eigenschappen van cluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/42_IPProperties.png)

3. Selecteer **statisch IP-adres** en geeft u een beschikbaar adres uit hetzelfde subnet bevinden als uw virtuele machines.

4. In de **Clusterkernresources** sectie, met de rechtermuisknop op de naam van cluster en klikt u op **Online brengen**. Klik, wacht u totdat beide bronnen online zijn. Wanneer de naam van clusterresource online gaat, wordt de DC-server met een nieuwe AD-account bijgewerkt. Deze AD-account voor de beschikbaarheidsgroep geclusterde service later gebruiken.

### <a name="addNode"></a>De andere SQL-Server toevoegen aan cluster

De andere SQL-Server toevoegen aan het cluster.

1. In de browserstructuur van de met de rechtermuisknop op het cluster en klikt u op **knooppunt toevoegen**.

    ![Knooppunt toevoegen aan het Cluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/44-addnode.png)

1. In de **Wizard knooppunt toevoegen**, klikt u op **volgende**. In de **Servers selecteren** pagina, de tweede SQL-Server toevoegen. Typ de naam van de server in **Enter servernaam** en klik vervolgens op **toevoegen**. Wanneer u klaar bent, klikt u op **volgende**.

1. In de **Validation Warning** pagina, klikt u op **Nee** (in een productiescenario moet u uitvoeren de validatietests). Klik op **Volgende**.

8. In de **bevestiging** pagina als u gebruikmaakt van opslagruimten, schakel het selectievakje met het label **alle in aanmerking komende opslag toevoegen aan het cluster.**

   ![Bevestiging van het knooppunt toevoegen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/46-addnodeconfirmation.png)

    >[!WARNING]
   >Als u van opslagruimten gebruikmaakt en niet doen Schakel **alle in aanmerking komende opslag toevoegen aan het cluster**, Windows losgekoppeld van de virtuele schijven tijdens het clusterproces. Als gevolg hiervan worden niet weergegeven in Schijfbeheer of Explorer totdat de opslagruimten zijn verwijderd uit het cluster en opnieuw gekoppeld met behulp van PowerShell. Storage Spaces gegroepeerd meerdere schijven in opslaggroepen. Zie voor meer informatie, [opslagruimten](https://technet.microsoft.com/library/hh831739).

1. Klik op **volgende**.

1. Klik op **Voltooien**.

   Failover Cluster Manager laat zien dat het cluster een nieuw knooppunt heeft en wordt weergegeven in de **knooppunten** container.

10. Meld u af bij van de extern bureaublad-sessiehost.

### <a name="add-a-cluster-quorum-file-share"></a>Een clusterquorum-bestandsshare toevoegen

In dit voorbeeld gebruikt de Windows-cluster een bestandsshare te maken van een clusterquorum. In deze zelfstudie maakt gebruik van een knooppunt en bestandssharemeerderheid quorum. Zie voor meer informatie, [Quorumconfiguraties in een failovercluster](https://technet.microsoft.com/library/cc731739.aspx).

1. Verbinding maken met de file share witness lidserver met een extern bureaublad-sessiehost.

1. Op **Serverbeheer**, klikt u op **extra**. Open **Computerbeheer**.

1. Klik op **gedeelde mappen**.

1. Met de rechtermuisknop op **Shares**, en klikt u op **nieuwe Share...** .

   ![Nieuwe Share](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Gebruik **maken van een Wizard gedeelde map** om een bestandsshare te maken.

1. Op **mappad**, klikt u op **Bladeren** en zoekt of maakt u een pad voor de gedeelde map. Klik op **volgende**.

1. In **naam, beschrijving en instellingen** controleren of de sharenaam en het pad. Klik op **volgende**.

1. Op **machtigingen voor gedeelde mappen** ingesteld **machtigingen aanpassen**. Klik op **aangepaste...** .

1. Op **machtigingen aanpassen**, klikt u op **toevoegen...** .

1. Zorg ervoor dat het account dat wordt gebruikt voor het maken van het cluster volledig beheer heeft.

   ![Nieuwe Share](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/50-filesharepermissions.png)

1. Klik op **OK**.

1. In **machtigingen voor gedeelde mappen**, klikt u op **voltooien**. Klik op **voltooien** opnieuw.  

1. Meld u af bij de server

### <a name="configure-cluster-quorum"></a>Clusterquorum configureren

Vervolgens stelt u het clusterquorum.

1. Verbinding maken met het eerste clusterknooppunt met extern bureaublad.

1. In **Failoverclusterbeheer**, met de rechtermuisknop op het cluster, wijs **meer acties**, en klikt u op **Clusterquoruminstellingen configureren...** .

   ![Nieuwe Share](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/52-configurequorum.png)

1. In **Wizard clusterquorum configureren**, klikt u op **volgende**.

1. In **optie voor quorumconfiguratie**, kiest u **selecteert u de quorumwitness**, en klikt u op **volgende**.

1. Op **Quorumwitness selecteren**, klikt u op **een bestandsshare-witness configureren**.

   >[!TIP]
   >Windows Server 2016 biedt ondersteuning voor een cloud-witness. Als u dit type witness kiest, u hoeft niet een bestand witness delen. Zie voor meer informatie, [een cloudwitness voor een Failover-Cluster implementeren](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness). In deze zelfstudie maakt gebruik van een bestandssharewitness wordt ondersteund door eerdere besturingssystemen.

1. Op **bestandssharewitness configureren**, typt u het pad voor de share die u hebt gemaakt. Klik op **volgende**.

1. Controleer de instellingen op **bevestiging**. Klik op **volgende**.

1. Klik op **Voltooien**.

De clusterkernresources zijn geconfigureerd met een bestandsshare-witness.

## <a name="enable-availability-groups"></a>Beschikbaarheidsgroepen inschakelen

Schakel vervolgens de **AlwaysOn Availability Groups** functie. Voer deze stappen uit op beide SQL-Servers.

1. Uit de **Start** scherm, start u **SQL Server Configuration Manager**.
2. Klik in de browserstructuur van de op **SQL Server-Services**, klik met de rechtermuisknop op de **SQL Server (MSSQLSERVER)** service en klikt u op **eigenschappen**.
3. Klik op de **AlwaysOn High Availability** tabblad, en selecteer vervolgens **Schakel AlwaysOn Availability Groups**, als volgt:

    ![AlwaysOn-beschikbaarheidsgroepen inschakelen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/54-enableAlwaysOn.png)

4. Klik op **Toepassen**. Klik op **OK** in het pop-updialoogvenster.

5. Start de SQL Server-service opnieuw.

Herhaal deze stappen op de SQL-Server.

<!-----------------
## <a name="endpoint-firewall"></a>Open firewall for the database mirroring endpoint

Each instance of SQL Server that participates in an Availability Group requires a database mirroring endpoint. This endpoint is a TCP port for the instance of SQL Server that is used to synchronize the database replicas in the Availability Groups on that instance.

On both SQL Servers, open the firewall for the TCP port for the database mirroring endpoint.

1. On the first SQL Server **Start** screen, launch **Windows Firewall with Advanced Security**.
2. In the left pane, select **Inbound Rules**. On the right pane, click **New Rule**.
3. For **Rule Type**, choose **Port**.
1. For the port, specify TCP and choose an unused TCP port number. For example, type *5022* and click **Next**.

   >[!NOTE]
   >For this example, we're using TCP port 5022. You can use any available port.

5. In the **Action** page, keep **Allow the connection** selected and click **Next**.
6. In the **Profile** page, accept the default settings and click **Next**.
7. In the **Name** page, specify a rule name, such as **Default Instance Mirroring Endpoint** in the **Name** text box, then click **Finish**.

Repeat these steps on the second SQL Server.
-------------------------->

## <a name="create-a-database-on-the-first-sql-server"></a>Maak een database in de eerste SQL-Server

1. Start het RDP-bestand met de eerste SQL-Server met een domeinaccount dat lid is van de vaste serverrol sysadmin.
1. Open SQL Server Management Studio en maak verbinding met de eerste SQL-Server.
7. In **Objectverkenner**, met de rechtermuisknop op **Databases** en klikt u op **nieuwe Database**.
8. In **databasenaam**, type **MyDB1**, klikt u vervolgens op **OK**.

### <a name="backupshare"></a> Maak een back-upshare

1. Op de eerste SQL-Server in **Serverbeheer**, klikt u op **extra**. Open **Computerbeheer**.

1. Klik op **gedeelde mappen**.

1. Met de rechtermuisknop op **Shares**, en klikt u op **nieuwe Share...** .

   ![Nieuwe Share](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Gebruik **maken van een Wizard gedeelde map** om een bestandsshare te maken.

1. Op **mappad**, klikt u op **Bladeren** en zoekt of maakt u een pad voor de database een back-up gedeelde map. Klik op **volgende**.

1. In **naam, beschrijving en instellingen** controleren of de sharenaam en het pad. Klik op **volgende**.

1. Op **machtigingen voor gedeelde mappen** ingesteld **machtigingen aanpassen**. Klik op **aangepaste...** .

1. Op **machtigingen aanpassen**, klikt u op **toevoegen...** .

1. Zorg ervoor dat de SQL Server en SQL Server Agent-service-accounts voor beide servers volledig beheer hebben.

   ![Nieuwe Share](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/68-backupsharepermission.png)

1. Klik op **OK**.

1. In **machtigingen voor gedeelde mappen**, klikt u op **voltooien**. Klik op **voltooien** opnieuw.  

### <a name="take-a-full-backup-of-the-database"></a>Een volledige back-up van de database uitvoeren

U moet back-up van de nieuwe database de logboekback-upketen initialiseren. Als u een back-up van de nieuwe database niet uitvoert, kan deze kan niet worden opgenomen in een beschikbaarheidsgroep.

1. In **Objectverkenner**, met de rechtermuisknop op de database, wijs **taken...** , klikt u op **Back-Up**.

1. Klik op **OK** een volledige back-up uitvoeren op de standaardlocatie van de back-up.

## <a name="create-the-availability-group"></a>De beschikbaarheidsgroep maken
U bent nu klaar om te configureren van een beschikbaarheidsgroep toevoegen met behulp van de volgende stappen uit:

* Maak een database op de eerste SQL-Server.
* Een volledige back-up-en een transactielogboekback-up van de database
* Herstellen van de volledige en logboekback-ups met de tweede SQL Server met de **NORECOVERY** optie
* De beschikbaarheidsgroep maken (**AG1**) met synchrone doorvoer, automatische failover en leesbare secundaire replica's

### <a name="create-the-availability-group"></a>De beschikbaarheidsgroep maken:

1. Op de externe bureaubladsessie naar de eerste SQL-Server. In **Objectverkenner** in SSMS, met de rechtermuisknop op **AlwaysOn High Availability** en klikt u op **Wizard Nieuwe beschikbaarheidsgroep**.

    ![Wizard Nieuwe beschikbaarheidsgroep starten](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/56-newagwiz.png)

2. In de **inleiding** pagina, klikt u op **volgende**. In de **naam van de beschikbaarheidsgroep opgeven** pagina, typ een naam voor de beschikbaarheidsgroep, bijvoorbeeld **AG1**in **naam van de beschikbaarheidsgroep**. Klik op **volgende**.

    ![Wizard Nieuwe AG, AG-naam opgeven](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/58-newagname.png)

3. In de **Databases selecteren** pagina, selecteer uw database en klik op **volgende**.

   >[!NOTE]
   >De database aan de vereisten voldoet voor een beschikbaarheidsgroep omdat u hebt ten minste één volledige back-up uitgevoerd op de beoogde primaire replica.

   ![Wizard Nieuwe AG, selecteert u Databases](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/60-newagselectdatabase.png)
4. In de **replica's opgeven** pagina, klikt u op **Replica toevoegen**.

   ![Wizard Nieuwe AG, replica's opgeven](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/62-newagaddreplica.png)
5. De **verbinding maken met Server** dialoogvenster wordt weergegeven. Typ de naam van de tweede server in **servernaam**. Klik op **Verbinden**.

   Klik in de **replica's opgeven** pagina u ziet nu de tweede server die worden vermeld in **Beschikbaarheidsreplica**. Als volgt te werk om de replica's te configureren.

   ![Wizard Nieuwe AG, geef replica's (voltooid)](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/64-newagreplica.png)

6. Klik op **eindpunten** om te zien van het eindpunt voor databasespiegeling voor deze beschikbaarheidsgroep. Gebruik dezelfde poort die u hebt gebruikt bij het instellen van de [firewall-regel voor eindpunten voor databasespiegeling](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

    ![Wizard Nieuwe AG, eerste gegevenssynchronisatie selecteren](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/66-endpoint.png)

8. In de **eerste gegevenssynchronisatie selecteren** pagina, selecteert u **volledige** en geef een gedeelde netwerklocatie. Voor de locatie, gebruikt u de [back-upshare die u hebt gemaakt](#backupshare). In het voorbeeld het geval is,  **\\ \\ \<eerst SQL Server\>\Backup\\**. Klik op **volgende**.

   >[!NOTE]
   >Volledige synchronisatie duurt een volledige back-up van de database op het eerste exemplaar van SQL Server en aan het tweede exemplaar te herstellen. Volledige synchronisatie wordt voor grote databases, niet aanbevolen omdat het lang duurt. U kunt dit moment beperken door handmatig een back-up maken van de database en het herstellen van met `NO RECOVERY`. Als de database is al hersteld met `NO RECOVERY` op de tweede SQL Server voordat u de beschikbaarheidsgroep configureert, kiest u **alleen deelnemen**. Als u wilt een back-up na de configuratie van de beschikbaarheidsgroep, kiest u **eerste gegevenssynchronisatie overslaan**.

    ![Wizard Nieuwe AG, eerste gegevenssynchronisatie selecteren](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/70-datasynchronization.png)

9. In de **validatie** pagina, klikt u op **volgende**. Deze pagina ziet die vergelijkbaar is met de volgende afbeelding:

    ![Wizard Nieuwe AG, validatie](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/72-validation.png)

    >[!NOTE]
    >Er is een waarschuwing voor de listenerconfiguratie van de omdat u niet een listener voor beschikbaarheidsgroep hebt geconfigureerd. U kunt deze waarschuwing negeren omdat op Azure virtual machines u de listener maken na het maken van de Azure load balancer.

10. In de **samenvatting** pagina, klikt u op **voltooien**, wacht u totdat de wizard configureert u de nieuwe beschikbaarheidsgroep. In de **voortgang** pagina, klikt u op **meer details** om de voortgang van de gedetailleerde weer te geven. Nadat de wizard voltooid is, Controleer de **resultaten** pagina om te controleren of de beschikbaarheidsgroep is gemaakt.

     ![Resultaten van de Wizard Nieuwe AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/74-results.png)
11. Klik op **sluiten** om de wizard af te sluiten.

### <a name="check-the-availability-group"></a>Controleer de beschikbaarheidsgroep.

1. In **Objectverkenner**, vouw **AlwaysOn High Availability**, vouw vervolgens **Availability Groups**. U ziet nu de nieuwe beschikbaarheidsgroep in deze container. Met de rechtermuisknop op de beschikbaarheidsgroep en klikt u op **Dashboard weergeven**.

   ![AG-Dashboard weergeven](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/76-showdashboard.png)

   Uw **AlwaysOn Dashboard** ziet er ongeveer als volgt uit.

   ![AG-Dashboard](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/78-agdashboard.png)

   U ziet de replica's, de failover-modus van elke replica en de synchronisatiestatus.

2. In **Failoverclusterbeheer**, klikt u op het cluster. Selecteer **rollen**. De naam van de beschikbaarheidsgroep die u gebruikt, is een rol op het cluster. Deze beschikbaarheidsgroep heeft geen een IP-adres voor clientverbindingen, omdat u niet een listener hebt geconfigureerd. U configureert de listener nadat u een Azure load balancer gemaakt.

   ![AG in Failoverclusterbeheer](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/80-clustermanager.png)

   > [!WARNING]
   > Probeer geen failover uitvoeren voor de beschikbaarheidsgroep van de Failover Cluster Manager. Alle failover-bewerkingen moeten worden uitgevoerd vanuit **AlwaysOn Dashboard** in SSMS. Zie voor meer informatie, [beperkingen op met behulp van de Failover-clusterbeheer met Availability Groups](https://msdn.microsoft.com/library/ff929171.aspx).
    >

Op dit moment hebt u een beschikbaarheidsgroep met replica's op twee exemplaren van SQL Server. U kunt de beschikbaarheidsgroep verplaatsen tussen exemplaren. U kan geen verbinding maken met de beschikbaarheidsgroep nog omdat u geen een listener. In Azure virtuele machines moet de listener van een load balancer. De volgende stap is het maken van de load balancer in Azure.

<a name="configure-internal-load-balancer"></a>

## <a name="create-an-azure-load-balancer"></a>Een Azure-load balancer maken

Een beschikbaarheidsgroep van SQL Server vereist op Azure virtual machines, een load balancer. De load balancer bevat de IP-adressen voor de beschikbaarheidsgroep-listeners en de Windows Server-failovercluster. In deze sectie bevat een overzicht van de load balancer maken in Azure portal.

Een Azure Load Balancer mag een Standard Load Balancer of een Basic Load Balancer. Standard Load Balancer heeft meer functionaliteit dan de basisversie van Load Balancer. Voor een beschikbaarheidsgroep, is de Standard Load Balancer is vereist als u een Beschikbaarheidszone (in plaats van een Beschikbaarheidsset) gebruiken. Zie voor meer informatie over het verschil tussen de load balancer-typen, [vergelijking van de Load Balancer-SKU](../../../load-balancer/load-balancer-overview.md#skus).

1. In de Azure-portal, gaat u naar de resourcegroep waar uw SQL-Servers zijn en klik op **+ toevoegen**.
1. Zoeken naar **Load Balancer**. Kies de load balancer gepubliceerd door Microsoft.

   ![AG in Failoverclusterbeheer](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/82-azureloadbalancer.png)

1. Klik op **Create**.
1. Configureer de volgende parameters voor de load balancer.

   | Instelling | Veld |
   | --- | --- |
   | **Naam** |Gebruik een naam in voor de load balancer, bijvoorbeeld **sqlLB**. |
   | **Type** |Intern |
   | **Virtueel netwerk** |Gebruik de naam van de Azure-netwerk. |
   | **Subnet** |Gebruik de naam van het subnet waarin de virtuele machine zich bevindt.  |
   | **IP-adrestoewijzing** |Statisch |
   | **IP-adres** |Gebruik een beschikbaar adres uit het subnet. Dit adres gebruiken voor de beschikbaarheidsgroeplistener. Houd er rekening mee dat dit af van het IP-adres van uw cluster wijkt.  |
   | **Abonnement** |Gebruik hetzelfde abonnement als de virtuele machine. |
   | **Locatie** |Gebruik de dezelfde locatie als de virtuele machine. |

   De blade in Azure portal ziet er als volgt:

   ![Load Balancer maken](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/84-createloadbalancer.png)

1. Klik op **maken**om te maken van de load balancer.

Voor het configureren van de load balancer, moet u een back-endadresgroep, een test maken en instellen van de load balancer-regels. Voer deze in de Azure-portal.

### <a name="add-backend-pool-for-the-availability-group-listener"></a>Back-endgroep toevoegen voor de beschikbaarheidsgroep-listener

1. In de Azure-portal, gaat u naar de beschikbaarheidsgroep. Mogelijk moet u de weergave om te zien van de zojuist gemaakte load balancer vernieuwen.

   ![Load Balancer niet vinden in de resourcegroep](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/86-findloadbalancer.png)

1. Klik op de load balancer, **back-endpools**, en klikt u op **+ toevoegen**.

1. Typ een naam voor de back-endpool.

1. Back-end-pool koppelen aan de beschikbaarheidsset met de virtuele machines.

1. Onder **doel-IP-configuraties van**, Controleer **virtuele MACHINE** en kies beide van de virtuele machines die als voor replica's van beschikbaarheidsgroepen host. Neem niet de file share witness-server.

   >[!NOTE]
   >Als beide virtuele machines niet opgegeven zijn, wordt alleen verbindingen met de primaire replica mislukt.

1. Klik op **OK** om de back endpool te maken.

### <a name="set-the-probe"></a>Instellen van de test

1. Klik op de load balancer, **statuscontroles**, en klikt u op **+ toevoegen**.

1. De statustest listener als volgt instellen:

   | Instelling | Description | Voorbeeld
   | --- | --- |---
   | **Naam** | Tekst | SQLAlwaysOnEndPointProbe |
   | **Protocol** | Kies TCP | TCP |
   | **Poort** | Een niet-gebruikte poort | 59999 |
   | **Interval**  | De tijdsduur tussen testpogingen in seconden |5 |
   | **Drempelwaarde voor onjuiste status** | Het aantal opeenvolgende testfouten dat moet optreden voor een virtuele machine om te worden beschouwd als niet in orde  | 2 |

1. Klik op **OK** om in te stellen de statustest.

### <a name="set-the-load-balancing-rules"></a>Instellen van de load balancer-regels

1. Klik op de load balancer, **Taakverdelingsregels**, en klikt u op **+ toevoegen**.

1. De listener-load balancer-regels als volgt instellen.
   | Instelling | Description | Voorbeeld
   | --- | --- |---
   | **Naam** | Tekst | SQLAlwaysOnEndPointListener |
   | **Frontend-IP-adres** | Kies een adres |Gebruik het adres dat u hebt gemaakt tijdens het maken van de load balancer. |
   | **Protocol** | Kies TCP |TCP |
   | **Poort** | Gebruik de poort voor de beschikbaarheidsgroep-listener | 1433 |
   | **Back-Endpoort** | Dit veld wordt niet gebruikt wanneer zwevend IP is ingesteld voor direct server return | 1433 |
   | **Probe** |De naam die u hebt opgegeven voor de test | SQLAlwaysOnEndPointProbe |
   | **Sessiepersistentie** | Vervolgkeuzelijst | **Geen** |
   | **Time-out voor inactiviteit** | Minuten dat een TCP-verbinding openen | 4 |
   | **Zwevend IP (direct server return)** | |Ingeschakeld |

   > [!WARNING]
   > Direct server return is ingesteld tijdens het maken van. De naam kan niet worden gewijzigd.

1. Klik op **OK** om in te stellen de listener-load balancer-regels.

### <a name="add-the-cluster-core-ip-address-for-the-windows-server-failover-cluster-wsfc"></a>Voeg het IP-adres van de cluster-core voor Windows Server Failover Cluster (WSFC)

Het WSFC-IP-adres moet ook worden op de load balancer.

1. Klik in de portal aan de dezelfde Azure load balancer, **Frontend-IP-configuratie** en klikt u op **+ toevoegen**. U hebt geconfigureerd voor het WSFC in de clusterkernresources IP-adres gebruiken. Het IP-adres instellen als statisch.

1. Klik op de load balancer, **statuscontroles**, en klikt u op **+ toevoegen**.

1. De statustest WSFC-cluster core IP-adres als volgt instellen:

   | Instelling | Description | Voorbeeld
   | --- | --- |---
   | **Naam** | Tekst | WSFCEndPointProbe |
   | **Protocol** | Kies TCP | TCP |
   | **Poort** | Een niet-gebruikte poort | 58888 |
   | **Interval**  | De tijdsduur tussen testpogingen in seconden |5 |
   | **Drempelwaarde voor onjuiste status** | Het aantal opeenvolgende testfouten dat moet optreden voor een virtuele machine om te worden beschouwd als niet in orde  | 2 |

1. Klik op **OK** om in te stellen de statustest.

1. Stel de load balancer-regels. Klik op **Taakverdelingsregels**, en klikt u op **+ toevoegen**.

1. Het cluster core IP-adres load balancer-regels als volgt instellen.
   | Instelling | Description | Voorbeeld
   | --- | --- |---
   | **Naam** | Tekst | WSFCEndPoint |
   | **Frontend-IP-adres** | Kies een adres |Het adres dat u hebt gemaakt tijdens het configureren van het WSFC-IP-adres gebruiken. Dit wijkt af van het listener-IP-adres |
   | **Protocol** | Kies TCP |TCP |
   | **Poort** | De poort voor de cluster-IP-adres gebruiken. Dit is een beschikbare poort die niet wordt gebruikt voor de test-listener-poort. | 58888 |
   | **Back-Endpoort** | Dit veld wordt niet gebruikt wanneer zwevend IP is ingesteld voor direct server return | 58888 |
   | **Probe** |De naam die u hebt opgegeven voor de test | WSFCEndPointProbe |
   | **Sessiepersistentie** | Vervolgkeuzelijst | **Geen** |
   | **Time-out voor inactiviteit** | Minuten dat een TCP-verbinding openen | 4 |
   | **Zwevend IP (direct server return)** | |Ingeschakeld |

   > [!WARNING]
   > Direct server return is ingesteld tijdens het maken van. De naam kan niet worden gewijzigd.

1. Klik op **OK** om in te stellen van de load balancer-regels.

## <a name="configure-listener"></a> Configureer de listener

Het volgende wat te doen is het configureren van een beschikbaarheidsgroep-listener op het failovercluster.

> [!NOTE]
> Deze zelfstudie laat zien hoe u één listener - met een ILB IP-adres maakt. Zie voor het maken van een of meer listeners met behulp van een of meer IP-adressen, [Create Availability Group-listener en load balancer | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-listener-port"></a>-Listener-poort instellen

Stel de listener-poort in SQL Server Management Studio.

1. Start SQL Server Management Studio en maak verbinding met de primaire replica.

1. Navigeer naar **AlwaysOn hoge beschikbaarheid** | **beschikbaarheidsgroepen** | **beschikbaarheidsgroep Listeners**.

1. U ziet nu de listener met de naam die u hebt gemaakt in Failoverclusterbeheer. Met de rechtermuisknop op de listenernaam van de en klikt u op **eigenschappen**.

1. In de **poort** vak, het poortnummer opgeven voor de beschikbaarheidsgroep-listener. 1433 is de standaardinstelling, klikt u op **OK**.

U hebt nu een SQL Server-beschikbaarheidsgroep in Azure virtuele machines die in Resource Manager-modus uitgevoerd.

## <a name="test-connection-to-listener"></a>Testen van verbinding met de listener

De verbinding wilt testen:

1. RDP naar een SQL-Server die zich in hetzelfde virtuele netwerk, maar is geen eigenaar van de replica. U kunt de SQL-Server gebruiken in het cluster.

1. Gebruik **sqlcmd** hulpprogramma om de verbinding te testen. Bijvoorbeeld, het volgende script maakt een **sqlcmd** verbinding met de primaire replica met de listener met de Windows-verificatie:

  ```cmd
  sqlcmd -S <listenerName> -E
  ```

  Als de listener met behulp van een andere poort dan de standaardwaarde standaardpoort (1433), de poort in de verbindingsreeks opgeven. Bijvoorbeeld, de volgende sqlcmd-opdracht is verbonden met een listener op poort 1435:

  ```cmd
  sqlcmd -S <listenerName>,1435 -E
  ```

De SQLCMD-verbinding wordt automatisch verbinding met elk exemplaar van SQL Server als host fungeert voor de primaire replica.

> [!TIP]
> Zorg ervoor dat de poort die u opgeeft geopend op de firewall van beide SQL-Servers is. Beide servers vereist een inkomende regel voor de TCP-poort die u gebruikt. Zie voor meer informatie, [toevoegen of bewerken firewallregel](https://technet.microsoft.com/library/cc753558.aspx).

## <a name="next-steps"></a>Volgende stappen

- [Een IP-adres toevoegen aan een load balancer voor een tweede beschikbaarheidsgroep](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).
