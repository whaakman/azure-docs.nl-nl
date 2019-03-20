---
title: SQL Server-beschikbaarheidsgroepen - virtuele machines van Azure - vereisten | Microsoft Docs
description: In deze zelfstudie laat zien hoe het configureren van de vereisten voor het maken van een SQL Server Always On-beschikbaarheidsgroep op Azure Virtual machines.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: c492db4c-3faa-4645-849f-5a1a663be55a
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/29/2018
ms.author: mikeray
ms.openlocfilehash: 1d0f3bfa03eb4bafdd10222e28782c318848b7f7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57995191"
---
# <a name="complete-the-prerequisites-for-creating-always-on-availability-groups-on-azure-virtual-machines"></a>Voldoen aan de vereisten voor het maken van AlwaysOn-beschikbaarheidsgroepen op Azure virtual machines

Deze zelfstudie laat zien hoe om te voldoen aan de vereisten voor het maken van een [SQL Server Always On-beschikbaarheidsgroep op Azure virtual machines (VM's)](virtual-machines-windows-portal-sql-availability-group-tutorial.md). Wanneer u klaar bent met de vereisten, hebt u een domeincontroller, twee SQL Server-VM's en een witness-server in één resourcegroep bestaan.

**Geschatte tijd**: Het duurt een paar uur om te voldoen aan de vereisten. Veel van deze tijd wordt besteed aan het maken van virtuele machines.

Het volgende diagram ziet u wat u in de zelfstudie maken.

![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="review-availability-group-documentation"></a>Raadpleeg de documentatie voor beschikbaarheid van groep

In deze zelfstudie wordt ervan uitgegaan dat u een basiskennis hebt van SQL Server Always On availability groups hebt. Als u niet bekend met deze technologie bent, Zie [overzicht van de Always On Availability Groups (SQL Server)](https://msdn.microsoft.com/library/ff877884.aspx).


## <a name="create-an-azure-account"></a>Maak een Azure-account
U hebt een Azure-account nodig. U kunt [een gratis Azure-account openen](https://signup.azure.com/signup?offer=ms-azr-0044p&appId=102&ref=azureplat-generic&redirectURL=https:%2F%2Fazure.microsoft.com%2Fget-started%2Fwelcome-to-azure%2F&correlationId=24f9d452-1909-40d7-b609-2245aa7351a6&l=en-US) of [voordelen voor Visual Studio-abonnee activeren](https://docs.microsoft.com/visualstudio/subscriptions/subscriber-benefits).

## <a name="create-a-resource-group"></a>Een resourcegroep maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **+** naar een nieuw object maakt in de portal.

   ![Nieuw object](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-portalplus.png)

3. Type **resourcegroep** in de **Marketplace** zoekvenster.

   ![Resourcegroep](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroupsymbol.png)
4. Klik op **resourcegroep**.
5. Klik op **Create**.
6. Onder **groepsnaam voor Accountresources**, typ een naam voor de resourcegroep. Typ bijvoorbeeld **sql-ha-rg**.
7. Als u meerdere Azure-abonnementen hebt, controleert u of dat het abonnement is de Azure-abonnement dat u wilt maken van de beschikbaarheidsgroep in.
8. Selecteer een locatie. De locatie is de Azure-regio waar u wilt maken van de beschikbaarheidsgroep. Dit artikel is gebaseerd alle resources in een Azure-locatie.
9. Controleer **vastmaken aan dashboard** is ingeschakeld. Deze optionele instelling plaatst een snelkoppeling naar de resourcegroep op de Azure portal-dashboard.

   ![Resourcegroep](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroup.png)

10. Klik op **Maken** om de resourcegroep te maken.

Azure maakt de resourcegroep en pincodes een snelkoppeling naar de resourcegroep in de portal.

## <a name="create-the-network-and-subnets"></a>Het netwerk en subnetten maken
De volgende stap is het maken van de netwerken en subnetten in de Azure-resourcegroep.

De oplossing maakt gebruik van één virtueel netwerk met twee subnetten. De [overzicht van Virtual network](../../../virtual-network/virtual-networks-overview.md) vindt u meer informatie over netwerken in Azure.

Het virtuele netwerk maken:

1. In de Azure portal, in de resourcegroep, klikt u op **+ toevoegen**. 

   ![Nieuw item](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/02-newiteminrg.png)
2. Zoeken naar **virtueel netwerk**.

     ![Virtueel netwerk zoeken](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/04-findvirtualnetwork.png)
3. Klik op **virtueel netwerk**.
4. Op de **virtueel netwerk**, klikt u op de **Resource Manager** implementatiemodel, en klik vervolgens op **maken**.

    De volgende tabel ziet u de instellingen voor het virtuele netwerk:

   | **Veld** | Value |
   | --- | --- |
   | **Naam** |autoHAVNET |
   | **Adresruimte** |10.33.0.0/24 |
   | **Subnetnaam** |Gemeente |
   | **Subnetadresbereik** |10.33.0.0/29 |
   | **Abonnement** |Geef het abonnement dat u wilt gebruiken. **Abonnement** is leeg als u slechts één abonnement hebt. |
   | **Resourcegroep** |Kies **gebruik bestaande** en kies de naam van de resourcegroep. |
   | **Locatie** |Geef de Azure-locatie. |

   Uw adres ruimte en subnet-adresbereik kan afwijken van de tabel. Afhankelijk van uw abonnement stelt de portal voor een beschikbare adresruimte en het bijbehorende adresbereik. Als er geen onvoldoende adresruimte beschikbaar is, gebruikt u een ander abonnement.

   Het voorbeeld wordt de naam van het subnet **Admin**. Dit subnet is voor de domeincontrollers.

5. Klik op **Create**.

   ![Het virtuele netwerk configureren](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/06-configurevirtualnetwork.png)

Azure u keert terug naar het dashboard van de portal en ontvangt u een melding wanneer het nieuwe netwerk wordt gemaakt.

### <a name="create-a-second-subnet"></a>Maak een tweede subnet
Het nieuwe virtuele netwerk heeft een subnet, met de naam **Admin**. Dit subnet wordt gebruikt door de domeincontrollers. De SQL Server-VM's gebruiken een tweede subnet met de naam **SQL**. Dit subnet configureren:

1. Op uw dashboard, klikt u op de resourcegroep die u hebt gemaakt, **SQL-HA-RG**. Het netwerk niet vinden in de resourcegroep onder **Resources**.

    Als **SQL-HA-RG** niet wordt weergegeven, vinden door te klikken op **resourcegroepen** en filteren op naam van de resourcegroep.
2. Klik op **autoHAVNET** in de lijst met resources. 
3. Op de **autoHAVNET** virtuele netwerk, onder **instellingen** Selecteer **subnetten**.

    Let op het subnet dat u al hebt gemaakt.

   ![Het virtuele netwerk configureren](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/07-addsubnet.png)
5. Maak een tweede subnet. Klik op **+ Subnet**.
6. Op **subnet toevoegen**, het subnet configureren door te typen **sqlsubnet** onder **naam**. Azure automatisch Hiermee geeft u een geldige **adresbereik**. Controleer of dit adresbereik heeft ten minste 10 adressen erin. In een productieomgeving mogelijk meer adressen.
7. Klik op **OK**.

    ![Het virtuele netwerk configureren](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/08-configuresubnet.png)

De volgende tabel geeft een overzicht van de configuratie-instellingen van het netwerk:

| **Veld** | Value |
| --- | --- |
| **Naam** |**autoHAVNET** |
| **Adresruimte** |Deze waarde is afhankelijk van de beschikbare adresruimten in uw abonnement. Een typische waarde is 10.0.0.0/16. |
| **Subnetnaam** |**admin** |
| **Subnetadresbereik** |Deze waarde is afhankelijk van de beschikbare adresbereiken in uw abonnement. Een typische waarde is 10.0.0.0/24. |
| **Subnetnaam** |**sqlsubnet** |
| **Subnetadresbereik** |Deze waarde is afhankelijk van de beschikbare adresbereiken in uw abonnement. Een typische waarde is 10.0.1.0/24. |
| **Abonnement** |Geef het abonnement dat u wilt gebruiken. |
| **Resourcegroep** |**SQL-HA-RG** |
| **Locatie** |Geef op dezelfde locatie die u hebt gekozen voor de resourcegroep. |

## <a name="create-availability-sets"></a>Beschikbaarheidssets maken

Voordat u virtuele machines maakt, moet u maken van beschikbaarheidssets. Beschikbaarheidssets verminderen de downtime voor gepland of ongepland onderhoud. Een Azure-beschikbaarheidsset is een logische groep resources die door Azure wordt geplaatst op fysieke domeinen met fouten en updatedomeinen. Een foutdomein zorgt ervoor dat de leden van de beschikbaarheidsset afzonderlijk power- en netwerkbronnen hebben. Een updatedomein zorgt ervoor dat leden van de beschikbaarheidsset niet worden opgehaald voor onderhoud op hetzelfde moment. Zie voor meer informatie, [de beschikbaarheid van virtuele machines beheren](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Moet u twee beschikbaarheidssets. Eén meter is voor de domeincontrollers. De tweede is voor de SQL Server-VM's.

Een beschikbaarheidsset maken, gaat u naar de resourcegroep en klik op **toevoegen**. De resultaten filteren door te typen **beschikbaarheidsset**. Klik op **Beschikbaarheidsset** in de resultaten en klik vervolgens op **maken**.

Configureer twee beschikbaarheidssets op basis van de parameters in de volgende tabel:

| **Veld** | Domain controller beschikbaarheidsset | SQL Server-beschikbaarheidsset |
| --- | --- | --- |
| **Naam** |adavailabilityset |sqlavailabilityset |
| **Resourcegroep** |SQL-HA-RG |SQL-HA-RG |
| **Domeinen met fouten** |3 |3 |
| **Update-domeinen** |5 |3 |

Nadat u de beschikbaarheidssets maakt, kunt u terugkeren naar de resourcegroep in Azure portal.

## <a name="create-domain-controllers"></a>Maken van domeincontrollers
Nadat u het netwerk, subnetten, beschikbaarheidssets, en een internetgerichte load balancer hebt gemaakt, bent u klaar om te maken van de virtuele machines voor de domeincontrollers.

### <a name="create-virtual-machines-for-the-domain-controllers"></a>Maken van virtuele machines voor de domeincontrollers
Als u wilt maken en configureren van de domeincontrollers, gaat u terug naar de **SQL-HA-RG** resourcegroep.

1. Klik op **Add**. 
2. Type **Windows Server 2016 Datacenter**.
3. Klik op **Windows Server 2016 Datacenter**. In **Windows Server 2016 Datacenter**, Controleer of het implementatiemodel **Resource Manager**, en klik vervolgens op **maken**. 

Herhaal de voorgaande stappen voor het maken van twee virtuele machines. Naam van de twee virtuele machines:

* ad-primary-dc
* ad-secondary-dc

  > [!NOTE]
  > De **ad-secundaire-dc** virtuele machine is optioneel, voor hoge beschikbaarheid van Active Directory Domain Services.
  >
  >

De volgende tabel ziet u de instellingen voor deze twee machines:

| **Veld** | Value |
| --- | --- |
| **Naam** |Eerste domeincontroller: *ad-primaire-dc*.</br>Tweede domeincontroller *ad-secundaire-dc*. |
| **Type VM-schijf** |SSD |
| **Gebruikersnaam** |DomainAdmin |
| **Wachtwoord** |Contoso!0000 |
| **Abonnement** |*Uw abonnement* |
| **Resourcegroep** |SQL-HA-RG |
| **Locatie** |*Uw locatie* |
| **Grootte** |DS1_V2 |
| **Storage** | **Beheerde schijven gebruiken** - **Ja** |
| **Virtueel netwerk** |autoHAVNET |
| **Subnet** |beheerder |
| **Openbaar IP-adres** |*Dezelfde naam als de virtuele machine* |
| **Netwerkbeveiligingsgroep** |*Dezelfde naam als de virtuele machine* |
| **Beschikbaarheidsset** |adavailabilityset </br>**Foutdomeinen**: 2 </br>**Updatedomeinen**: 2|
| **Diagnostics** |Ingeschakeld |
| **Opslagaccount voor diagnostische gegevens** |*Automatisch gemaakt* |

   >[!IMPORTANT]
   >U kunt een virtuele machine alleen plaatsen in een beschikbaarheidsset wanneer u deze maakt. U kunt de beschikbaarheidsset nadat een virtuele machine is gemaakt, niet wijzigen. Zie [de beschikbaarheid van virtuele machines beheren](../manage-availability.md).

Azure maakt de virtuele machines.

Nadat de virtuele machines worden gemaakt, configureert u de domeincontroller.

### <a name="configure-the-domain-controller"></a>De domeincontroller configureren
In de volgende stappen configureert de **ad-primaire-dc** machine als een domeincontroller voor corp.contoso.com.

1. Open in de portal de **SQL-HA-RG** resource-groep en selecteer de **ad-primaire-dc** machine. Op **ad-primaire-dc**, klikt u op **Connect** een RDP-bestand voor toegang tot extern bureaublad te openen.

    ![Verbinding maken met een virtuele machine](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/20-connectrdp.png)
2. Meld u aan met uw geconfigureerde administrator-account (**\DomainAdmin**) en het wachtwoord (**Contoso! 0000**).
3. Standaard de **Serverbeheer** dashboard moet worden weergegeven.
4. Klik op de **functies en onderdelen toevoegen** koppeling op het dashboard.

    ![Server Manager - rollen toevoegen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
5. Selecteer **volgende** totdat u de **serverfuncties** sectie.
6. Selecteer de **Active Directory Domain Services** en **DNS-Server** rollen. Wanneer u wordt gevraagd, voeg eventuele extra functies die voor deze rollen vereist zijn.

   > [!NOTE]
   > Windows waarschuwt u dat er geen statische IP-adres is. Als u de configuratie van de test, klikt u op **doorgaan**. Voor productiescenario's, stelt u het IP-adres in statisch in de Azure-portal of [PowerShell gebruiken voor het instellen van het statische IP-adres van de domain controller machine](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   >
   >

    ![Dialoogvenster functies toevoegen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/23-addroles.png)
7. Klik op **volgende** totdat de **bevestiging** sectie. Selecteer de **de doelserver automatisch opnieuw opstarten indien nodig** selectievakje.
8. Klik op **Install**.
9. Zodra de functies geïnstalleerd hebt, gaat u terug naar de **Serverbeheer** dashboard.
10. Selecteer de nieuwe **AD DS** optie in het linkerdeelvenster.
11. Klik op de **meer** koppeling op de gele balk van de waarschuwing.

    ![AD DS-dialoogvenster op de DNS-Server-VM](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/24-addsmore.png)
12. In de **actie** kolom van de **alle Server-taakdetails** dialoogvenster, klikt u op **deze server verhogen naar een domeincontroller**.
13. In de **Active Directory Domain Services-configuratiewizard**, gebruikt u de volgende waarden:

    | **Page** | Instelling |
    | --- | --- |
    | **Implementatieconfiguratie** |**Een nieuwe forest toevoegen**<br/> **Naam van hoofddomein** corp.contoso.com = |
    | **Opties voor domeincontroller** |**DSRM-wachtwoord** = Contoso! 0000<br/>**Bevestig het wachtwoord** = Contoso! 0000 |
14. Klik op **volgende** te doorlopen van de andere pagina's in de wizard. Op de **controle van vereisten** pagina, controleert u of dat u ziet het volgende bericht: **Alle controles van vereisten is geslaagd**. U kunt eventuele waarschuwingsberichten die van toepassing zijn bekijken, maar het is mogelijk om door te gaan met de installatie.
15. Klik op **Install**. De **ad-primaire-dc** virtuele machine automatisch opnieuw wordt opgestart.

### <a name="note-the-ip-address-of-the-primary-domain-controller"></a>Noteer het IP-adres van de primaire domeincontroller

Gebruik de primaire domeincontroller voor DNS. Houd er rekening mee de primaire domeincontroller IP-adres.

Eén manier om op te halen van het primaire domeincontroller-IP-adres is via de Azure-portal.

1. Open op de Azure-portal, de resourcegroep.

2. Klik op de primaire domeincontroller.

3. Klik op de primaire domeincontroller **netwerkinterfaces**.

![Netwerkinterfaces](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/25-primarydcip.png)

Houd er rekening mee de privé IP-adres voor deze server.

### <a name="configure-the-virtual-network-dns"></a>Het virtuele netwerk DNS configureren
Nadat u de eerste domeincontroller en DNS op de eerste server inschakelen, configureert u het virtuele netwerk voor het gebruik van deze server voor DNS.

1. Klik op het virtuele netwerk in de Azure-portal.

2. Onder **instellingen**, klikt u op **DNS-Server**.

3. Klik op **aangepaste**, en typt u het privé IP-adres van de primaire domeincontroller.

4. Klik op **Opslaan**.

### <a name="configure-the-second-domain-controller"></a>De tweede domeincontroller configureren
Nadat de primaire domeincontroller opnieuw is opgestart, kunt u de tweede domeincontroller configureren. Deze optionele stap is voor hoge beschikbaarheid. Volg deze stappen voor het configureren van de tweede domeincontroller:

1. Open in de portal de **SQL-HA-RG** resource-groep en selecteer de **ad-secundaire-dc** machine. Op **ad-secundaire-dc**, klikt u op **Connect** een RDP-bestand voor toegang tot extern bureaublad te openen.
2. Aanmelden bij de virtuele machine met behulp van de geconfigureerde administrator-account (**BUILTIN\DomainAdmin**) en het wachtwoord (**Contoso! 0000**).
3. Het adres van de gewenste DNS-server naar het adres van de domeincontroller wijzigen.
4. In **Netwerkcentrum**, klikt u op de netwerkinterface.
   ![Netwerkinterface](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/26-networkinterface.png)

5. Klik op **Eigenschappen**.
6. Selecteer **Internet Protocol versie 4 (TCP/IPv4)** en klikt u op **eigenschappen**.
7. Selecteer **de volgende DNS-serveradressen** en geef het adres van de primaire domeincontroller in **voorkeurs-DNS-server**.
8. Klik op **OK**, en vervolgens **sluiten** de wijzigingen wilt doorvoeren. U bent nu kunnen deelnemen aan de virtuele machine **corp.contoso.com**.

   >[!IMPORTANT]
   >Als u de verbinding met uw extern bureaublad verliest na het wijzigen van de DNS-instelling, gaat u naar de Azure-portal en start de virtuele machine.

9. Open in het externe bureaublad naar de secundaire domeincontroller **Dashboard voor Serverbeheer**.
10. Klik op de **functies en onderdelen toevoegen** koppeling op het dashboard.

    ![Server Manager - rollen toevoegen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
11. Selecteer **volgende** totdat u de **serverfuncties** sectie.
12. Selecteer de **Active Directory Domain Services** en **DNS-Server** rollen. Wanneer u wordt gevraagd, voeg eventuele extra functies die voor deze rollen vereist zijn.
13. Zodra de functies geïnstalleerd hebt, gaat u terug naar de **Serverbeheer** dashboard.
14. Selecteer de nieuwe **AD DS** optie in het linkerdeelvenster.
15. Klik op de **meer** koppeling op de gele balk van de waarschuwing.
16. In de **actie** kolom van de **alle Server-taakdetails** dialoogvenster, klikt u op **deze server verhogen naar een domeincontroller**.
17. Onder **implementatieconfiguratie**, selecteer **een domeincontroller toevoegen aan een bestaand domein**.
    ![Implementatieconfiguratie](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/28-deploymentconfig.png)
18. Klik op **Selecteren**.
19. Verbinding maken met behulp van het administrator-account (**CORP. Contoso.COM\domainadmin**) en het wachtwoord (**Contoso! 0000**).
20. In **Selecteer een domein in het forest**, klikt u op uw domein en klik vervolgens op **OK**.
21. In **opties voor domeincontroller**, gebruikt u de standaardwaarden en een DSRM-wachtwoord instellen.

    >[!NOTE]
    >De **DNS-opties** pagina mogelijk waarschuwt u dat een overdracht voor deze DNS-server kan niet worden gemaakt. U kunt deze waarschuwing in niet-productieomgevingen negeren.
22. Klik op **volgende** totdat het dialoogvenster de **vereisten** controleren. Klik vervolgens op **Installeren**.

Nadat de server klaar is met de wijzigingen in de configuratie, moet u de server opnieuw opstarten.

### <a name="add-the-private-ip-address-to-the-second-domain-controller-to-the-vpn-dns-server"></a>Het privé IP-adres toevoegen aan de tweede domeincontroller voor de VPN-DNS-Server

In Azure portal, onder het virtuele netwerk, de DNS-Server zodanig dat het IP-adres van de secundaire domeincontroller te wijzigen. Deze instelling kunt de DNS-serviceredundantie.

### <a name="DomainAccounts"></a> De domeinaccounts configureren

In de volgende stappen configureert u de Active Directory-accounts. De volgende tabel ziet u de accounts:

| |Account voor installatie<br/> |sqlserver-0 <br/>SQL Server en SQL Agent-Service-account |sqlserver-1<br/>SQL Server en SQL Agent-Service-account
| --- | --- | --- | ---
|**Voornaam** |Installeren |SQLSvc1 | SQLSvc2
|**User SamAccountName** |Installeren |SQLSvc1 | SQLSvc2

Gebruik de volgende stappen om elk account te maken.

1. Aanmelden bij de **ad-primaire-dc** machine.
2. In **Serverbeheer**, selecteer **extra**, en klik vervolgens op **Active Directory Administrative Center**.   
3. Selecteer **corp (lokaal)** in het linkerdeelvenster.
4. Aan de rechterkant **taken** venster **nieuw**, en klik vervolgens op **gebruiker**.
   ![Active Directory-beheercentrum](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/29-addcnewuser.png)

   >[!TIP]
   >Stel een complex wachtwoord in voor elk account.<br/> Voor niet-productieomgevingen, stelt u het gebruikersaccount op nooit verlopen.

5. Klik op **OK** om de gebruiker te maken.
6. Herhaal de voorgaande stappen voor elk van de drie accounts.

### <a name="grant-the-required-permissions-to-the-installation-account"></a>De vereiste machtigingen om het installatie-account te verlenen
1. In de **Active Directory Administrative Center**, selecteer **corp (lokaal)** in het linkerdeelvenster. Klik in de rechterkolom **taken** deelvenster, klikt u op **eigenschappen**.

    ![CORP-gebruikerseigenschappen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/31-addcproperties.png)
2. Selecteer **extensies**, en klik vervolgens op de **Geavanceerd** knop op de **Security** tabblad.
3. In de **geavanceerde beveiligingsinstellingen voor de corp** dialoogvenster, klikt u op **toevoegen**.
4. Klik op **een principal selecteren**, zoeken naar **CORP\Install**, en klik vervolgens op **OK**.
5. Selecteer de **alle eigenschappen lezen** selectievakje.

6. Selecteer de **Computerobjecten maken** selectievakje.

     ![De machtigingen van de Corp-gebruiker](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/33-addpermissions.png)
7. Klik op **OK**, en klik vervolgens op **OK** opnieuw. Sluit de **corp** eigenschappenvenster.

Nu dat u klaar bent met het configureren van Active Directory en de gebruikersobjecten, twee SQL Server-VM's en een witness-server-VM maken. Vervolgens worden alle drie aan het domein toegevoegd.

## <a name="create-sql-server-vms"></a>SQL Server-VM's maken

Maak drie extra virtuele machines. De oplossing vereist twee virtuele machines met SQL Server-exemplaren. Een derde virtuele machine fungeert als een witness. Windows Server 2016 kunt gebruiken een [cloud witness](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness), maar voor consistentie met eerdere besturingssystemen in dit document maakt gebruik van een virtuele machine voor een witness.  

Houd rekening met de volgende ontwerpbeslissingen nemen voordat u doorgaat.

* **Azure Managed Disks voor Storage-**

   Voor de virtuele machine-opslag, Azure Managed Disks te gebruiken. Microsoft raadt Managed Disks voor virtuele machines met SQL Server. Managed Disks verwerken de opslag achter de schermen. Bovendien distribueert Azure de opslagresources zodat voldoende redundantie wordt geboden wanneer virtuele machines met Managed Disks zich in dezelfde beschikbaarheidsset bevinden. Zie [Azure Managed Disks overview](../managed-disks-overview.md) (Overzicht van Azure Managed Disks) voor meer informatie. Zie voor meer informatie over beheerde schijven in een beschikbaarheidsset [beheerde schijven gebruiken voor virtuele machines in een beschikbaarheidsset](../manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).

* **Netwerk - privé IP-adressen in de productieomgeving**

   In deze zelfstudie wordt voor virtuele machines van de openbare IP-adressen. Een openbaar IP-adres kan externe verbinding rechtstreeks naar de virtuele machine via internet - het configuratiestappen eenvoudiger maakt. In een productieomgeving, wordt aangeraden alleen privé IP-adressen om te reduceren de kwetsbaarheidfootprint van het exemplaar van SQL Server VM-resource.

### <a name="create-and-configure-the-sql-server-vms"></a>Maken en configureren van de SQL Server-VM 's
Maak vervolgens drie virtuele machines--twee SQL Server-VM's en een virtuele machine voor een extra clusterknooppunt. Voor het maken van elk van de virtuele machines, gaat u terug naar de **SQL-HA-RG** resourcegroep, klikt u op **toevoegen**, zoeken naar het juiste galerie-item, klikt u op **virtuele Machine**, en klik vervolgens op  **Uit galerie**. Gebruik de informatie in de volgende tabel voor het maken van de virtuele machines:


| Pagina | VM1 | VM2 | VM3 |
| --- | --- | --- | --- |
| Selecteer het juiste galerie-item |**Windows Server 2016 Datacenter** |**SQL Server 2016 SP1 Enterprise op WindowsServer 2016** |**SQL Server 2016 SP1 Enterprise op WindowsServer 2016** |
| Virtuele-machineconfiguratie **grondbeginselen** |**Naam** cluster fsw =<br/>**Gebruikersnaam** DomainAdmin =<br/>**Wachtwoord** = Contoso! 0000<br/>**Abonnement** = van uw abonnement<br/>**Resourcegroep** = SQL-HA-RG<br/>**Locatie** = van uw azure-locatie |**Naam** sqlserver-0 =<br/>**Gebruikersnaam** DomainAdmin =<br/>**Wachtwoord** = Contoso! 0000<br/>**Abonnement** = van uw abonnement<br/>**Resourcegroep** = SQL-HA-RG<br/>**Locatie** = van uw azure-locatie |**Naam** = SQL Server-1<br/>**Gebruikersnaam** DomainAdmin =<br/>**Wachtwoord** = Contoso! 0000<br/>**Abonnement** = van uw abonnement<br/>**Resourcegroep** = SQL-HA-RG<br/>**Locatie** = van uw azure-locatie |
| Virtuele-machineconfiguratie **grootte** |**GROOTTE** = DS1\_V2 (1 vCPU, 3,5 GB) |**GROOTTE** DS2 =\_V2 (2 vcpu's, 7 GB)</br>De grootte moet ondersteuning van SSD-opslag (Premium disk-ondersteuning. )) |**GROOTTE** DS2 =\_V2 (2 vcpu's, 7 GB) |
| Virtuele-machineconfiguratie **instellingen** |**Opslag**: Beheerde schijven gebruiken.<br/>**Virtueel netwerk** autoHAVNET =<br/>**Subnet** sqlsubnet(10.1.1.0/24) =<br/>**Openbare IP-adres** automatisch is gegenereerd.<br/>**Netwerkbeveiligingsgroep** = None<br/>**Bewaking diagnose** = ingeschakeld<br/>**Opslagaccount voor diagnostische gegevens** = gebruik een automatisch gegenereerde storage-account<br/>**Beschikbaarheidsset** sqlAvailabilitySet =<br/> |**Opslag**: Beheerde schijven gebruiken.<br/>**Virtueel netwerk** autoHAVNET =<br/>**Subnet** sqlsubnet(10.1.1.0/24) =<br/>**Openbare IP-adres** automatisch is gegenereerd.<br/>**Netwerkbeveiligingsgroep** = None<br/>**Bewaking diagnose** = ingeschakeld<br/>**Opslagaccount voor diagnostische gegevens** = gebruik een automatisch gegenereerde storage-account<br/>**Beschikbaarheidsset** sqlAvailabilitySet =<br/> |**Opslag**: Beheerde schijven gebruiken.<br/>**Virtueel netwerk** autoHAVNET =<br/>**Subnet** sqlsubnet(10.1.1.0/24) =<br/>**Openbare IP-adres** automatisch is gegenereerd.<br/>**Netwerkbeveiligingsgroep** = None<br/>**Bewaking diagnose** = ingeschakeld<br/>**Opslagaccount voor diagnostische gegevens** = gebruik een automatisch gegenereerde storage-account<br/>**Beschikbaarheidsset** sqlAvailabilitySet =<br/> |
| Virtuele-machineconfiguratie **SQL Server-instellingen** |Niet van toepassing |**SQL-verbinding** = privé (binnen Virtueelnetwerk)<br/>**Poort** 1433 =<br/>**SQL-verificatie** = uitschakelen<br/>**Opslagconfiguratie** algemene =<br/>**Automatisch patchen** zondag = om 2:00 uur<br/>**Automatische back-up** = uitgeschakeld</br>**Integratie van Azure Key Vault** = uitgeschakeld |**SQL-verbinding** = privé (binnen Virtueelnetwerk)<br/>**Poort** 1433 =<br/>**SQL-verificatie** = uitschakelen<br/>**Opslagconfiguratie** algemene =<br/>**Automatisch patchen** zondag = om 2:00 uur<br/>**Automatische back-up** = uitgeschakeld</br>**Integratie van Azure Key Vault** = uitgeschakeld |

<br/>

> [!NOTE]
> De hier voorgestelde machinegrootten zijn bedoeld voor het testen van beschikbaarheidsgroepen in virtuele Azure-machines. Voor de beste prestaties voor werkbelastingen voor productie, verschijnen de aanbevelingen voor SQL Server-machinegrootten en configuratie in [best practices prestaties for SQL Server in virtuele Azure-machines](virtual-machines-windows-sql-performance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

Nadat de drie virtuele machines zijn volledig is ingericht, dient u te laten deelnemen aan de **corp.contoso.com** domein en het verlenen CORP\Install beheerdersrechten heeft op de virtuele machines.

### <a name="joinDomain"></a>De servers toevoegen aan het domein

U kunt nu kunnen deelnemen aan de VM's **corp.contoso.com**. Voer de volgende stappen uit voor zowel de SQL Server-VM's en de file share witness-server:

1. Extern verbinding maken met de virtuele machine met **BUILTIN\DomainAdmin**.
2. In **Serverbeheer**, klikt u op **lokale Server**.
3. Klik op de **werkgroep** koppeling.
4. In de **computernaam** sectie, klikt u op **wijziging**.
5. Selecteer de **domein** selectievakje en typ **corp.contoso.com** in het tekstvak in. Klik op **OK**.
6. In de **Windows Security** pop-upvenster dialoogvenster, geeft u de referenties voor de administrator-account van de standaard-domein (**CORP\DomainAdmin**) en het wachtwoord (**Contoso! 0000**) .
7. Wanneer u het bericht "Welkom bij het domein corp.contoso.com" ziet, klikt u op **OK**.
8. Klik op **sluiten**, en klik vervolgens op **nu opnieuw opstarten** in het dialoogvenster pop-upvenster.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-cluster-vm"></a>De gebruiker Corp\Install toevoegen als beheerder op elke VM-cluster

Nadat elke virtuele machine opnieuw wordt opgestart als een lid van het domein, voegt u toe **CORP\Install** als een lid van de lokale groep administrators.

1. Wacht totdat de virtuele machine opnieuw is opgestart en voer het RDP-bestand op de primaire domeincontroller zich aanmeldt bij het **sqlserver-0** met behulp van de **CORP\DomainAdmin** account.
   >[!TIP]
   >Zorg ervoor dat u zich met het domein administrator-account aanmelden. In de vorige stappen zijn u met behulp van het beheerdersaccount IN gebouwd. Nu dat de server zich in het domein, moet u de domeinaccount gebruiken. Geef in het RDP-sessie *domein*\\*gebruikersnaam*.

2. In **Serverbeheer**, selecteer **extra**, en klik vervolgens op **Computerbeheer**.
3. In de **Computerbeheer** venster, vouw **lokale gebruikers en groepen**, en selecteer vervolgens **groepen**.
4. Dubbelklik op de **beheerders** groep.
5. In de **eigenschappen van de beheerder** dialoogvenster, klikt u op de **toevoegen** knop.
6. Voer de gebruiker **CORP\Install**, en klik vervolgens op **OK**.
7. Klik op **OK** sluiten de **eigenschappen van Administrator** dialoogvenster.
8. Herhaal de vorige stappen op **sqlserver-1** en **cluster fsw**.

### <a name="setServiceAccount"></a>De SQL Server-service-accounts instellen

Stel de SQL Server-serviceaccount op elke virtuele machine van SQL Server. Gebruik van de accounts die u hebt gemaakt tijdens het configureren van de domeinaccounts.

1. Open **SQL Server Configuration Manager**.
2. Met de rechtermuisknop op de SQL Server-service, en klik vervolgens op **eigenschappen**.
3. Stel het account en wachtwoord.
4. Herhaal deze stappen op de andere SQL Server-VM.  

Voor SQL Server-beschikbaarheidsgroepen moet elke SQL Server-VM uit te voeren als een domeinaccount.

### <a name="create-a-sign-in-on-each-sql-server-vm-for-the-installation-account"></a>Een aanmelding op elke SQL Server-VM voor de installatieaccount maken

Gebruik het installatieaccount (CORP\install) voor de beschikbaarheidsgroep configureren. Dit account moet lid zijn van de **sysadmin** vaste serverrol op elke SQL Server-VM. De volgende stappen maakt u een aanmelding voor de installatieaccount:

1. Verbinding maken met de server via het Remote Desktop Protocol (RDP) met behulp van de  *\<MachineName\>\DomainAdmin* account.

1. Open SQL Server Management Studio en maak verbinding met het lokale exemplaar van SQL Server.

1. In **Objectverkenner**, klikt u op **Security**.

1. Met de rechtermuisknop op **aanmeldingen**. Klik op **nieuwe aanmelding**.

1. In **aanmelding - nieuw**, klikt u op **zoeken**.

1. Klik op **locaties**.

1. Voer de referenties voor de domeinbeheerder netwerk.

1. De installatieaccount gebruiken.

1. Stel de aanmelding moet een lid van de **sysadmin** vaste serverrol.

1. Klik op **OK**.

Herhaal de voorgaande stappen op de andere SQL Server-VM.

## <a name="add-failover-clustering-features-to-both-sql-server-vms"></a>Failover Clustering-functies toevoegen voor zowel SQL Server-VM 's

Om toe te voegen functies Failover Clustering, voer de volgende stappen uit op beide VM's van SQL Server:

1. Verbinding maken met de virtuele machine voor SQL Server via het Remote Desktop Protocol (RDP) met behulp van de *CORP\install* account. Open **Dashboard voor Serverbeheer**.
2. Klik op de **functies en onderdelen toevoegen** koppeling op het dashboard.

    ![Server Manager - rollen toevoegen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
3. Selecteer **volgende** totdat u de **serverfuncties** sectie.
4. In **functies**, selecteer **Failover Clustering**.
5. Eventuele aanvullende vereiste onderdelen toevoegen.
6. Klik op **installeren** om toe te voegen de functies.

Herhaal de stappen op de andere SQL Server-VM.

  >[!NOTE]
  > Deze stap, samen met daadwerkelijk de SQL Server-VM's toevoegen aan het failovercluster kan nu worden geautomatiseerd met [Azure SQL VM CLI](virtual-machines-windows-sql-availability-group-cli.md) en [Azure-Snelstartsjablonen](virtual-machines-windows-sql-availability-group-quickstart-template.md).


## <a name="a-nameendpoint-firewall-configure-the-firewall-on-each-sql-server-vm"></a><a name="endpoint-firewall"> De firewall configureren op elke SQL Server-VM

De oplossing is vereist voor de volgende TCP-poorten moeten zijn geopend in de firewall:

- **SQL Server-machine**:<br/>
   Poort 1433 voor een standaardexemplaar van SQL Server.
- **Azure load balancer-test:**<br/>
   Een beschikbare poort. De voorbeelden gebruiken vaak 59999.
- **Eindpunt voor databasespiegeling:** <br/>
   Een beschikbare poort. De voorbeelden gebruiken vaak 5022.

De firewall-poorten moeten geopend zijn op beide VM's van SQL Server.

De methode van de poorten te openen, is afhankelijk van de firewalloplossing die u gebruikt. De volgende sectie wordt uitgelegd hoe u de poorten openen in Windows Firewall. Open de vereiste poorten op elk van uw SQL Server-VM's.

### <a name="open-a-tcp-port-in-the-firewall"></a>Een TCP-poort in de firewall openen

1. Op de eerste SQL-Server **Start** scherm, start u **Windows Firewall met geavanceerde beveiliging**.
2. Selecteer in het linkerdeelvenster **regels voor binnenkomende verbindingen**. Klik in het rechter deelvenster op **nieuwe regel**.
3. Voor **regeltype**, kiest u **poort**.
4. Geef voor de poort, **TCP** en typt u de juiste poortnummers. Zie het volgende voorbeeld:

   ![SQL-firewall](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/35-tcpports.png)

5. Klik op **volgende**.
6. Op de **actie** pagina, bewaart **de verbinding toestaan** geselecteerd en klik vervolgens op **volgende**.
7. Op de **profiel** pagina, accepteer de standaardinstellingen en klik vervolgens op **volgende**.
8. Op de **naam** pagina, geeft u de regelnaam van een (zoals **Azure LB Probe**) in de **naam** tekstvak in en klik vervolgens op **voltooien**.

Herhaal deze stappen op de tweede SQL Server-VM.

## <a name="configure-system-account-permissions"></a>Machtigingen systeemaccount configureren

Voor het maken van een account voor het systeem-account en de juiste machtigingen, voert u de volgende stappen uit op elke SQL Server-exemplaar:

1. Maak een account voor `[NT AUTHORITY\SYSTEM]` op elke SQL Server-exemplaar. Het volgende script maakt u dit account:

   ```sql
   USE [master]
   GO
   CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS WITH DEFAULT_DATABASE=[master]
   GO 
   ```

1. De volgende machtigingen te verlenen `[NT AUTHORITY\SYSTEM]` op elke SQL Server-exemplaar:

   - `ALTER ANY AVAILABILITY GROUP`
   - `CONNECT SQL`
   - `VIEW SERVER STATE`

   Het volgende script verleent deze machtigingen:

   ```sql
   GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM]
   GO
   GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM]
   GO
   GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM]
   GO 
   ```

## <a name="next-steps"></a>Volgende stappen

* [Een SQL Server Always On-beschikbaarheidsgroep maken op Azure virtual machines](virtual-machines-windows-portal-sql-availability-group-tutorial.md)
