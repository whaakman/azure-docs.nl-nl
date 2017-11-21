---
title: SQL Server-beschikbaarheidsgroepen - virtuele machines in Azure - vereisten | Microsoft Docs
description: Deze zelfstudie laat zien hoe u de vereisten voor het maken van een SQL Server Always On-beschikbaarheidsgroep op Azure Virtual machines configureert.
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: c492db4c-3faa-4645-849f-5a1a663be55a
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/09/2017
ms.author: mikeray
ms.openlocfilehash: 0748e0ffa405fc02f6da7e2c412beec12510fde5
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="complete-the-prerequisites-for-creating-always-on-availability-groups-on-azure-virtual-machines"></a>Voldoen aan de vereisten voor het maken van AlwaysOn-beschikbaarheidsgroepen op virtuele machines in Azure

Deze zelfstudie laat zien hoe u voldoen aan de vereisten voor het maken van een [SQL Server Always On beschikbaarheidsgroep op Azure virtuele machines (VM's)](virtual-machines-windows-portal-sql-availability-group-tutorial.md). Wanneer u klaar bent met de vereisten, hebt u een domeincontroller, twee SQL Server-VM's en een witness-server in één resourcegroep.

**Tijd schatting**: het duurt een paar uur om te voldoen aan de vereisten. Veel van deze tijd wordt besteed aan het maken van virtuele machines.

Het volgende diagram ziet u wat u in de zelfstudie bouwen.

![Beschikbaarheidsgroep](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="review-availability-group-documentation"></a>Documentatie voor beschikbaarheid van groep lezen

Deze zelfstudie wordt ervan uitgegaan dat u basiskennis van SQL Server Always On availability groups. Als u niet bekend met deze technologie bent, Zie [overzicht van AlwaysOn-beschikbaarheidsgroepen (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).


## <a name="create-an-azure-account"></a>Maak een Azure-account
U hebt een Azure-account nodig. U kunt [gratis Azure-account openen](/pricing/free-trial/?WT.mc_id=A261C142F) of [voordelen als Visual Studio-abonnee activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

## <a name="create-a-resource-group"></a>Een resourcegroep maken
1. Meld u aan bij [Azure Portal](http://portal.azure.com).
2. Klik op  **+**  voor het maken van een nieuw object in de portal.

   ![Nieuw object](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-portalplus.png)

3. Type **resourcegroep** in de **Marketplace** zoekvenster.

   ![Resourcegroep](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroupsymbol.png)
4. Klik op **resourcegroep**.
5. Klik op **Create**.
6. Op de **resourcegroep** blade onder **Resourcegroepnaam**, typ een naam voor de resourcegroep. Typ bijvoorbeeld **sql-ha-rg**.
7. Als u meerdere Azure-abonnementen hebt, controleert u of het abonnement de Azure-abonnement dat u wilt maken van de beschikbaarheidsgroep in.
8. Selecteer een locatie. De locatie is de Azure-regio waar u wilt maken van de beschikbaarheidsgroep. Voor deze zelfstudie gaan we voor het bouwen van alle resources in een Azure-locatie.
9. Controleer **vastmaken aan dashboard** is ingeschakeld. Deze instelling optioneel wordt een snelkoppeling voor de resourcegroep op de Azure-portaldashboard.

   ![Resourcegroep](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroup.png)

10. Klik op **maken** om de resourcegroep te maken.

Azure maakt de resourcegroep en pincodes een snelkoppeling naar de resourcegroep in de portal.

## <a name="create-the-network-and-subnets"></a>De netwerk- en subnetten maken
De volgende stap is het maken van de netwerken en subnetten in de Azure-resourcegroep.

De oplossing maakt gebruik van een virtueel netwerk met twee subnetten. De [Virtual network-overzicht](../../../virtual-network/virtual-networks-overview.md) vindt u meer informatie over netwerken in Azure.

Het virtuele netwerk maken:

1. Klik in de Azure-portal in de resourcegroep op **+ toevoegen**. Azure wordt geopend de **Alles** blade.

   ![Nieuw item](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/02-newiteminrg.png)
2. Zoeken naar **virtueel netwerk**.

     ![Het virtuele netwerk zoeken](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/04-findvirtualnetwork.png)
3. Klik op **virtueel netwerk**.
4. Op de **virtueel netwerk** blade, klikt u op de **Resource Manager** implementatiemodel en klik vervolgens op **maken**.

    De volgende tabel ziet u de instellingen voor het virtuele netwerk:

   | **Veld** | Waarde |
   | --- | --- |
   | **Naam** |autoHAVNET |
   | **Adresruimte** |10.33.0.0/24 |
   | **Subnetnaam** |Beheerder |
   | **Subnetadresbereik** |10.33.0.0/29 |
   | **Abonnement** |Geef het abonnement dat u wilt gebruiken. **Abonnement** is leeg als u slechts één abonnement hebt. |
   | **Resourcegroep** |Kies **gebruik bestaande** en kies de naam van de resourcegroep. |
   | **Locatie** |Geef de Azure-locatie. |

   Uw adres en het subnet-adresbereik kan afwijken van de tabel. Afhankelijk van uw abonnement stelt de portal voor een beschikbare adresruimte en de bijbehorende subnet-adresbereik. Als er geen voldoende adresruimte beschikbaar is, gebruikt u een ander abonnement.

   Het voorbeeld wordt de subnetnaam **Admin**. Dit subnet is voor de domeincontrollers.

5. Klik op **Create**.

   ![Het virtuele netwerk configureren](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/06-configurevirtualnetwork.png)

Azure keert u terug naar het portaldashboard en waarschuwt u als het nieuwe netwerk wordt gemaakt.

### <a name="create-a-second-subnet"></a>Maak een tweede subnet
Het nieuwe virtuele netwerk heeft een subnet, met de naam **Admin**. Dit subnet wordt gebruikt door de domeincontrollers. De VM's van SQL Server gebruikt een tweede subnet met de naam **SQL**. Dit subnet configureren:

1. Klik op de resourcegroep die u hebt gemaakt, op uw dashboard **SQL-HA-RG**. Het netwerk niet vinden in de resourcegroep onder **Resources**.

    Als **SQL-HA-RG** niet wordt weergegeven, vinden door te klikken op **resourcegroepen** en filteren op naam van de resourcegroep.
2. Klik op **autoHAVNET** op de lijst met resources. Azure opent u de blade van de configuratie van netwerk.
3. Op de **autoHAVNET** blade virtueel netwerk onder **instellingen** , klikt u op **subnetten**.

    Let op het subnet dat u al hebt gemaakt.

   ![Het virtuele netwerk configureren](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/07-addsubnet.png)
5. Maak een tweede subnet. Klik op **+ Subnet**.
6. Op de **subnet toevoegen** blade configureren van het subnet met typen **sqlsubnet** onder **naam**. Azure automatisch een geldige geeft **-adresbereik**. Controleer of dit adresbereik ten minste 10 adressen erin. In een productieomgeving mogelijk meer adressen.
7. Klik op **OK**.

    ![Het virtuele netwerk configureren](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/08-configuresubnet.png)

De volgende tabel geeft een overzicht van de configuratie-instellingen van het netwerk:

| **Veld** | Waarde |
| --- | --- |
| **Naam** |**autoHAVNET** |
| **Adresruimte** |Deze waarde is afhankelijk van de beschikbare adresruimten in uw abonnement. Een typische waarde is 10.0.0.0/16. |
| **Subnetnaam** |**beheerder** |
| **Subnetadresbereik** |Deze waarde is afhankelijk van de beschikbare adresbereiken in uw abonnement. Een typische waarde is 10.0.0.0/24. |
| **Subnetnaam** |**sqlsubnet** |
| **Subnetadresbereik** |Deze waarde is afhankelijk van de beschikbare adresbereiken in uw abonnement. Een typische waarde is 10.0.1.0/24. |
| **Abonnement** |Geef het abonnement dat u wilt gebruiken. |
| **Resourcegroep** |**SQL-HA-RG** |
| **Locatie** |Geef dezelfde locatie die u hebt gekozen voor de resourcegroep. |

## <a name="create-availability-sets"></a>Beschikbaarheidssets maken

Voordat u virtuele machines maken, moet u beschikbaarheidssets maken. Beschikbaarheidssets Verminder de uitvaltijd van gebeurtenissen voor gepland of ongepland onderhoud. Een Azure beschikbaarheidsset is een logische groep resources die door Azure wordt geplaatst op fysieke domeinen met fouten en domeinen van de update. Een foutdomein zorgt ervoor dat de leden van de beschikbaarheidsset afzonderlijke energie- en netwerkbronnen. Een updatedomein zorgt ervoor dat de leden van de beschikbaarheidsset niet worden verbroken voor onderhoud op hetzelfde moment. Zie voor meer informatie [de beschikbaarheid van virtuele machines beheren](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

U moet twee beschikbaarheidssets. Een is voor de domeincontrollers. De tweede is voor de VM's van SQL Server.

Een beschikbaarheidsset maken, gaat u naar de resourcegroep en klik op **toevoegen**. De resultaten filteren door te typen **beschikbaarheidsset**. Klik op **Beschikbaarheidsset** in de resultaten en klik vervolgens op **maken**.

Configureer twee beschikbaarheidssets overeenkomstig de parameters in de volgende tabel:

| **Veld** | Beschikbaarheidsset domain-controller | Beschikbaarheidsset voor SQL Server |
| --- | --- | --- |
| **Naam** |adavailabilityset |sqlavailabilityset |
| **Resourcegroep** |SQL-HA-RG |SQL-HA-RG |
| **Domeinen met fouten** |3 |3 |
| **Domeinen bijwerken** |5 |3 |

Nadat u de beschikbaarheidssets hebt gemaakt, keert u terug naar de resourcegroep in de Azure portal.

## <a name="create-domain-controllers"></a>Maken van domeincontrollers
Wanneer u het netwerk, subnetten, beschikbaarheidssets en een Internet gerichte load balancer hebt gemaakt, bent u klaar om te maken van de virtuele machines voor de domeincontrollers.

### <a name="create-virtual-machines-for-the-domain-controllers"></a>Maken van virtuele machines voor de domeincontrollers
Als u wilt maken en configureren van de domeincontrollers, terug naar de **SQL-HA-RG** resourcegroep.

1. Klik op **Add**. De **Alles** blade wordt geopend.
2. Type **Windows Server 2016 Datacenter**.
3. Klik op **Windows Server 2016 Datacenter**. In de **Windows Server 2016 Datacenter** blade, Controleer of het implementatiemodel **Resource Manager**, en klik vervolgens op **maken**. Azure wordt geopend de **virtuele machine maken** blade.

Herhaal de voorgaande stappen voor het maken van twee virtuele machines. Naam van de twee virtuele machines:

* AD-primaire-dc
* AD-secundaire-dc

  > [!NOTE]
  > De **ad-secundaire-dc** virtuele machine is optioneel, voor hoge beschikbaarheid bieden voor Active Directory Domain Services.
  >
  >

De volgende tabel ziet u de instellingen voor deze twee machines:

| **Veld** | Waarde |
| --- | --- |
| **Naam** |Eerste domeincontroller: *ad-primaire-dc*.</br>Tweede domeincontroller *ad-secundaire-dc*. |
| **Type VM-schijf** |SSD |
| **Gebruikersnaam** |DomainAdmin |
| **Wachtwoord** |Contoso! 0000 |
| **Abonnement** |*Uw abonnement* |
| **Resourcegroep** |SQL-HA-RG |
| **Locatie** |*Uw locatie* |
| **Grootte** |DS1_V2 |
| **Storage** | **Gebruik de schijven van de beheerde** - **Ja** |
| **Virtueel netwerk** |autoHAVNET |
| **Subnet** |Beheerder |
| **Openbaar IP-adres** |*Dezelfde naam als de virtuele machine* |
| **Netwerkbeveiligingsgroep** |*Dezelfde naam als de virtuele machine* |
| **Beschikbaarheidsset** |adavailabilityset </br>**Fault-domeinen**: 2</br>**Bijwerken van domeinen**: 2|
| **Diagnostics** |Ingeschakeld |
| **Opslagaccount voor diagnostische gegevens** |*Automatisch gemaakt* |

   >[!IMPORTANT]
   >U kunt alleen een virtuele machine in een beschikbaarheidsset wanneer u deze maakt plaatsen. U kunt de beschikbaarheid instellen nadat een virtuele machine is gemaakt niet wijzigen. Zie [de beschikbaarheid van virtuele machines beheren](../manage-availability.md).

Azure maakt de virtuele machines.

Nadat de virtuele machines worden gemaakt, configureert u de domeincontroller.

### <a name="configure-the-domain-controller"></a>De domeincontroller configureren
In de volgende stappen configureert de **ad-primaire-dc** machines als een domeincontroller voor corp.contoso.com.

1. Open in de portal de **SQL-HA-RG** resource Groepsbeleid en selecteer de **ad-primaire-dc** machine. Op de **ad-primaire-dc** blade, klikt u op **Connect** een RDP-bestand voor toegang tot extern bureaublad te openen.

    ![Verbinding maken met een virtuele machine](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/20-connectrdp.png)
2. Meld u aan met uw geconfigureerde beheerdersaccount (**\DomainAdmin**) en het wachtwoord (**Contoso! 0000**).
3. Standaard de **Serverbeheer** dashboard moet worden weergegeven.
4. Klik op de **functies en onderdelen toevoegen** koppeling in het dashboard.

    ![Server Manager - functies toevoegen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
5. Selecteer **volgende** totdat u de **serverfuncties** sectie.
6. Selecteer de **Active Directory Domain Services** en **DNS-Server** rollen. Wanneer u wordt gevraagd, voeg eventuele aanvullende functies die voor deze rollen vereist zijn.

   > [!NOTE]
   > Windows waarschuwt u dat er geen statische IP-adres is. Als u de configuratie van de test, klikt u op **doorgaan**. Voor productiescenario's, stelt u het IP-adres naar statisch in de Azure-portal of [PowerShell gebruiken voor het instellen van het statische IP-adres van de domain controller machine](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   >
   >

    ![Dialoogvenster rollen toevoegen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/23-addroles.png)
7. Klik op **volgende** totdat u de **bevestiging** sectie. Selecteer de **de doelserver automatisch opnieuw opstarten indien nodig** selectievakje.
8. Klik op **Install**.
9. Nadat de functies geïnstalleerd hebt, keert u terug naar de **Serverbeheer** dashboard.
10. Selecteer de nieuwe **AD DS** optie in het linkerdeelvenster.
11. Klik op de **meer** koppeling op de gele waarschuwing-balk.

    ![Dialoogvenster van de AD DS op de DNS-Server-VM](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/24-addsmore.png)
12. In de **actie** kolom van de **alle Server-taakdetails** dialoogvenster, klikt u op **deze server verhogen naar een domeincontroller**.
13. In de **Wizard Active Directory Domain Services configureren**, gebruik de volgende waarden:

    | **Pagina** | Instelling |
    | --- | --- |
    | **Implementatieconfiguratie** |**Een nieuw forest toevoegen**<br/> **Naam van hoofddomein** corp.contoso.com = |
    | **Opties voor domeincontroller** |**DSRM-wachtwoord** = Contoso! 0000<br/>**Bevestig het wachtwoord** = Contoso! 0000 |
14. Klik op **volgende** te doorlopen van de andere pagina's in de wizard. Op de **controle van vereisten** pagina, controleert u of u het volgende foutbericht weergegeven: **alle controles op vereisten zijn geslaagd**. U kunt toepasselijke waarschuwingsberichten bekijken, maar is het mogelijk om door te gaan met de installatie.
15. Klik op **Install**. De **ad-primaire-dc** virtuele machine automatisch opnieuw wordt opgestart.

### <a name="note-the-ip-address-of-the-primary-domain-controller"></a>Noteer de IP-adres van de primaire domeincontroller

Gebruik de primaire domeincontroller voor DNS. Let op het primaire domeincontroller IP-adres.

Een manier om op te halen van het primaire domeincontroller IP-adres is via de Azure portal.

1. Open de resourcegroep op de Azure-portal.

2. Klik op de primaire domeincontroller.

3. Klik op de blade van de domeincontroller primair domein **netwerkinterfaces**.

![Netwerkinterfaces](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/25-primarydcip.png)

Noteer de privé IP-adres voor deze server.

### <a name="configure-the-virtual-network-dns"></a>Het virtuele netwerk DNS configureren
Na het maken van de eerste domeincontroller en DNS op de eerste server inschakelen, het virtuele netwerk voor het gebruik van deze server voor DNS configureren.

1. Klik in de Azure-portal op het virtuele netwerk.

2. Onder **instellingen**, klikt u op **DNS-Server**.

3. Klik op **aangepaste**, en typt u het privé IP-adres van de primaire domeincontroller.

4. Klik op **Opslaan**.

### <a name="configure-the-second-domain-controller"></a>De tweede domeincontroller configureren
Nadat de primaire domeincontroller opnieuw is opgestart, kunt u de tweede domeincontroller configureren. Deze stap optioneel is voor hoge beschikbaarheid. Volg deze stappen voor het configureren van de tweede domeincontroller:

1. Open in de portal de **SQL-HA-RG** resource Groepsbeleid en selecteer de **ad-secundaire-dc** machine. Op de **ad-secundaire-dc** blade, klikt u op **Connect** een RDP-bestand voor toegang tot extern bureaublad te openen.
2. Aanmelden bij de virtuele machine met behulp van uw geconfigureerde beheerdersaccount (**BUILTIN\DomainAdmin**) en het wachtwoord (**Contoso! 0000**).
3. Het voorkeurs-DNS-serveradres wijzigen op het adres van de domeincontroller.
4. In **Netwerkcentrum**, klikt u op de netwerkinterface.
   ![Netwerkinterface](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/26-networkinterface.png)

5. Klik op **Eigenschappen**.
6. Selecteer **Internet Protocol versie 4 (TCP/IPv4)** en klik op **eigenschappen**.
7. Selecteer **de volgende DNS-serveradressen** en geef het adres van de primaire domeincontroller in **voorkeurs-DNS-server**.
8. Klik op **OK**, en vervolgens **sluiten** de wijzigingen doorvoeren. U bent nu aan de virtuele machine **corp.contoso.com**.

   >[!IMPORTANT]
   >Als u de verbinding met uw extern bureaublad na het wijzigen van de DNS-instelling verliest, gaat u naar de Azure-portal en de virtuele machine opnieuw opstarten.

9. Open in het externe bureaublad naar de secundaire domeincontroller **Dashboard voor Serverbeheer**.
10. Klik op de **functies en onderdelen toevoegen** koppeling in het dashboard.

    ![Server Manager - functies toevoegen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
11. Selecteer **volgende** totdat u de **serverfuncties** sectie.
12. Selecteer de **Active Directory Domain Services** en **DNS-Server** rollen. Wanneer u wordt gevraagd, voeg eventuele aanvullende functies die voor deze rollen vereist zijn.
13. Nadat de functies geïnstalleerd hebt, keert u terug naar de **Serverbeheer** dashboard.
14. Selecteer de nieuwe **AD DS** optie in het linkerdeelvenster.
15. Klik op de **meer** koppeling op de gele waarschuwing-balk.
16. In de **actie** kolom van de **alle Server-taakdetails** dialoogvenster, klikt u op **deze server verhogen naar een domeincontroller**.
17. Onder **implementatieconfiguratie**, selecteer **een domeincontroller toevoegt aan een bestaand domein**.
   ![Implementatieconfiguratie](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/28-deploymentconfig.png)
18. Klik op **Selecteren**.
19. Verbinding maken met behulp van de administrator-account (**CORP. Contoso.COM\domainadmin**) en het wachtwoord (**Contoso! 0000**).
20. In **Selecteer een domein in het forest**, klikt u op uw domein en klik vervolgens op **OK**.
21. In **opties voor domeincontroller**, gebruikt u de standaardwaarden en een DSRM-wachtwoord instellen.

   >[!NOTE]
   >De **DNS-opties** pagina worden gewaarschuwd dat een overdracht voor deze DNS-server kan niet worden gemaakt. U kunt deze waarschuwing in niet-productieve omgevingen negeren.
22. Klik op **volgende** totdat het dialoogvenster bereikt de **vereisten** controleren. Klik vervolgens op **Installeren**.

Nadat de server klaar is voor wijzigingen in de configuratie, moet u de server opnieuw opstarten.

### <a name="add-the-private-ip-address-to-the-second-domain-controller-to-the-vpn-dns-server"></a>Het privé IP-adres toevoegen aan de tweede domeincontroller voor de VPN-DNS-Server

Wijzig de DNS-Server zodanig dat het IP-adres van de secundaire domeincontroller in de Azure-portal onder virtuele netwerk. Hierdoor kan de DNS-service redundantie.

### <a name=DomainAccounts></a>Configureer de domeinaccounts

In de volgende stappen configureert u de Active Directory-accounts. De volgende tabel toont de accounts:

| |Installatie-account<br/> |SQL Server-0 <br/>SQL Server en SQL Agent-Service-account |SQL Server-1<br/>SQL Server en SQL Agent-Service-account
| --- | --- | --- | ---
|**Voornaam** |Installeren |SQLSvc1 | SQLSvc2
|**SamAccountName van gebruiker** |Installeren |SQLSvc1 | SQLSvc2

Gebruik de volgende stappen om elk account te maken.

1. Aanmelden bij de **ad-primaire-dc** machine.
2. In **Serverbeheer**, selecteer **extra**, en klik vervolgens op **Active Directory Administrative Center**.   
3. Selecteer **corp (lokaal)** in het linkerdeelvenster.
4. Aan de rechterkant **taken** deelvenster **nieuw**, en klik vervolgens op **gebruiker**.
   ![Active Directory-beheercentrum](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/29-addcnewuser.png)

   >[!TIP]
   >Stel een complex wachtwoord voor elke account.<br/> Niet-productieve omgevingen, stel de gebruikersaccount nooit verloopt.

5. Klik op **OK** voor het maken van de gebruiker.
6. Herhaal de voorgaande stappen voor elk van de drie accounts.

### <a name="grant-the-required-permissions-to-the-installation-account"></a>De vereiste machtigingen toewijzen aan het installatie-account
1. In de **Active Directory Administrative Center**, selecteer **corp (lokaal)** in het linkerdeelvenster. Klik in de rechterkolom **taken** deelvenster, klikt u op **eigenschappen**.

    ![Gebruikerseigenschappen CORP](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/31-addcproperties.png)
2. Selecteer **extensies**, en klik vervolgens op de **Geavanceerd** knop op de **beveiliging** tabblad.
3. In de **geavanceerde beveiligingsinstellingen voor de corp** dialoogvenster, klikt u op **toevoegen**.
4. Klik op **een principal selecteren**, zoeken naar **CORP\Install**, en klik vervolgens op **OK**.
5. Selecteer de **alle eigenschappen lezen** selectievakje.

6. Selecteer de **Computerobjecten maken** selectievakje.

     ![Gebruikersmachtigingen Corp](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/33-addpermissions.png)
7. Klik op **OK**, en klik vervolgens op **OK** opnieuw. Sluit de **corp** eigenschappenvenster.

Nu dat u klaar bent met het configureren van Active Directory en de gebruikersobjecten, twee SQL Server-VM's en de virtuele machine van een witness-server maken. Vervolgens worden toegevoegd aan alle drie aan het domein.

## <a name="create-sql-server-vms"></a>SQL Server-VM's maken

Drie extra virtuele machines maken. De oplossing vereist twee virtuele machines met SQL Server-exemplaren. Een derde virtuele machine fungeert als een witness. Windows Server 2016 kunt gebruiken een [cloud witness](http://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness), maar voor consistentie met eerdere besturingssystemen in dit document maakt gebruik van een virtuele machine voor een witness.  

Houd rekening met de volgende deisign beslissingen voordat u doorgaat.

* **Opslag - Azure schijven die worden beheerd**

   Voor de virtuele-machineopslag Azure beheerd schijven te gebruiken. Microsoft raadt beheerd schijven voor virtuele machines van SQL Server. Managed Disks verwerken de opslag achter de schermen. Bovendien distribueert Azure de opslagresources zodat voldoende redundantie wordt geboden wanneer virtuele machines met Managed Disks zich in dezelfde beschikbaarheidsset bevinden. Zie [Azure Managed Disks overview](../managed-disks-overview.md) (Overzicht van Azure Managed Disks) voor meer informatie. Zie voor meer details over beheerde schijven in een beschikbaarheidsset [beheerd schijven gebruiken voor virtuele machines in een beschikbaarheidsset](../manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).

* **Netwerk - particuliere IP-adressen in productie**

   Deze zelfstudie maakt gebruik van openbare IP-adressen voor de virtuele machines. Hierdoor kan externe verbinding rechtstreeks naar de virtuele machine via het internet - het configuratiestappen eenvoudiger maakt. Microsoft raadt alleen privé IP-adressen in een productieomgeving aan om te reduceren de kwetsbaarheidfootprint van het exemplaar van SQL Server VM-resource.

### <a name="create-and-configure-the-sql-server-vms"></a>Maken en configureren van de VM's van SQL Server
Maak vervolgens drie virtuele machines--twee SQL Server-VM's en een VM voor een extra clusterknooppunt. Voor het maken van elk van de VM, gaat u terug naar de **SQL-HA-RG** resourcegroep, klikt u op **toevoegen**, zoeken naar het juiste galerij-item, klikt u op **virtuele Machine**, en klik vervolgens op  **Vanuit galerie**. Gebruik de informatie in de volgende tabel kunt u de virtuele machines maken:


| Pagina | VM1 | VM2 | VM3 |
| --- | --- | --- | --- |
| Selecteer het juiste galerij-item |**Windows Server 2016 Datacenter** |**SQL Server 2016 SP1 Enterprise op WindowsServer 2016** |**SQL Server 2016 SP1 Enterprise op WindowsServer 2016** |
| Virtuele-machineconfiguratie **basisbeginselen** |**Naam** cluster fsw =<br/>**Gebruikersnaam** DomainAdmin =<br/>**Wachtwoord** = Contoso! 0000<br/>**Abonnement** = van uw abonnement<br/>**Resourcegroep** SQL-HA-RG =<br/>**Locatie** = van uw azure-locatie |**Naam** sqlserver-0 =<br/>**Gebruikersnaam** DomainAdmin =<br/>**Wachtwoord** = Contoso! 0000<br/>**Abonnement** = van uw abonnement<br/>**Resourcegroep** SQL-HA-RG =<br/>**Locatie** = van uw azure-locatie |**Naam** sqlserver-1 =<br/>**Gebruikersnaam** DomainAdmin =<br/>**Wachtwoord** = Contoso! 0000<br/>**Abonnement** = van uw abonnement<br/>**Resourcegroep** SQL-HA-RG =<br/>**Locatie** = van uw azure-locatie |
| Virtuele-machineconfiguratie **grootte** |**De grootte van** = DS1\_V2 (1 vCPU, 3.5 GB) |**De grootte van** DS2 =\_V2 (2 Vcpu, 7 GB)</br>De grootte moet ondersteuning bieden voor SSD-opslag (ondersteuning van Premium-schijven. )) |**De grootte van** DS2 =\_V2 (2 Vcpu, 7 GB) |
| Virtuele-machineconfiguratie **instellingen** |**Opslag**: gebruik schijven die worden beheerd.<br/>**Virtueel netwerk** autoHAVNET =<br/>**Subnet** sqlsubnet(10.1.1.0/24) =<br/>**Openbaar IP-adres** automatisch is gegenereerd.<br/>**Netwerkbeveiligingsgroep** = geen<br/>**Bewaking van diagnostische gegevens** = ingeschakeld<br/>**Opslagaccount voor diagnostische gegevens** = gebruik een automatisch gegenereerde storage-account<br/>**Beschikbaarheidsset** sqlAvailabilitySet =<br/> |**Opslag**: gebruik schijven die worden beheerd.<br/>**Virtueel netwerk** autoHAVNET =<br/>**Subnet** sqlsubnet(10.1.1.0/24) =<br/>**Openbaar IP-adres** automatisch is gegenereerd.<br/>**Netwerkbeveiligingsgroep** = geen<br/>**Bewaking van diagnostische gegevens** = ingeschakeld<br/>**Opslagaccount voor diagnostische gegevens** = gebruik een automatisch gegenereerde storage-account<br/>**Beschikbaarheidsset** sqlAvailabilitySet =<br/> |**Opslag**: gebruik schijven die worden beheerd.<br/>**Virtueel netwerk** autoHAVNET =<br/>**Subnet** sqlsubnet(10.1.1.0/24) =<br/>**Openbaar IP-adres** automatisch is gegenereerd.<br/>**Netwerkbeveiligingsgroep** = geen<br/>**Bewaking van diagnostische gegevens** = ingeschakeld<br/>**Opslagaccount voor diagnostische gegevens** = gebruik een automatisch gegenereerde storage-account<br/>**Beschikbaarheidsset** sqlAvailabilitySet =<br/> |
| Virtuele-machineconfiguratie **SQL Server-instellingen** |Niet van toepassing |**SQL-connectiviteit** = privé (binnen virtueel netwerk)<br/>**Poort** 1433 =<br/>**SQL-verificatie** = uitschakelen<br/>**Opslagconfiguratie** algemene =<br/>**Automatisch patchen** = zondag om 2:00 uur<br/>**Automatische back-up** = uitgeschakeld</br>**Integratie van Azure Sleutelkluis** = uitgeschakeld |**SQL-connectiviteit** = privé (binnen virtueel netwerk)<br/>**Poort** 1433 =<br/>**SQL-verificatie** = uitschakelen<br/>**Opslagconfiguratie** algemene =<br/>**Automatisch patchen** = zondag om 2:00 uur<br/>**Automatische back-up** = uitgeschakeld</br>**Integratie van Azure Sleutelkluis** = uitgeschakeld |

<br/>

> [!NOTE]
> De grootte van de machines die hier voorgesteld zijn bedoeld voor het testen van beschikbaarheidsgroepen in Azure Virtual machines. Zie de aanbevelingen voor de grootte van de SQL Server-machines en configuratie-instellingen voor de beste prestaties op productieworkloads [best practices prestaties for SQL Server op Azure virtual machines](virtual-machines-windows-sql-performance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

Nadat de drie virtuele machines zijn volledig is ingericht, moet u kan toevoegen aan de **corp.contoso.com** domein en verleen CORP\Install beheerrechten op de virtuele machines.

### <a name="joinDomain"></a>De servers toevoegen aan het domein

U kunt nu aan de virtuele machines naar **corp.contoso.com**. Doe het volgende voor de VM's van SQL Server en de bestandsserver van de bestandsshare-witness:

1. Extern verbinding maken met de virtuele machine met **BUILTIN\DomainAdmin**.
2. In **Serverbeheer**, klikt u op **lokale Server**.
3. Klik op de **werkgroep** koppeling.
4. In de **computernaam** sectie, klikt u op **wijziging**.
5. Selecteer de **domein** selectievakje in en typ **corp.contoso.com** in het tekstvak. Klik op **OK**.
6. In de **Windows-beveiliging** dialoogvenster pop, geef de referenties voor de administrator-account van het standaard (**CORP\DomainAdmin**) en het wachtwoord (**Contoso! 0000**) .
7. Wanneer u het bericht "Welkom bij het domein corp.contoso.com" ziet, klikt u op **OK**.
8. Klik op **sluiten**, en klik vervolgens op **nu opnieuw opstarten** in het dialoogvenster pop-upvenster.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-cluster-vm"></a>De gebruiker Corp\Install toevoegen als beheerder op elke VM-cluster

Nadat u elke virtuele machine opnieuw wordt opgestart als lid van het domein, toevoegen **CORP\Install** als lid van de lokale groep administrators.

1. Wacht totdat de virtuele machine opnieuw wordt gestart en vervolgens het RDP-bestand van de primaire domeincontroller aan te melden bij opnieuw starten **sqlserver 0** met behulp van de **CORP\DomainAdmin** account.
   >[!TIP]
   >Zorg ervoor dat u zich met het domeinbeheerdersaccount aanmelden. In de vorige stappen hebt u het beheerdersaccount IN gebouwd. Nu dat de server zich in het domein, wordt het domeinaccount gebruiken. Geef in uw RDP-sessie *domein*\\*gebruikersnaam*.

2. In **Serverbeheer**, selecteer **extra**, en klik vervolgens op **Computerbeheer**.
3. In de **Computerbeheer** venster Vouw **lokale gebruikers en groepen**, en selecteer vervolgens **groepen**.
4. Dubbelklik op de **beheerders** groep.
5. In de **beheerders eigenschappen** dialoogvenster, klikt u op de **toevoegen** knop.
6. Voer de gebruiker **CORP\Install**, en klik vervolgens op **OK**.
7. Klik op **OK** sluiten de **beheerder eigenschappen** dialoogvenster.
8. Herhaal de vorige stappen op **sqlserver 1** en **cluster fsw**.

### <a name="setServiceAccount"></a>Instellen van de SQL Server-service-accounts

Stel de SQL Server-serviceaccount op elke virtuele machine van SQL Server. Gebruik van de accounts die u hebt gemaakt wanneer u [de domeinaccounts geconfigureerd](#DomainAccounts).

1. Open **SQL Server Configuration Manager**.
2. Met de rechtermuisknop op de SQL Server-service en klik vervolgens op **eigenschappen**.
3. Stel de account en wachtwoord.
4. Herhaal deze stappen op de andere SQL Server-VM.  

Voor SQL Server-beschikbaarheidsgroepen moet elke virtuele machine van SQL Server worden uitgevoerd als een domeinaccount.

### <a name="create-a-sign-in-on-each-sql-server-vm-for-the-installation-account"></a>Maak een aanmeldingspagina op elke SQL Server-VM voor de installatieaccount

De installatieaccount (CORP\install) gebruiken voor het configureren van de beschikbaarheidsgroep. Dit account moet lid zijn van de **sysadmin** vaste serverrol op elke virtuele machine van SQL Server. De volgende stappen maakt u een aanmeldingspagina voor de installatieaccount:

1. Verbinding maken met de server via de Remote Desktop Protocol (RDP) met behulp van de  *\<MachineName\>\DomainAdmin* account.

1. Open SQL Server Management Studio en maak verbinding met het lokale exemplaar van SQL Server.

1. In **Objectverkenner**, klikt u op **beveiliging**.

1. Met de rechtermuisknop op **aanmeldingen**. Klik op **New Login**.

1. In **Login - nieuwe**, klikt u op **Search**.

1. Klik op **locaties**.

1. Voer de netwerkreferenties voor de domeinbeheerder.

1. De installatieaccount gebruiken.

1. Stel de aanmeldingspagina lid zijn van de **sysadmin** vaste serverrol.

1. Klik op **OK**.

Herhaal de voorgaande stappen op de andere SQL Server-VM.

## <a name="add-failover-clustering-features-to-both-sql-server-vms"></a>Functies voor failoverclustering toevoegen aan beide VM's van SQL Server

Om toe te voegen functies voor failoverclustering, doe het volgende op beide VM's van SQL Server:

1. Verbinding maken met de virtuele machine van SQL Server via de Remote Desktop Protocol (RDP) met behulp van de *CORP\install* account. Open **Dashboard voor Serverbeheer**.
2. Klik op de **functies en onderdelen toevoegen** koppeling in het dashboard.

    ![Server Manager - functies toevoegen](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
3. Selecteer **volgende** totdat u de **serverfuncties** sectie.
4. In **functies**, selecteer **Failoverclustering**.
5. Voeg eventuele aanvullende vereiste functies.
6. Klik op **installeren** om toe te voegen van de functies.

Herhaal de stappen op de andere SQL Server-VM.

## <a name="a-nameendpoint-firewall-configure-the-firewall-on-each-sql-server-vm"></a><a name="endpoint-firewall">De firewall configureren op elke SQL Server-VM

De oplossing is vereist voor de volgende TCP-poorten open zijn in de firewall:

- **SQL Server-machine**:<br/>
   Poort 1433 voor een standaardexemplaar van SQL Server.
- **Azure load balancer-test:**<br/>
   Een beschikbare poort. De voorbeelden gebruiken vaak 59999.
- **Eindpunt voor databasespiegeling:** <br/>
   Een beschikbare poort. De voorbeelden gebruiken vaak 5022.

De firewall-poorten moeten geopend zijn op beide VM's van SQL Server.

De methode de poorten te openen, is afhankelijk van de firewalloplossing die u gebruikt. De volgende sectie wordt uitgelegd hoe de poorten openen in Windows Firewall. Open de vereiste poorten op elk van uw SQL Server-VM.

### <a name="open-a-tcp-port-in-the-firewall"></a>Open een TCP-poort in de firewall

1. Op de eerste SQL-Server **Start** scherm, starten **Windows Firewall met geavanceerde beveiliging**.
2. Selecteer in het linkerdeelvenster **regels voor binnenkomende verbindingen**. Klik in het rechterdeelvenster op **nieuwe regel**.
3. Voor **regeltype**, kies **poort**.
4. Geef voor de poort **TCP** en typt u de juiste poortnummers. Zie het volgende voorbeeld:

   ![SQL-firewall](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/35-tcpports.png)

5. Klik op **Volgende**.
6. Op de **actie** pagina, houden **de verbinding toestaan** geselecteerd en klik vervolgens op **volgende**.
7. Op de **profiel** pagina, accepteer de standaardinstellingen en klik vervolgens op **volgende**.
8. Op de **naam** pagina, geeft u de regelnaam van een (zoals **Azure LB Probe**) in de **naam** in het tekstvak en klik vervolgens op **voltooien**.

Herhaal deze stappen op de tweede SQL Server-VM.

## <a name="next-steps"></a>Volgende stappen

* [Een SQL Server Always On-beschikbaarheidsgroep maken op virtuele machines in Azure](virtual-machines-windows-portal-sql-availability-group-tutorial.md)
