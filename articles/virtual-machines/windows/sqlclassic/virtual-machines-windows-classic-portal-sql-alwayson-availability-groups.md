---
title: AlwaysOn-beschikbaarheidsgroep configureren in Azure Virtual Machines (klassiek) | Microsoft Docs
description: Maken van een AlwaysOn-beschikbaarheidsgroep met Azure Virtual Machines. Deze zelfstudie wordt voornamelijk gebruikt de gebruikersinterface en hulpprogramma's in plaats van het uitvoeren van scripts.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 8d48a3d2-79f8-4ab0-9327-2f30ee0b2ff1
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: b360fe9f28eeb9b10c82fce729165b1b572ac3c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-always-on-availability-group-in-azure-virtual-machines-classic"></a>AlwaysOn-beschikbaarheidsgroep configureren in Azure Virtual Machines (klassiek)
> [!div class="op_single_selector"]
> * [Klassieke: UI](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klassieke: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Voordat u begint, kunt u overwegen of u kunt deze taak in Azure Resource Manager-model voltooien. U wordt aangeraden Azure Resource Manager-model voor nieuwe implementaties. Zie [op virtuele machines in Azure SQL Server altijd op beschikbaarheidsgroepen](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT] 
> U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en classic](../../../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel wordt uitgelegd hoe u het klassieke implementatiemodel. 

Zie voor het voltooien van deze taak is met het Azure Resource Manager-model [op virtuele machines in Azure SQL Server altijd op beschikbaarheidsgroepen](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

Deze end-to-end zelfstudie ziet u hoe u beschikbaarheidsgroepen implementeert met behulp van SQL Server Always On uitgevoerd op Azure Virtual Machines.

Uw oplossing SQL Server Always On in Azure wordt aan het einde van de zelfstudie bestaat uit de volgende elementen:

* Een virtueel netwerk dat meerdere subnetten bevat en een front-end- en een back-end-subnet
* Een domeincontroller met een Active Directory (Azure AD)-domein
* Twee virtuele machines die SQL Server wordt uitgevoerd en zijn geïmplementeerd met het subnet voor back-end en toegevoegd aan het Azure AD-domein
* Een failovercluster met drie knooppunten met het model van de Quorumconfiguratie Knooppuntmeerderheid
* Een beschikbaarheidsgroep met twee synchrone doorvoer replica's van een beschikbaarheidsdatabase

De volgende afbeelding is een grafische weergave van de oplossing.

![Testlab-architectuur voor beschikbaarheidsgroepen in Azure](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Houd er rekening mee dat dit een mogelijke configuratie is. U kunt bijvoorbeeld het aantal virtuele machines voor een beschikbaarheidsgroep van twee replica minimaliseren. Deze configuratie wordt opgeslagen op rekenuren in Azure met behulp van de domeincontroller als de bestandssharewitness in quorum in een cluster met twee knooppunten. Deze methode wordt het aantal virtuele machines met één van de configuratie van de geïllustreerde verlaagd.

Deze zelfstudie wordt ervan uitgegaan:

* U hebt al een Azure-account.
* Weet u al het gebruik van de gebruikersinterface in de galerie met virtuele machine voor het inrichten van een klassieke virtuele machine waarop SQL Server wordt uitgevoerd.
* U hebt al een goed begrip van AlwaysOn-beschikbaarheidsgroepen. Zie voor meer informatie [altijd op beschikbaarheidsgroepen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Als u geïnteresseerd bent in het gebruik van AlwaysOn-beschikbaarheidsgroepen met SharePoint, zien ook de [configureren SQL Server 2012 Always On Availability Groups voor SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx).
> 
> 

## <a name="create-the-virtual-network-and-domain-controller-server"></a>De virtuele netwerk en domain controller-server maken
U begint met een nieuwe Azure proefaccount. Nadat u uw account hebt ingesteld, moet u op het startscherm van de klassieke Azure portal.

1. Klik op de **nieuw** knop op de linkerhoek van de onderkant van de pagina, zoals wordt weergegeven in de volgende schermafbeelding.
   
    ![Klik op Nieuw in de portal](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)
2. Klik op **netwerkservices** > **virtueel netwerk** > **aangepast maken**, zoals wordt weergegeven in de volgende schermafbeelding.
   
    ![Virtueel netwerk maken](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)
3. In de **VIRTUEEL netwerk maken** dialoogvenster vak, maakt u een nieuw virtueel netwerk door te doorlopen van de pagina's en de instellingen in de volgende tabel. 
   
   | Pagina | Instellingen |
   | --- | --- |
   | Details van virtueel netwerk |**NAAM ContosoNET =**<br/>**De regio VS-West =** |
   | DNS-Servers en VPN-verbinding |Geen |
   | Adresruimten voor virtueel netwerk |Instellingen worden weergegeven in de volgende schermafbeelding: ![Virtueel netwerk maken](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png) |
4. Maak de virtuele machine die u als de domeincontroller (DC gebruiken wilt). Klik op **nieuw** > **Compute** > **virtuele Machine** > **uit galerie**, zoals wordt weergegeven in de volgende schermafbeelding.
   
    ![Een virtuele machine maken](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)
5. In de **een virtuele MACHINE maken** dialoogvenster een nieuwe virtuele machine configureren door te doorlopen van de pagina's en de instellingen in de volgende tabel. 
   
   | Pagina | Instellingen |
   | --- | --- |
   | Het besturingssysteem van de virtuele machine selecteren |Windows Server 2012 R2 Datacenter |
   | Configuratie van virtuele machine |**VERSIE RELEASEDATUM** = (laatste)<br/>**NAAM van virtuele MACHINE** ContosoDC =<br/>**LAAG** = STANDARD<br/>**De grootte van** = A2 (2 kernen)<br/>**NIEUWE gebruikersnaam** AzureAdmin =<br/>**Nieuw wachtwoord** = Contoso! 000<br/>**Bevestig** = Contoso! 000 |
   | Configuratie van virtuele machine |**CLOUDSERVICE** = Maak een nieuwe cloudservice<br/>**DNS-naam van CLOUDSERVICE** = de naam van een unieke cloudservice<br/>**DNS-naam** een unieke naam = (ex: ContosoDC123)<br/>**REGIO/AFFINITEITSGROEP/VIRTUEEL netwerk** ContosoNET =<br/>**VIRTUEEL NETWERKSUBNETTEN** Back(10.10.2.0/24) =<br/>**STORAGE-ACCOUNT** = gebruik een automatisch gegenereerde storage-account<br/>**BESCHIKBAARHEIDSSET** = (geen) |
   | Opties voor de virtuele machine |Standaardinstellingen gebruiken |

Nadat u de nieuwe virtuele machine configureert, wacht u totdat de virtuele machine niet provsioned. Dit proces neemt enige tijd voltooien. Als u op de **virtuele Machine** tabblad in de klassieke Azure portal, ziet u ContosoDC waarbij statussen van **starten (inrichten)** naar **gestopt**, **starten**, **uitgevoerd (inrichten)**, en tot slot **met**.

De DC-server is nu is ingericht. Vervolgens configureert u het Active Directory-domein op deze DC-server.

## <a name="configure-the-domain-controller"></a>De domeincontroller configureren
In de volgende stappen configureert u de machine ContosoDC als een domeincontroller voor corp.contoso.com.

1. Selecteer in de portal de **ContosoDC** machine. Op de **Dashboard** tabblad **Connect** een extern bureaublad (RDP)-bestand voor toegang tot extern bureaublad te openen.
   
    ![Verbinding maken met Vritual Machine](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)
2. Meld u aan met uw geconfigureerde beheerdersaccount (**\AzureAdmin**) en het wachtwoord (**Contoso! 000**).
3. Standaard de **Serverbeheer** dashboard moet worden weergegeven.
4. Klik op de **functies en onderdelen toevoegen** koppeling in het dashboard.
   
    ![Server Explorer functies toevoegen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)
5. Klik op **volgende** totdat u de **serverfuncties** sectie.
6. Selecteer de **Active Directory Domain Services** en **DNS-Server** rollen. Wanneer u wordt gevraagd, meer onderdelen toevoegen die deze rollen is vereist.
   
   > [!NOTE]
   > U ontvangt een waarschuwing dat er geen statische IP-adres is validatie. Als u de configuratie testen wilt, klikt u op **doorgaan**. Voor productiescenario's [PowerShell gebruiken voor het instellen van het statische IP-adres van de domain controller machine](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   > 
   > 
   
    ![Dialoogvenster rollen toevoegen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)
7. Klik op **volgende** totdat u de **bevestiging** sectie. Selecteer de **de doelserver automatisch opnieuw opstarten indien nodig** selectievakje.
8. Klik op **Install**.
9. Nadat de onderdelen zijn geïnstalleerd, keert u terug naar de **Serverbeheer** dashboard.
10. Selecteer de nieuwe **AD DS** optie in het linkerdeelvenster.
11. Klik op de **meer** koppeling op de gele waarschuwing-balk.
    
     ![Dialoogvenster van AD DS op virtuele machine van DNS-server](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)
12. In de **actie** kolom van de **alle Server-taakdetails** in het dialoogvenster, klikt u op **deze server verhogen naar een domeincontroller**.
13. In de **Wizard Active Directory Domain Services configureren**, gebruik de volgende waarden:
    
    | Pagina | Instelling |
    | --- | --- |
    | Implementatieconfiguratie |**Een nieuw forest toevoegen** geselecteerde =<br/>**Naam van hoofddomein** corp.contoso.com = |
    | Opties voor domeincontroller |**Wachtwoord** = Contoso! 000<br/>**Bevestig het wachtwoord** = Contoso! 000 |
14. Klik op **volgende** te doorlopen van de andere pagina's in de wizard. Op de **controle van vereisten** pagina, controleert u of u het volgende foutbericht weergegeven: **alle controles op vereisten zijn geslaagd**. Houd er rekening mee dat u wordt aangeraden alle toepasselijke waarschuwingsberichten, maar het is mogelijk om door te gaan met de installatie.
15. Klik op **Install**. De **ContosoDC** virtuele machine wordt automatisch opnieuw opgestart.

## <a name="configure-domain-accounts"></a>Domeinaccounts configureren
De volgende stappen configureert u de Active Directory-accounts voor later gebruik.

1. Meld u weer aan bij de **ContosoDC** machine.
2. In **Serverbeheer**, klikt u op **extra** > **Active Directory Administrative Center**.
   
    ![Active Directory-beheercentrum](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)
3. In de **Active Directory Administrative Center**, selecteer **corp (lokaal)** in het linkerdeelvenster.
4. Aan de rechterkant **taken** deelvenster, klikt u op **nieuw** > **gebruiker**. Gebruik de volgende instellingen:
   
   | Instelling | Waarde |
   | --- | --- |
   | **Voornaam** |Installeren |
   | **SamAccountName van gebruiker** |Installeren |
   | **Wachtwoord** |Contoso! 000 |
   | **Wachtwoord bevestigen** |Contoso! 000 |
   | **Andere opties voor wachtwoord** |Geselecteerd |
   | **Wachtwoord verloopt nooit** |Geselecteerd |
5. Klik op **OK** maken de **installeren** gebruiker. Dit account wordt gebruikt om het failovercluster en de beschikbaarheidsgroep te configureren.
6. Maak twee extra gebruikers, **CORP\SQLSvc1** en **CORP\SQLSvc2**, met dezelfde stappen. Deze accounts wordt gebruikt voor de SQL Server-exemplaren. U moet vervolgens geven **CORP\Install** de benodigde machtigingen voor het configureren van Windows Failoverclustering.
7. In de **Active Directory Administrative Center**, klikt u op **corp (lokaal)** in het linkerdeelvenster. In de **taken** deelvenster, klikt u op **eigenschappen**.
   
    ![Gebruikerseigenschappen CORP](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)
8. Selecteer **extensies**, en klik vervolgens op de **Geavanceerd** knop op de **beveiliging** tabblad.
9. In de **geavanceerde beveiligingsinstellingen voor de corp** in het dialoogvenster, klikt u op **toevoegen**.
10. Klik op **een principal selecteren**, zoeken naar **CORP\Install**, en klik vervolgens op **OK**.
11. Selecteer de **alle eigenschappen lezen** en **Computerobjecten maken** machtigingen.
    
     ![Gebruikersmachtigingen Corp](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)
12. Klik op **OK**, en klik vervolgens op **OK** opnieuw. Sluit het venster corp-eigenschappen.

Nu dat u Active Directory en de gebruikersobjecten hebt geconfigureerd, wordt u drie virtuele machines van de SQL Server maken en voeg ze toe aan dit domein.

## <a name="create-the-sql-server-virtual-machines"></a>De virtuele machines van SQL Server maken
Maak drie virtuele machines. Een voor een clusterknooppunt is en twee zijn voor SQL Server. Voor het maken van elk van de virtuele machines, gaat u terug naar de klassieke Azure portal, klikt u op **nieuw** > **Compute** > **virtuele Machine** > **uit galerie**. Vervolgens gebruikt u de sjablonen in de volgende tabel om u te helpen u bij het maken van de virtuele machines.

| Pagina | VM1 | VM2 | VM3 |
| --- | --- | --- | --- |
| Het besturingssysteem van de virtuele machine selecteren |**Windows Server 2012 R2 Datacenter** |**SQL Server 2014 RTM Enterprise** |**SQL Server 2014 RTM Enterprise** |
| Configuratie van virtuele machine |**VERSIE RELEASEDATUM** = (laatste)<br/>**NAAM van virtuele MACHINE** ContosoWSFCNode =<br/>**LAAG** = STANDARD<br/>**De grootte van** = A2 (2 kernen)<br/>**NIEUWE gebruikersnaam** AzureAdmin =<br/>**Nieuw wachtwoord** = Contoso! 000<br/>**Bevestig** = Contoso! 000 |**VERSIE RELEASEDATUM** = (laatste)<br/>**NAAM van virtuele MACHINE** ContosoSQL1 =<br/>**LAAG** = STANDARD<br/>**De grootte van** = A3 (4 kernen)<br/>**NIEUWE gebruikersnaam** AzureAdmin =<br/>**Nieuw wachtwoord** = Contoso! 000<br/>**Bevestig** = Contoso! 000 |**VERSIE RELEASEDATUM** = (laatste)<br/>**NAAM van virtuele MACHINE** ContosoSQL2 =<br/>**LAAG** = STANDARD<br/>**De grootte van** = A3 (4 kernen)<br/>**NIEUWE gebruikersnaam** AzureAdmin =<br/>**Nieuw wachtwoord** = Contoso! 000<br/>**Bevestig** = Contoso! 000 |
| Configuratie van virtuele machine |**CLOUDSERVICE** = eerder gemaakte unieke Cloud Service DNS-naam (ex: ContosoDC123)<br/>**REGIO/AFFINITEITSGROEP/VIRTUEEL netwerk** ContosoNET =<br/>**VIRTUEEL NETWERKSUBNETTEN** Back(10.10.2.0/24) =<br/>**STORAGE-ACCOUNT** = gebruik een automatisch gegenereerde storage-account<br/>**BESCHIKBAARHEIDSSET** = Maak een beschikbaarheidsset instellen<br/>**NAAM VAN DE BESCHIKBAARHEIDSSET** SQLHADR = |**CLOUDSERVICE** = eerder gemaakte unieke Cloud Service DNS-naam (ex: ContosoDC123)<br/>**REGIO/AFFINITEITSGROEP/VIRTUEEL netwerk** ContosoNET =<br/>**VIRTUEEL NETWERKSUBNETTEN** Back(10.10.2.0/24) =<br/>**STORAGE-ACCOUNT** = gebruik een automatisch gegenereerde storage-account<br/>**BESCHIKBAARHEIDSSET** = SQLHADR (u kunt ook de beschikbaarheidsset nadat de machine is gemaakt. Alle drie de machines moeten worden toegewezen aan de beschikbaarheidsset SQLHADR.) |**CLOUDSERVICE** = eerder gemaakte unieke Cloud Service DNS-naam (ex: ContosoDC123)<br/>**REGIO/AFFINITEITSGROEP/VIRTUEEL netwerk** ContosoNET =<br/>**VIRTUEEL NETWERKSUBNETTEN** Back(10.10.2.0/24) =<br/>**STORAGE-ACCOUNT** = gebruik een automatisch gegenereerde storage-account<br/>**BESCHIKBAARHEIDSSET** = SQLHADR (u kunt ook de beschikbaarheidsset nadat de machine is gemaakt. Alle drie de machines moeten worden toegewezen aan de beschikbaarheidsset SQLHADR.) |
| Opties voor de virtuele machine |Standaardinstellingen gebruiken |Standaardinstellingen gebruiken |Standaardinstellingen gebruiken |

<br/>

> [!NOTE]
> De vorige configuratie stelt standaardcategorie virtuele machines, omdat BASIC laagmachines eindpunten taakverdeling niet ondersteunen. U moet Netwerktaakverdeling eindpunten later naar een beschikbaarheidsgroep-listener te maken. De machine-grootten die hier worden voorgesteld zijn ook bedoeld voor het testen van beschikbaarheidsgroepen in Azure Virtual Machines. Zie de aanbevelingen voor de grootte van de SQL Server-machines en configuratie-instellingen voor de beste prestaties op productieworkloads [best practices prestaties for SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-performance.md).
> 
> 

Nadat de drie virtuele machines zijn volledig is ingericht, moet u kan toevoegen aan de **corp.contoso.com** domein en verleen CORP\Install beheerrechten op de virtuele machines. Gebruik hiervoor de volgende stappen uit voor elk van de drie virtuele machines.

1. Hiermee wijzigt u eerst, het voorkeurs-DNS-serveradres. Elke virtuele machine RDP-bestand downloaden naar uw lokale directory door de virtuele machine in de lijst selecteren en klikken op de **Connect** knop. Om te selecteren van een virtuele machine, klik op een willekeurige plaats maar de eerste cel in de rij zoals weergegeven in de volgende schermafbeelding.
   
    ![Download het RDP-bestand](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)
2. Open het RDP-bestand dat u hebt gedownload en meld u aan met de virtuele machine met behulp van uw geconfigureerde beheerdersaccount (**BUILTIN\AzureAdmin**) en het wachtwoord (**Contoso! 000**).
3. Nadat u zich aanmeldt, ziet u de **Serverbeheer** dashboard. Klik op **lokale Server** in het linkerdeelvenster.
4. Klik op de **IPv4-adres toegewezen door DHCP, IPv6 ingeschakeld** koppeling.
5. In de **netwerkverbindingen** dialoogvenster vak, klikt u op het netwerkpictogram.
   
    ![De virtuele machine voorkeurs-DNS-server wijzigen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)
6. Klik op de opdrachtbalk op **Wijzig de instellingen van deze verbinding**. (Afhankelijk van de grootte van het venster kunt u wellicht te klikken op de dubbele pijl naar rechts om te zien met deze opdracht).
7. Selecteer **Internet Protocol versie 4 (TCP/IPv4)**, en klik vervolgens op **eigenschappen**.
8. Selecteer **de volgende DNS-serveradressen** en geef vervolgens **10.10.2.4** in **voorkeurs-DNS-server**.
9. De **10.10.2.4** adres is het adres dat toegewezen aan een virtuele machine in het subnet 10.10.2.0/24 in een Azure-netwerk. Dat de virtuele machine is **ContosoDC**. Om te controleren of **ContosoDC**van IP-adres, gebruik **nslookup contosodc** in het opdrachtpromptvenster, zoals wordt weergegeven in de volgende schermafbeelding.
   
    ![Gebruik NSLOOKUP IP-adres voor de domeincontroller vinden](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)
10. Klik op **OK** > **sluiten** de wijzigingen doorvoeren. U kunt nu deelnemen aan de virtuele machine **corp.contoso.com**.
11. Terug in de **lokale Server** venster, klikt u op de **werkgroep** koppeling.
12. In de **computernaam** sectie, klikt u op **wijziging**.
13. Selecteer de **domein** selectievakje, type **corp.contoso.com** in het tekstvak en klik vervolgens op **OK**.
14. In de **Windows-beveiliging** dialoogvenster geeft u de referenties voor de administrator-account van het standaard (**CORP\AzureAdmin**) en het wachtwoord (**Contoso! 000**).
15. Wanneer u het bericht "Welkom bij het domein corp.contoso.com" ziet, klikt u op **OK**.
16. Klik op **sluiten** > **nu opnieuw opstarten** in het dialoogvenster.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-virtual-machine"></a>De gebruiker Corp\Install toevoegen als beheerder op elke virtuele machine
1. Wacht tot de virtuele machine opnieuw is opgestart, en open vervolgens het RDP-bestand opnieuw om aan te melden bij de virtuele machine met behulp van de **BUILTIN\AzureAdmin** account.
2. In **Serverbeheer** klikt u op **extra** > **Computerbeheer**.
   
    ![Computerbeheer](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)
3. In de **Computerbeheer** dialoogvenster Vouw **lokale gebruikers en groepen**, en klik vervolgens op **groepen**.
4. Dubbelklik op de **beheerders** groep.
5. In de **beheerders eigenschappen** in het dialoogvenster, klikt u op de **toevoegen** knop.
6. Voer de gebruiker **CORP\Install**, en klik vervolgens op **OK**. Wanneer u wordt gevraagd om referenties, gebruiken de **AzureAdmin** rekening met de **Contoso! 000** wachtwoord.
7. Klik op **OK** sluiten de **beheerder eigenschappen** in het dialoogvenster.

### <a name="add-the-failover-clustering-feature-to-each-virtual-machine"></a>Voeg het onderdeel Failover Clustering toe aan elke virtuele machine
1. In de **Serverbeheer** dashboard, klikt u op **functies en onderdelen toevoegen**.
2. In de **Wizard Functies toevoegen en onderdelen**, klikt u op **volgende** totdat u de **functies** pagina.
3. Selecteer **Failoverclustering**. Wanneer u wordt gevraagd, andere afhankelijke onderdelen toevoegen.
   
    ![Functie Failover Clustering toevoegen aan virtuele machine](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)
4. Klik op **volgende**, en klik vervolgens op **installeren** op de **bevestiging** pagina.
5. Wanneer de **Failoverclustering** functie-installatie is voltooid, klikt u op **sluiten**.
6. Afmelden bij de virtuele machine.
7. Herhaal de stappen in deze sectie voor alle drie de servers: **ContosoWSFCNode**, **ContosoSQL1**, en **ContosoSQL2**.

De virtuele machines zijn nu ingericht SQL-Server en wordt uitgevoerd, maar elk heeft de standaardopties voor SQL Server.

## <a name="create-the-failover-cluster"></a>De failover-cluster maken
In deze sectie maakt u het failover-cluster die als host fungeert voor de beschikbaarheidsgroep die u later gaat maken. Nu moet u het volgende aan elk van de drie virtuele machines die u in het failovercluster gebruiken wilt hebt gedaan:

* De virtuele machine in Azure volledig is ingericht
* De virtuele machine toegevoegd aan het domein
* Toegevoegd **CORP\Install** aan de lokale groep Administrators
* Het onderdeel failover clustering toegevoegd

Al deze zijn vereisten op elke virtuele machine voordat u kunt het deelnemen aan het failovercluster.

Houd er ook rekening mee dat de virtuele Azure-netwerk niet meer op dezelfde manier als een on-premises netwerk werkt. U moet maken van het cluster in de volgende volgorde:

1. Maken van een cluster met één knooppunt op één knooppunt (**ContosoSQL1**).
2. Wijzigen van de IP-adres van het cluster naar een niet-gebruikte IP-adres (**10.10.2.101**).
3. De naam van het cluster online brengen.
4. Toevoegen van de andere knooppunten (**ContosoSQL2** en **ContosoWSFCNode**).

Gebruik de volgende stappen voor het voltooien van de taken die het cluster volledig te configureren.

1. Open het RDP-bestand voor **ContosoSQL1**, en meld u aan met behulp van het domeinaccount **CORP\Install**.
2. In de **Serverbeheer** dashboard, klikt u op **extra** > **Failoverclusterbeheer**.
3. In het linkerdeelvenster met de rechtermuisknop op **Failoverclusterbeheer**, en klik vervolgens op **maken van een Cluster**, zoals wordt weergegeven in de volgende schermafbeelding.
   
    ![Cluster maken](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)
4. Maak een cluster met één knooppunt door de pagina's doorlopen en het gebruik van de instellingen in de volgende tabel in de Wizard Cluster maken:
   
   | Pagina | Instellingen |
   | --- | --- |
   | Voordat u begint |Standaardinstellingen gebruiken |
   | Servers selecteren |Type **ContosoSQL1** in **Enter servernaam** en klik op **toevoegen** |
   | Van validatiewaarschuwing |Selecteer **nr. ik geen ondersteuning van Microsoft nodig hebt voor dit cluster, en daarom niet wilt de validatietests uit te voeren. Als ik op Volgende klikt, doorgaan met het maken van het cluster**. |
   | Toegangspunt voor beheer van het Cluster |Type **Cluster1** in **clusternaam** |
   | Bevestiging |Gebruik de standaardwaarden tenzij u van opslagruimten gebruikmaakt. Zie de waarschuwing onder deze tabel. |
   
   > [!WARNING]
   > Als u [opslagruimten](https://technet.microsoft.com/library/hh831739), die meerdere schijven in opslaggroepen hebt gegroepeerd, moet u het selectievakje de **alle in aanmerking komende opslag toevoegen aan het cluster** selectievakje op het **bevestiging** pagina. Als u deze optie niet uitschakelt, wordt de virtuele schijven worden losgekoppeld tijdens het clusterproces. Als gevolg hiervan weergegeven deze ook niet in Schijfbeheer of Verkenner totdat de opslagruimten zijn verwijderd uit het cluster en opnieuw worden gekoppeld met behulp van PowerShell.
   > 
   > 
5. Vouw in het linkerdeelvenster **Failoverclusterbeheer**, en klik vervolgens op **Cluster1.corp.contoso.com**.
6. In het middelste deelvenster, schuif omlaag naar de **Clusterkernresources** uit en vouw de **naam: Clutser1** details. Ziet u zowel de **naam** en de **IP-adres** bronnen in de **mislukt** status. De bron van het IP-adres kan niet online worden gebracht, omdat het cluster is toegewezen van hetzelfde IP-adres als de machine zelf, dit is een dubbel adres.
7. Met de rechtermuisknop op de mislukte **IP-adres** bron en klik vervolgens op **eigenschappen**.
   
    ![Eigenschappen van cluster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)
8. Selecteer **statisch IP-adres**, geef **10.10.2.101** in de **adres** in het tekstvak en klik vervolgens op **OK**.
9. In de **Clusterkernresources** sectie, met de rechtermuisknop op **naam: Cluster1**, en klik vervolgens op **Online brengen**. Wacht totdat beide bronnen online zijn. Wanneer de clusterbron met de naam van online wordt gezet, wordt de DC-server wordt bijgewerkt met een nieuw Active Directory-computeraccount. Deze Active Directory-account wordt gebruikt voor het uitvoeren van de beschikbaarheid van groep geclusterde service later.
10. De resterende knooppunten toevoegen aan het cluster. Met de rechtermuisknop in de structuur van de browser **Cluster1.corp.contoso.com**, en klik vervolgens op **knooppunt toevoegen**, zoals wordt weergegeven in de volgende schermafbeelding.
    
     ![Knooppunt toevoegen aan het cluster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)
11. In de **Wizard knooppunt toevoegen**, klikt u op **volgende** op de **Servers selecteren** pagina, het toevoegen van **ContosoSQL2** en **ContosoWSFCNode** aan de lijst door de naam van de server in te typen **Enter servernaam** en vervolgens te klikken op **toevoegen**. Wanneer u klaar bent, klikt u op **volgende**.
12. Op de **Validation Warning** pagina, klikt u op **Nee**, maar in een productiescenario, moet u de validatietests uitvoeren. Klik op **Volgende**.
13. Op de **bevestiging** pagina, klikt u op **volgende** de knooppunten toevoegen.
    
    > [!WARNING]
    > Als u [opslagruimten](https://technet.microsoft.com/library/hh831739), die meerdere schijven in opslaggroepen hebt gegroepeerd, moet u het selectievakje de **alle in aanmerking komende opslag toevoegen aan het cluster** selectievakje. Als u deze optie niet uitschakelt, wordt de virtuele schijven worden losgekoppeld tijdens het clusterproces. Als gevolg hiervan worden ook niet weergegeven in Schijfbeheer of Explorer totdat de opslagruimten van cluster zijn verwijderd en opnieuw gekoppeld met behulp van PowerShell.
    > 
    > 
14. Nadat de knooppunten aan het cluster worden toegevoegd, klikt u op **voltooien**. Failover-clusterbeheer worden nu weergegeven dat het cluster heeft drie knooppunten en deze in de **knooppunten** container.
15. Afmelden bij de extern bureaublad-sessiehost.

## <a name="prepare-the-sql-server-instances-for-availability-groups"></a>De SQL Server-exemplaren voor beschikbaarheidsgroepen voorbereiden
In deze sectie doet u het volgende uit op beide **ContosoSQL1** en **contosoSQL2**:

* Toevoegen van een aanmelding voor **NT AUTHORITY\System** met de benodigde machtigingen zijn ingesteld op het standaardexemplaar van SQL Server.
* Voeg **CORP\Install** als een rol sysadmin op het standaardexemplaar van SQL Server.
* Open de firewall voor externe toegang van SQL Server.
* De Always On availability groepsfunctie inschakelen.
* De SQL Server-serviceaccount wijzigen **CORP\SQLSvc1** en **CORP\SQLSvc2**respectievelijk.

Deze acties kunnen worden uitgevoerd in een willekeurige volgorde. Evenwel doorlopen de volgende stappen die deze in volgorde. Volg de stappen voor beide **ContosoSQL1** en **ContosoSQL2**:

1. Als u bent niet afgemeld bij de extern bureaublad-sessiehost voor de virtuele machine, moet u dit nu doen.
2. Open de RDP-bestanden voor **ContosoSQL1** en **ContosoSQL2**, en meld u aan als **BUILTIN\AzureAdmin**.
3. Voeg **NT AUTHORITY\System** aan de SQL Server-aanmeldingen met vereiste machtigingen. Open **SQL Server Management Studio**.
4. Klik op **Connect** verbinding maken met het standaardexemplaar van SQL Server.
5. In **Objectverkenner**, vouw **beveiliging**, en vouw vervolgens **aanmeldingen**.
6. Met de rechtermuisknop op de **NT AUTHORITY\System** aanmelding en klik vervolgens op **eigenschappen**.
7. Op de **Securables** pagina voor de lokale server, selecteer **Grant** voor de volgende machtigingen en klik vervolgens op **OK**.
   
   * Wijzigen van een beschikbaarheidsgroep
   * Verbinding maken met SQL
   * Status van de server weergeven
8. Voeg **CORP\Install** als een **sysadmin** het standaardexemplaar van SQL Server-rol. In **Objectverkenner**, met de rechtermuisknop op **aanmeldingen**, en klik vervolgens op **New Login**.
9. Type **CORP\Install** in **aanmeldingsnaam**.
10. Op de **serverfuncties** pagina **sysadmin**, en klik vervolgens op **OK**. Nadat u de aanmelding hebt gemaakt, kunt u deze bekijken door het uitbreiden van **aanmeldingen** in **Objectverkenner**.
11. Een firewallregel maken voor SQL Server op de **Start** scherm open **Windows Firewall met geavanceerde beveiliging**.
12. Selecteer in het linkerdeelvenster **regels voor binnenkomende verbindingen**. Klik in het rechterdeelvenster **nieuwe regel**.
13. Op de **regeltype** pagina, klikt u op **programma** > **volgende**.
14. Op de **programma** pagina **dit programmapad**, type **%ProgramFiles%\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** in het tekstvak en klik vervolgens op **volgende**. Als u deze aanwijzingen maar met behulp van SQL Server 2012, SQL Server-map is **MSSQL11. MSSQLSERVER**.
15. Op de **actie** pagina, houden **de verbinding toestaan** geselecteerd en klik vervolgens op **volgende**.
16. Op de **profiel** pagina, accepteer de standaardinstellingen en klik vervolgens op **volgende**.
17. Op de **naam** pagina, geef de regelnaam van een, zoals **SQL Server (programmaregel)**, in de **naam** tekst vak en klik vervolgens op **voltooien**.
18. Om in te schakelen de **altijd op beschikbaarheidsgroepen** functie op de **Start** scherm open **SQL Server Configuration Manager**.
19. Klik in de structuur van de browser op **SQL Server-Services**, met de rechtermuisknop op de **SQL Server (MSSQLSERVER)** service en klik vervolgens op **eigenschappen**.
20. Klik op de **altijd op hoge beschikbaarheid** tabblad **inschakelen altijd op beschikbaarheidsgroepen**, zoals weergegeven in de volgende schermafbeelding en klik vervolgens op **toepassen**. Klik op **OK** in het dialoogvenster en voert u niet sluit de **eigenschappen** nog in het dialoogvenster. U kunt de SQL Server-service wordt opnieuw opgestart na het wijzigen van het serviceaccount.
    
     ![Schakel altijd op beschikbaarheidsgroepen](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)
21. De SQL Server-serviceaccount, klikt u op de **aanmelden** tabblad, typt u **CORP\SQLSvc1** (voor **ContosoSQL1**) of **CORP\SQLSvc2** (voor **ContosoSQL2**) in **accountnaam**, vul en Bevestig het wachtwoord en klik vervolgens op **OK**.
22. Klik in het dialoogvenster op **Ja** de SQL Server-service te starten. Nadat de SQL Server-service opnieuw is opgestart, wijzigingen die u hebt aangebracht in de **eigenschappen** in het dialoogvenster geldig zijn.
23. Afmelden bij de virtuele machines.

## <a name="create-the-availability-group"></a>Maken van de beschikbaarheidsgroep.
U bent nu klaar om te configureren van een beschikbaarheidsgroep. Hieronder vindt u een overzicht van wat u doet:

* Een nieuwe database maken (**MyDB1**) op **ContosoSQL1**.
* Neem een volledige back-up en de een transactielogboek van de database.
* Herstellen van de volledige en logboekback-ups naar **ContosoSQL2** met de **NORECOVERY** optie.
* Maken van de beschikbaarheidsgroep (**AG1**) met synchrone doorvoer en automatische failover leesbare secundaire replica's.

### <a name="create-the-mydb1-database-on-contososql1"></a>De database MyDB1 op ContosoSQL1 maken
1. Als u bent niet al afgemeld bij de extern bureaublad-sessies voor **ContosoSQL1** en **ContosoSQL2**, doet u dat nu.
2. Open het RDP-bestand voor **ContosoSQL1**, en meld u aan als **CORP\Install**.
3. In **Verkenner**onder **C:\\**, maak een map met de naam **back-**. U gebruikt deze map voor back-up en herstel van uw database.
4. Met de rechtermuisknop op de nieuwe map, wijs **delen met**, en klik vervolgens op **specifieke personen**, zoals wordt weergegeven in de volgende schermafbeelding.
   
    ![Maak een back-map](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)
5. Voeg **CORP\SQLSvc1**, en wijs hieraan de **lezen/schrijven** machtiging. Voeg **CORP\SQLSvc2**, en wijs hieraan de **lezen** toestemming hebben, zoals weergegeven in de volgende schermafbeelding en klik vervolgens op **Share**. Nadat het proces voor het delen van bestanden is voltooid, klikt u op **gedaan**.
   
    ![Machtigingen voor back-upmap](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)
6. De database maken op basis van de **Start** menu openen **SQL Server Management Studio**, en klik vervolgens op **Connect** verbinding maken met het standaardexemplaar van SQL Server.
7. In **Objectverkenner**, met de rechtermuisknop op **Databases**, en klik vervolgens op **nieuwe Database**.
8. In **databasenaam**, type **MyDB1**, en klik vervolgens op **OK**.

### <a name="make-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Maak een volledige back-up van MyDB1 en terugzetten op ContosoSQL2
1. Om een volledige back-up van de database in **Objectverkenner**, vouw **Databases**, met de rechtermuisknop op **MyDB1**, wijs **taken**, en klik vervolgens op **Back-Up**.
2. In de **bron** sectie, houden **back-uptype** ingesteld op **volledige**. In de **bestemming** sectie, klikt u op **verwijderen** het standaardpad voor de back-upbestand verwijderen.
3. In de **bestemming** sectie, klikt u op **toevoegen**.
4. In de **bestandsnaam** in het tekstvak  **\\ContosoSQL1\backup\MyDB1.bak**, klikt u op **OK**, en klik vervolgens op **OK** opnieuw naar de back-up van de database. Wanneer de back-upbewerking is voltooid, klikt u op **OK** opnieuw naar het dialoogvenster te sluiten.
5. Om een transactielogboek van de database in **Objectverkenner**, vouw **Databases**, met de rechtermuisknop op **MyDB1**, wijs **taken**, en klik vervolgens op **Back-Up**.
6. In **back-uptype**, selecteer **transactielogboek**. Houd de **bestemming** pad is ingesteld op de domeincontroller die u eerder opgegeven bestand en klik vervolgens op **OK**. Nadat de back-upbewerking is voltooid, klikt u op **OK** opnieuw.
7. Herstellen de volledige installatieoptie en transactie logboekback-ups op **ContosoSQL2**, open het RDP-bestand voor **ContosoSQL2**, en meld u aan als **CORP\Install**. Laat de extern bureaublad-sessiehost voor **ContosoSQL1** openen.
8. Van de **Start** menu openen **SQL Server Management Studio**, en klik vervolgens op **Connect** verbinding maken met het standaardexemplaar van SQL Server.
9. In **Objectverkenner**, met de rechtermuisknop op **Databases**, en klik vervolgens op **Restore Database**.
10. In de **bron** sectie **apparaat**, en klik op het weglatingsteken **...** te klikken.
11. In **Selecteer back-upapparaten**, klikt u op **toevoegen**.
12. In **back-up bestandslocatie**, type  **\\ContosoSQL1\backup**, klikt u op **vernieuwen**, selecteer **MyDB1.bak**, klikt u op **OK**, en klik vervolgens op **OK** opnieuw. U ziet nu de volledige back-up en de logboekback-up in de **back-upsets herstellen** deelvenster.
13. Ga naar de **opties** pagina **RESTORE WITH NORECOVERY** in **herstelstatus**, en klik vervolgens op **OK** om de database te herstellen. Nadat de herstelbewerking is voltooid, klikt u op **OK**.

### <a name="create-the-availability-group"></a>Maken van de beschikbaarheidsgroep.
1. Ga terug naar de extern bureaublad-sessiehost voor **ContosoSQL1**. In **Objectverkenner** in SQL Server Management Studio, met de rechtermuisknop op **altijd op hoge beschikbaarheid**, en klik vervolgens op **nieuwe Wizard beschikbaarheidsgroep**, zoals wordt weergegeven in de volgende schermafbeelding.
   
    ![Start de Wizard Nieuwe beschikbaarheidsgroep](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)
2. Op de **inleiding** pagina, klikt u op **volgende**. Op de **naam van de beschikbaarheidsgroep opgeven** typt **AG1** in **naam van de beschikbaarheidsgroep**, klikt u vervolgens op **volgende** opnieuw.
   
    ![Wizard Nieuwe beschikbaarheidsgroep, geef de naam van beschikbaarheidsgroep](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)
3. Op de **Databases selecteren** pagina **MyDB1**, en klik vervolgens op **volgende**. De database voldoet aan de vereisten voor een beschikbaarheidsgroep, omdat u ten minste één volledige back-up hebt uitgevoerd op de beoogde primaire replica.
   
    ![Wizard Nieuwe Availabilty groep, selecteer databases](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)
4. op de **replica's opgeven** pagina, klikt u op **Replica toevoegen**.
   
    ![Wizard Nieuwe Availabilty groep, replica's opgeven](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)
5. In de **verbinding maken met Server** in het dialoogvenster, type **ContosoSQL2** in **servernaam**, en klik vervolgens op **Connect**.
   
    ![Nieuwe Availabilty groep Wizard verbinding maken met server](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)
6. Terug op de **replica's opgeven** pagina u ziet nu **ContosoSQL2** die worden vermeld in **beschikbare replica's**. De replica's configureren zoals wordt weergegeven in de volgende schermafbeelding. Wanneer u klaar bent, klikt u op **volgende**.
   
    ![Wizard Nieuwe Availabilty groep, geef replica's (voltooien)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)
7. Op de **eerste gegevenssynchronisatie selecteren** pagina **alleen deelnemen**, en klik vervolgens op **volgende**. U al gegevenssynchronisatie handmatig uitgevoerd wanneer u de volledige en transactie back-ups gemaakt op **ContosoSQL1** en deze teruggezet op **ContosoSQL2**. U kunt geen back-up en herstellen van bewerkingen voor uw database en selecteert u in plaats daarvan **volledige** zodat de nieuwe Wizard beschikbaarheidsgroep gegevenssynchronisatie voor u uitvoeren. Echter raadzaam niet deze optie voor zeer grote databases die zijn gevonden in sommige bedrijven.
   
    ![De Wizard Nieuwe Availabilty groep, eerste gegevenssynchronisatie selecteren](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)
8. Op de **validatie** pagina, klikt u op **volgende**. Deze pagina zijn vergelijkbaar met de volgende schermafbeelding. Er is een waarschuwing voor de listenerconfiguratie van de omdat u een beschikbaarheidsgroep-listener niet hebt geconfigureerd. Omdat een listener niet is geconfigureerd met deze zelfstudie, kunt u deze waarschuwing negeren. Zie de listener configureren na het voltooien van deze zelfstudie [een ILB-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure](../classic/ps-sql-int-listener.md).
   
    ![Availabilty Wizard nieuwe validatie](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)
9. Op de **samenvatting** pagina, klikt u op **voltooien**, en wacht tot de wizard configureert u de nieuwe beschikbaarheidsgroep. Op de **voortgang** pagina, klikt u op **meer details** om de voortgang van de gedetailleerde weer te geven. Nadat de wizard is voltooid, controleert de **resultaten** pagina om te controleren of de beschikbaarheidsgroep is gemaakt, zoals wordt weergegeven in de volgende schermafbeelding en klik vervolgens op **sluiten** de wizard wilt afsluiten.
   
    ![Wizard Nieuwe groep Availabilty, resultaten](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)
10. In **Objectverkenner**, vouw **altijd op hoge beschikbaarheid**, en vouw vervolgens **beschikbaarheidsgroepen**. U ziet nu de nieuwe beschikbaarheidsgroep in deze container. Met de rechtermuisknop op **AG1 (primaire)**, en klik vervolgens op **Dashboard weergeven**.
    
     ![Dashboard van de beschikbaarheidsgroep weergeven](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)
11. Uw **altijd op Dashboard** zijn vergelijkbaar met de structuur in de volgende schermafbeelding. U ziet de replica's, de failover-modus van elke replica en de synchronisatiestatus.
    
     ![Beschikbaarheidsgroep-Dashboard](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)
12. Ga terug naar **Serverbeheer**, klikt u op **extra**, en open vervolgens **Failoverclusterbeheer**.
13. Vouw **Cluster1.corp.contoso.com**, en vouw vervolgens **Services en toepassingen**. Selecteer **rollen** en merk op dat de **AG1** rol beschikbaarheid-groep is gemaakt. Houd er rekening mee dat AG1 geen IP-adres door welke database clients verbinding met de beschikbaarheidsgroep maken kunnen, omdat u niet een listener hebt geconfigureerd. U kunt rechtstreeks verbinding maken met het primaire knooppunt voor lees-/ schrijfbewerkingen en het secundaire knooppunt voor query's voor alleen-lezen.
    
     ![AG in Failoverclusterbeheer](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

> [!WARNING]
> Probeer geen failover van de beschikbaarheidsgroep vanuit Failoverclusterbeheer. Alle failover-bewerkingen moeten worden uitgevoerd vanuit **altijd op Dashboard** in SQL Server Management Studio. Zie voor meer informatie [beperkingen op met behulp van de Failover-clusterbeheer met beschikbaarheidsgroepen](https://msdn.microsoft.com/library/ff929171.aspx).
> 
> 

## <a name="next-steps"></a>Volgende stappen
U hebt nu geïmplementeerd SQL Server Always On door het maken van een beschikbaarheidsgroep in Azure. Zie voor het configureren van een listener voor deze beschikbaarheidsgroep [een ILB-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure](../classic/ps-sql-int-listener.md).

Zie voor meer informatie over het gebruik van SQL Server in Azure, [SQL Server op Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

