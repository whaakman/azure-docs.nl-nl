---
title: SQL Server-beschikbaarheidsgroepen - virtuele Machines in Azure - zelfstudie | Microsoft Docs
description: Deze zelfstudie laat zien hoe een SQL Server altijd op beschikbaarheidsgroep maken op Azure Virtual Machines.
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 08a00342-fee2-4afe-8824-0db1ed4b8fca
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/09/2017
ms.author: mikeray
ms.openlocfilehash: 228ca9ca5fddc493d27bfd6a40df5ee7306d6aa9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-always-on-availability-group-in-azure-vm-manually"></a>Configureren AlwaysOn-beschikbaarheidsgroep in Azure VM handmatig

Deze zelfstudie laat zien hoe een SQL Server altijd op beschikbaarheidsgroep maken op Azure Virtual Machines. De volledige zelfstudie maakt een beschikbaarheidsgroep met een databasereplica op twee SQL-Servers.

**Tijd schatting**: duurt ongeveer 30 minuten voltooid als aan de vereisten is voldaan.

Het diagram ziet u wat u in de zelfstudie bouwen.

![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="prerequisites"></a>Vereisten

De zelfstudie wordt ervan uitgegaan dat u basiskennis van SQL Server altijd op beschikbaarheidsgroepen. Als u meer informatie, Zie [overzicht van AlwaysOn-beschikbaarheidsgroepen (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).

De volgende tabel bevat de vereisten die u nodig hebt voltooid voordat u deze zelfstudie:

|  |Vereiste |Beschrijving |
|----- |----- |----- |
|![vierkante](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png) | Twee SQL-Servers | -In een Azure beschikbaarheidsset <br/> -In één domein <br/> -Met de functie Failover Clustering is geïnstalleerd |
|![vierkante](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)| Windows Server | Bestandsshare voor cluster-witness |  
|![vierkante](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server-serviceaccount | Domeinaccount |
|![vierkante](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server Agent-serviceaccount | Domeinaccount |  
|![vierkante](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Firewall-poorten | -SQL Server: **1433** voor het standaardexemplaar <br/> -Eindpunt voor databasespiegeling: **5022** of een beschikbare poort <br/> -Azure load balancer-test: **59999** of een beschikbare poort |
|![vierkante](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Toevoegen van de functie Failoverclustering | Deze functie nodig hebben voor zowel SQL-Servers |
|![vierkante](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Installatieaccount van het domein | -Lokale beheerder zijn op elke SQL Server <br/> -Lid is van SQL Server vaste serverrol sysadmin voor elk exemplaar van SQL Server  |


Voordat u de zelfstudie begint, moet u [voldoen aan vereisten voor het maken van AlwaysOn-beschikbaarheidsgroepen in Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md). Als deze vereiste onderdelen zijn al voltooid, kunt u springen naar [Cluster maken](#CreateCluster).


<!--**Procedure**: *This is the first “step”. Make titles H2’s and short and clear – H2’s appear in the right pane on the web page and are important for navigation.*-->

<a name="CreateCluster"></a>
##Het cluster maken

Nadat de vereiste onderdelen zijn voltooid, wordt de eerste stap is om een Windows Server-failovercluster met twee SQL-servers en een witness-server te maken.  

1. RDP naar de eerste SQL-Server met behulp van een domeinaccount met een Administrator op SQL-Servers en de witness-server.

   >[!TIP]
   >Als u hebt gevolgd de [vereisten document](virtual-machines-windows-portal-sql-availability-group-prereq.md), u hebt gemaakt met een account met de naam **CORP\Install**. Dit account gebruiken.

2. In de **Serverbeheer** dashboard, selecteer **extra**, en klik vervolgens op **Failoverclusterbeheer**.
3. In het linkerdeelvenster met de rechtermuisknop op **Failoverclusterbeheer**, en klik vervolgens op **maken van een Cluster**.
   ![Cluster maken](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)
4. Maak een cluster met één knooppunt door door de pagina's met de instellingen in de volgende tabel in de Wizard Cluster maken:

   | Pagina | Instellingen |
   | --- | --- |
   | Voordat u begint |Standaardinstellingen gebruiken |
   | Servers selecteren |Typ de naam van de eerste SQL Server in **Enter servernaam** en klik op **toevoegen**. |
   | Van validatiewaarschuwing |Selecteer **nr. ik geen ondersteuning van Microsoft nodig hebt voor dit cluster, en daarom niet wilt de validatietests uit te voeren. Als ik op Volgende klikt, doorgaan met het maken van het cluster**. |
   | Toegangspunt voor beheer van het Cluster |Typ de naam van een cluster, zoals **SQLAGCluster1** in **clusternaam**.|
   | Bevestiging |Gebruik de standaardwaarden tenzij u van opslagruimten gebruikmaakt. Zie de opmerking onder deze tabel. |

### <a name="set-the-cluster-ip-address"></a>De cluster-IP-adres instellen

1. In **Failoverclusterbeheer**, schuif omlaag naar **Clusterkernresources** en vouwt u de clusterdetails van de. Ziet u zowel de **naam** en de **IP-adres** bronnen in de **mislukt** status. De bron van het IP-adres kan niet online worden gebracht omdat het cluster is toegewezen aan hetzelfde IP-adres als de machine zelf, daarom is een dubbel adres.

2. Met de rechtermuisknop op de mislukte **IP-adres** bron en klik vervolgens op **eigenschappen**.

   ![Eigenschappen van cluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/42_IPProperties.png)

3. Selecteer **statisch IP-adres** en geef een beschikbaar adres van waar de SQL-Server zich in het tekstvak voor het bevindt subnet. Klik vervolgens op **OK**.
4. In de **Clusterkernresources** sectie, met de rechtermuisknop op de clusternaam van het en klik op **Online brengen**. Vervolgens wacht u totdat beide bronnen online zijn. Wanneer de clusterbron met de naam van online wordt gezet, de DC-server wordt bijgewerkt met een nieuw AD-account. Gebruik deze AD-account later uit te voeren de beschikbaarheidsgroep geclusterde service.

### <a name="addNode"></a>De SQL-Server toevoegen aan cluster

De SQL-Server toevoegen aan het cluster.

1. In de boomstructuur van de browser, met de rechtermuisknop op het cluster en klikt u op **knooppunt toevoegen**.

    ![Knooppunt toevoegen aan het Cluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/44-addnode.png)

1. In de **Wizard knooppunt toevoegen**, klikt u op **volgende**. In de **Servers selecteren** pagina, de tweede SQL-Server toevoegen. Typ de servernaam in **Enter servernaam** en klik vervolgens op **toevoegen**. Wanneer u klaar bent, klikt u op **volgende**.

1. In de **Validation Warning** pagina, klikt u op **Nee** (in een productiescenario u moet de validatietests uitgevoerd). Klik op **Volgende**.

8. In de **bevestiging** pagina als u gebruikmaakt van opslagruimten, schakelt u het selectievakje **alle in aanmerking komende opslag toevoegen aan het cluster.**

   ![Knooppunt bevestiging toevoegen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/46-addnodeconfirmation.png)

    >[!WARNING]
   >Als u met behulp van opslagruimten en niet doen schakelt **alle in aanmerking komende opslag toevoegen aan het cluster**, Windows wordt losgekoppeld van de virtuele schijven tijdens het clusterproces. Als gevolg hiervan worden niet weergegeven in Schijfbeheer of Explorer totdat de opslagruimten zijn verwijderd uit het cluster en opnieuw gekoppeld met behulp van PowerShell. Opslagruimten gegroepeerd op meerdere schijven in aan opslaggroepen. Zie voor meer informatie [opslagruimten](https://technet.microsoft.com/library/hh831739).

1. Klik op **Volgende**.

1. Klik op **Voltooien**.

   Failover Cluster Manager blijkt dat het cluster een nieuw knooppunt is en wordt weergegeven in de **knooppunten** container.

10. Meld u af bij van de extern bureaublad-sessiehost.

### <a name="add-a-cluster-quorum-file-share"></a>Toevoegen van een clusterquorum-bestandsshare

In dit voorbeeld gebruikt de Windows-cluster een bestandsshare te maken van een clusterquorum. Deze zelfstudie maakt gebruik van een knooppunt en bestandssharemeerderheid quorum. Zie voor meer informatie [Quorumconfiguraties in een failovercluster](http://technet.microsoft.com/library/cc731739.aspx).

1. Verbinding maken met de share witness lid bestandsserver met een extern bureaublad-sessiehost.

1. Op **Serverbeheer**, klikt u op **extra**. Open **Computerbeheer**.

1. Klik op **gedeelde mappen**.

1. Met de rechtermuisknop op **Shares**, en klik op **nieuwe Share...** .

   ![Nieuwe Share](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Gebruik **Wizard gedeelde map maken** voor het maken van een share.

1. Op **mappad**, klikt u op **Bladeren** en niet vinden of een pad voor de gedeelde map maken. Klik op **Volgende**.

1. In **naam, beschrijving en instellingen** Controleer of de sharenaam en het pad. Klik op **Volgende**.

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

1. In **Failoverclusterbeheer**, met de rechtermuisknop op het cluster, wijs **meer acties**, en klik op **Clusterquoruminstellingen configureren...** .

   ![Nieuwe Share](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/52-configurequorum.png)

1. In **Wizard clusterquorum configureren**, klikt u op **volgende**.

1. In **optie voor quorumconfiguratie**, kies **selecteert u de quorumwitness**, en klik op **volgende**.

1. Op **Quorumwitness selecteren**, klikt u op **een bestandssharewitness configureren**.

   >[!TIP]
   >Windows Server 2016 biedt ondersteuning voor een cloud-witness. Als u dit soort witness kiest, hoeft u niet een bestand witness delen. Zie voor meer informatie [een witness cloud voor een failover-Cluster implementeren](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness). Deze zelfstudie maakt gebruik van een bestandssharewitness wordt ondersteund door eerdere besturingssystemen.

1. Op **bestandssharewitness configureren**, typt u het pad voor de share die u hebt gemaakt. Klik op **Volgende**.

1. Controleer de instellingen op **bevestiging**. Klik op **Volgende**.

1. Klik op **Voltooien**.

De clusterkernresources zijn geconfigureerd met een bestandssharewitness.

## <a name="enable-availability-groups"></a>Beschikbaarheidsgroepen inschakelen

Schakel vervolgens de **AlwaysOn Availability Groups** functie. Voer deze stappen uit op beide SQL-Servers.

1. Van de **Start** scherm, starten **SQL Server Configuration Manager**.
2. Klik in de structuur van de browser op **SQL Server-Services**, klik met de rechtermuisknop op de **SQL Server (MSSQLSERVER)** service en klik op **eigenschappen**.
3. Klik op de **AlwaysOn hoge beschikbaarheid** tabblad en selecteer vervolgens **Schakel AlwaysOn Availability Groups**als volgt:

    ![AlwaysOn-beschikbaarheidsgroepen inschakelen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/54-enableAlwaysOn.png)

4. Klik op **Toepassen**. Klik op **OK** in het pop-upvenster.

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

## <a name="create-a-database-on-the-first-sql-server"></a>Een database maken op de eerste SQL-Server

1. Start het RDP-bestand naar de eerste SQL-Server met een domeinaccount dat lid is van de vaste serverrol sysadmin.
1. Open SQL Server Management Studio en maak verbinding met de eerste SQL-Server.
7. In **Objectverkenner**, met de rechtermuisknop op **Databases** en klik op **nieuwe Database**.
8. In **databasenaam**, type **MyDB1**, klikt u vervolgens op **OK**.

### <a name="backupshare"></a>Maak een back-share

1. Op de eerste SQL-Server in **Serverbeheer**, klikt u op **extra**. Open **Computerbeheer**.

1. Klik op **gedeelde mappen**.

1. Met de rechtermuisknop op **Shares**, en klik op **nieuwe Share...** .

   ![Nieuwe Share](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Gebruik **Wizard gedeelde map maken** voor het maken van een share.

1. Op **mappad**, klikt u op **Bladeren** en niet vinden of een pad voor de database een back-gedeelde map maken. Klik op **Volgende**.

1. In **naam, beschrijving en instellingen** Controleer of de sharenaam en het pad. Klik op **Volgende**.

1. Op **machtigingen voor gedeelde mappen** ingesteld **machtigingen aanpassen**. Klik op **aangepaste...** .

1. Op **machtigingen aanpassen**, klikt u op **toevoegen...** .

1. Zorg ervoor dat de SQL Server en SQL Server Agent-service-accounts voor beide servers volledig beheer hebben.

   ![Nieuwe Share](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/68-backupsharepermission.png)

1. Klik op **OK**.

1. In **machtigingen voor gedeelde mappen**, klikt u op **voltooien**. Klik op **voltooien** opnieuw.  

### <a name="take-a-full-backup-of-the-database"></a>Een volledige back-up van de database uitvoeren

U moet back-up van de nieuwe database in de logboekketen initialiseren. Als u een back-up van de nieuwe database pas van kracht worden, kan deze kan niet worden opgenomen in een beschikbaarheidsgroep.

1. In **Objectverkenner**, met de rechtermuisknop op de database, wijs **taken...** , klikt u op **Back-Up**.

1. Klik op **OK** te laten een volledige back-up op de standaardlocatie van de back-up.

## <a name="create-the-availability-group"></a>Maken van de beschikbaarheidsgroep.
U bent nu klaar om te configureren van een beschikbaarheidsgroep met behulp van de volgende stappen uit:

* Een database maken op de eerste SQL-Server.
* Neem een volledige back-up en de een transactielogboek van de database
* Herstellen van de volledige en logboekback-ups naar het tweede SQL Server met de **NORECOVERY** optie
* Maken van de beschikbaarheidsgroep (**AG1**) met synchrone doorvoer en automatische failover leesbare secundaire replica's

### <a name="create-the-availability-group"></a>De beschikbaarheidsgroep maken:

1. Op de extern-bureaubladsessie naar de eerste SQL-Server. In **Objectverkenner** SSMS, met de rechtermuisknop op **AlwaysOn hoge beschikbaarheid** en klik op **nieuwe Wizard beschikbaarheidsgroep**.

    ![De Wizard Nieuwe beschikbaarheidsgroep starten](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/56-newagwiz.png)

2. In de **inleiding** pagina, klikt u op **volgende**. In de **naam van de beschikbaarheidsgroep opgeven** pagina, typ een naam voor de beschikbaarheidsgroep, bijvoorbeeld **AG1**in **naam van de beschikbaarheidsgroep**. Klik op **Volgende**.

    ![Wizard Nieuwe AG AG-naam opgeven](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/58-newagname.png)

3. In de **Databases selecteren** pagina, selecteer uw database en klik op **volgende**.

   >[!NOTE]
   >De database voldoet aan de vereisten voor een beschikbaarheidsgroep, omdat u ten minste één volledige back-up hebt uitgevoerd op de beoogde primaire replica.

   ![Wizard Nieuwe AG Databases selecteren](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/60-newagselectdatabase.png)
4. In de **replica's opgeven** pagina, klikt u op **Replica toevoegen**.

   ![Wizard Nieuwe AG replica's opgeven](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/62-newagaddreplica.png)
5. De **verbinding maken met Server** dialoogvenster verschijnt. Typ de naam van de tweede server in **servernaam**. Klik op **Verbinden**.

   Terug in de **replica's opgeven** pagina u ziet nu de tweede server die worden vermeld in **Beschikbaarheidsreplica**. De replica's als volgt configureren.

   ![Wizard Nieuwe AG replica's (voltooid) opgeven](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/64-newagreplica.png)

6. Klik op **eindpunten** om te zien van het eindpunt voor databasespiegeling voor deze beschikbaarheidsgroep. Gebruik dezelfde poort die u hebt gebruikt bij het instellen van de [firewallregel voor databasespiegeling eindpunten](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

    ![Wizard Nieuwe AG eerste gegevenssynchronisatie selecteren](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/66-endpoint.png)

8. In de **eerste gegevenssynchronisatie selecteren** pagina **volledige** en geef een gedeelde netwerklocatie. Gebruik voor de locatie, de [back-upshare die u hebt gemaakt](#backupshare). In het voorbeeld was, **\\\\\<eerste SQL-Server\>\Backup\**. Klik op **Volgende**.

   >[!NOTE]
   >Volledige synchronisatie duurt een volledige back-up van de database op het eerste exemplaar van SQL Server en te herstellen op de tweede instantie. Voor grote databases worden volledige synchronisatie wordt niet aanbevolen omdat het kan lang duren. U kunt deze tijd verkorten door handmatig een back-up van de database maken en terugzetten met `NO RECOVERY`. Als de database al is teruggezet met `NO RECOVERY` op de tweede SQL Server voordat u de beschikbaarheidsgroep configureert, kiest u **alleen deelnemen**. Als u wilt de back-up uitvoeren na het configureren van de beschikbaarheidsgroep, kiest u **eerste gegevenssynchronisatie overslaan**.

    ![Wizard Nieuwe AG eerste gegevenssynchronisatie selecteren](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/70-datasynchronization.png)

9. In de **validatie** pagina, klikt u op **volgende**. Deze pagina moet er ongeveer als de volgende afbeelding:

    ![Wizard Nieuwe AG, validatie](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/72-validation.png)

    >[!NOTE]
    >Er is een waarschuwing voor de listenerconfiguratie van de omdat u een beschikbaarheidsgroep-listener niet hebt geconfigureerd. U kunt deze waarschuwing negeren omdat op virtuele machines in Azure maakt u de listener na het maken van de Azure load balancer.

10. In de **samenvatting** pagina, klikt u op **voltooien**, wacht u totdat de wizard configureert u de nieuwe beschikbaarheidsgroep. In de **voortgang** pagina, klikt u op **meer details** om de voortgang van de gedetailleerde weer te geven. Zodra de wizard voltooid is, controleert de **resultaten** pagina om te controleren of de beschikbaarheidsgroep is gemaakt.

     ![Wizard Nieuwe AG resulteert](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/74-results.png)
11. Klik op **sluiten** de wizard wilt afsluiten.

### <a name="check-the-availability-group"></a>Controleer de beschikbaarheidsgroep.

1. In **Objectverkenner**, vouw **AlwaysOn hoge beschikbaarheid**, vouw vervolgens **beschikbaarheidsgroepen**. U ziet nu de nieuwe beschikbaarheidsgroep in deze container. Met de rechtermuisknop op de beschikbaarheidsgroep en klik op **Dashboard weergeven**.

   ![AG-Dashboard weergeven](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/76-showdashboard.png)

   Uw **AlwaysOn Dashboard** ziet er ongeveer als volgt.

   ![AG-Dashboard](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/78-agdashboard.png)

   U ziet de replica's, de failover-modus van elke replica en de synchronisatiestatus.

2. In **Failoverclusterbeheer**, klikt u op het cluster. Selecteer **rollen**. De naam van de beschikbaarheidsgroep die u gebruikt, is een rol op het cluster. Deze beschikbaarheidsgroep heeft geen een IP-adres voor clientverbindingen, omdat u niet een listener hebt geconfigureerd. U configureert de listener nadat u een Azure load balancer maken.

   ![AG in Failoverclusterbeheer](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/80-clustermanager.png)

   > [!WARNING]
   > Probeer geen failover van de beschikbaarheidsgroep vanuit Failoverclusterbeheer. Alle failover-bewerkingen moeten worden uitgevoerd vanuit **AlwaysOn Dashboard** in SSMS. Zie voor meer informatie [beperkingen op met behulp van de Failover-clusterbeheer met beschikbaarheidsgroepen](https://msdn.microsoft.com/library/ff929171.aspx).
    >

U hebt op dit moment een beschikbaarheidsgroep met replica's op twee exemplaren van SQL Server. U kunt de beschikbaarheidsgroep verplaatsen tussen exemplaren. U kan geen verbinding met de beschikbaarheidsgroep nog omdat u niet over een listener. In Azure virtuele machines moet de listener voor een load balancer. De volgende stap is het maken van de load balancer in Azure.

<a name="configure-internal-load-balancer"></a>

## <a name="create-an-azure-load-balancer"></a>Een Azure load balancer maken

Op virtuele machines in Azure vereist een SQL Server-beschikbaarheidsgroep een load balancer. De load balancer bevat het IP-adres voor de beschikbaarheidsgroep-listener. Deze sectie bevat een overzicht van de load balancer maken in de Azure-portal.

1. In de Azure portal, gaat u naar de resourcegroep waar uw SQL-Servers zijn en klik op **+ toevoegen**.
2. Zoeken naar **Load Balancer**. Kies de load balancer gepubliceerd door Microsoft.

   ![AG in Failoverclusterbeheer](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/82-azureloadbalancer.png)

1.  Klik op **Create**.
3. Configureer de volgende parameters voor de load balancer.

   | Instelling | Veld |
   | --- | --- |
   | **Naam** |Gebruik een naam in voor de load balancer, zoals **sqlLB**. |
   | **Type** |interne |
   | **Virtueel netwerk** |Gebruik de naam van de virtuele Azure-netwerk. |
   | **Subnet** |De naam van het subnet dat de virtuele machine is in gebruik.  |
   | **IP-adrestoewijzing** |Statisch |
   | **IP-adres** |Gebruik een beschikbaar adres van subnet. |
   | **Abonnement** |Gebruik hetzelfde abonnement als de virtuele machine. |
   | **Locatie** |Gebruik dezelfde locatie als de virtuele machine. |

   De Azure portal blade ziet er als volgt:

   ![Load Balancer maken](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/84-createloadbalancer.png)

1. Klik op **maken**wilt maken van de load balancer.

Voor het configureren van de load balancer, moet u een back-endpool een test maken en de load-balancingregels. Voer deze in de Azure-portal.

### <a name="add-backend-pool"></a>Back-endpool toevoegen

1. Ga in de Azure-portal aan de beschikbaarheidsgroep. Mogelijk moet u de weergave voor de zojuist gemaakte load balancer vernieuwen.

   ![Load Balancer niet vinden in de resourcegroep](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/86-findloadbalancer.png)

1. Klik op de load balancer, **back-endpools**, en klik op **+ toevoegen**. De back-endpool als volgt instellen:

   | Instelling | Beschrijving | Voorbeeld
   | --- | --- |---
   | **Naam** | Typ een naam | SQLLBBE
   | **Gekoppeld aan** | Kies uit de lijst | Beschikbaarheidsset
   | **Beschikbaarheidsset** | Gebruik een naam van de beschikbaarheidsset die uw SQL Server-VM's in | sqlAvailabilitySet |
   | **Virtuele machines** |De twee namen van de virtuele machine in Azure SQL-Server | SQL Server-0, SQL Server-1

1. Typ de naam voor de back-end-adresgroep.

1. Klik op **+ toevoegen van een virtuele machine**.

1. Kies dat de beschikbaarheidsset dat de SQL-Servers zijn voor de beschikbaarheidsset.

1. Voor virtuele machines zijn beide van de SQL-Servers. De bestandsserver van de bestandsshare-witness niet opnemen.

1. Klik op **OK** om de back endpool te maken.

### <a name="set-the-probe"></a>Instellen van de test

1. Klik op de load balancer, **statuscontroles**, en klik op **+ toevoegen**.

1. De health test als volgt instellen:

   | Instelling | Beschrijving | Voorbeeld
   | --- | --- |---
   | **Naam** | Tekst | SQLAlwaysOnEndPointProbe |
   | **Protocol** | Kies TCP | TCP |
   | **Poort** | Een niet-gebruikte poort | 59999 |
   | **Interval**  | De tijdsduur tussen testpogingen in seconden |5 |
   | **Drempelwaarde voor onjuiste status** | Het aantal opeenvolgende testfouten dat moet optreden voor een virtuele machine moet worden beschouwd als niet in orde  | 2 |

1. Klik op **OK** om in te stellen van de health-test.

### <a name="set-the-load-balancing-rules"></a>De load-balancingregels instellen

1. Klik op de load balancer, **Taakverdelingsregels**, en klik op **+ toevoegen**.

1. De load-balancingregels als volgt instellen.
   | Instelling | Beschrijving | Voorbeeld
   | --- | --- |---
   | **Naam** | Tekst | SQLAlwaysOnEndPointListener |
   | **Frontend IP-adres** | Kies een adres |Gebruik het adres dat u hebt gemaakt tijdens het maken van de load balancer. |
   | **Protocol** | Kies TCP |TCP |
   | **Poort** | Gebruik de poort voor SQL Server-exemplaar | 1433 |
   | **Back-Endpoort** | Dit veld wordt niet gebruikt wanneer zwevend IP is ingesteld voor direct server return | 1433 |
   | **Test** |De naam die u hebt opgegeven voor de test | SQLAlwaysOnEndPointProbe |
   | **Sessiepersistentie** | Vervolgkeuzelijst | **Geen** |
   | **Time-out voor inactiviteit** | Minuten geopend te houden die een TCP-verbinding | 4 |
   | **Zwevend IP (direct server return)** | |Ingeschakeld |

   > [!WARNING]
   > Direct server return is ingesteld tijdens het maken van. Deze kan niet worden gewijzigd.

1. Klik op **OK** om in te stellen van de load-balancingregels.

## <a name="configure-listener"></a>De listener configureren

De volgende wat te doen is voor het configureren van een beschikbaarheidsgroep-listener op het failovercluster.

> [!NOTE]
> Deze zelfstudie laat zien hoe u een één listener - maakt met een ILB IP-adres. Zie het maken van een of meer listeners met behulp van een of meer IP-adressen [Create Availability Group-listener en load balancer | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-listener-port"></a>Set-listener-poort

Stel de listener-poort in SQL Server Management Studio.

1. Start SQL Server Management Studio en maak verbinding met de primaire replica.

1. Navigeer naar **AlwaysOn hoge beschikbaarheid** | **beschikbaarheidsgroepen** | **beschikbaarheidsgroep-Listeners**.

1. U ziet nu de naam van de listener die u hebt gemaakt in Failoverclusterbeheer. Met de rechtermuisknop op de naam van de listener en klik op **eigenschappen**.

1. In de **poort** vak het poortnummer opgeven voor de beschikbaarheidsgroep-listener met behulp van de $EndpointPort u eerder hebt gebruikt (1433 is de standaardinstelling), klikt u vervolgens op **OK**.

U hebt nu een SQL Server-beschikbaarheidsgroep in Azure virtuele machines die worden uitgevoerd in de modus Resource Manager.

## <a name="test-connection-to-listener"></a>De testverbinding met listener

De verbinding testen:

1. RDP naar een SQL-Server die zich in hetzelfde virtuele netwerk, maar is geen eigenaar van de replica. U kunt de SQL-Server in het cluster.

1. Gebruik **sqlcmd** hulpprogramma om de verbinding te testen. Bijvoorbeeld, het volgende script stelt een **sqlcmd** verbinding met de primaire replica via de listener met de Windows-verificatie:

    ```
    sqlcmd -S <listenerName> -E
    ```

    Als de listener met behulp van een andere poort dan de standaard poort (1433), de poort in de verbindingsreeks opgeven. Bijvoorbeeld de volgende sqlcmd-opdracht is verbonden met een listener op poort 1435:

    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

De SQLCMD-verbinding wordt automatisch verbinding met elk ander exemplaar van SQL Server als host fungeert voor de primaire replica.

> [!TIP]
> Zorg ervoor dat de poort die u opgeeft geopend op de firewall van beide SQL-Servers is. Beide servers vereisen een inkomende regel voor de TCP-poort die u gebruikt. Zie voor meer informatie [toevoegen of bewerken firewallregel](http://technet.microsoft.com/library/cc753558.aspx).
>
>



<!--**Notes**: *Notes provide just-in-time info: A Note is “by the way” info, an Important is info users need to complete a task, Tip is for shortcuts. Don’t overdo*.-->


<!--**Procedures**: *This is the second “step." They often include substeps. Again, use a short title that tells users what they’ll do*. *("Configure a new web project.")*-->

<!--**UI**: *Note the format for documenting the UI: bold for UI elements and arrow keys for sequence. (Ex. Click **File > New > Project**.)*-->

<!--**Screenshot**: *Screenshots really help users. But don’t include too many since they’re difficult to maintain. Highlight areas you are referring to in red.*-->

<!--**No. of steps**: *Make sure the number of steps within a procedure is 10 or fewer. Seven steps is ideal. Break up long procedure logically.*-->


<!--**Next steps**: *Reiterate what users have done, and give them interesting and useful next steps so they want to go on.*-->

## <a name="next-steps"></a>Volgende stappen

- [Een IP-adres toevoegen aan een load balancer voor een tweede beschikbaarheidsgroep](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).
