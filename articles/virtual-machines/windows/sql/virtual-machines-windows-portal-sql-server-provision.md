---
title: Inrichting handleiding voor SQL Server-VM's van Windows in de Azure portal | Microsoft Docs
description: Deze instructies handleiding beschrijft uw opties voor het maken van Windows SQL Server 2017 virtuele machines in de Azure-portal.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
tags: azure-resource-manager
ms.assetid: 1aff691f-a40a-4de2-b6a0-def1384e086e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 02/15/2018
ms.author: jroth
ms.openlocfilehash: 7e55227773820af26435100b11dfefce058e7e78
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="how-to-provision-a-windows-sql-server-virtual-machine-in-the-azure-portal"></a>Het inrichten van een virtuele machine van Windows SQL Server in de Azure portal

Deze handleiding bevat informatie over de verschillende opties beschikbaar wanneer u een virtuele machine van Windows SQL Server in de Azure-portal maakt. In dit artikel bevat informatie over meer configuratieopties dan de [Quick Start SQL Server VM](quickstart-sql-vm-create-portal.md), die meer via een mogelijk inrichting taak gaat. 

Gebruik deze handleiding voor het maken van uw eigen SQL Server-VM. Of gebruik het als referentie voor de beschikbare opties in de Azure portal.

> [!TIP]
> Als u vragen over virtuele machines van SQL Server hebt, raadpleegt u de [Frequently Asked Questions](virtual-machines-windows-sql-server-iaas-faq.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a id="select"></a> SQL Server-installatiekopieën voor virtuele machine-galerie

Wanneer u een virtuele machine van SQL Server maakt, kunt u een van de verschillende vooraf geconfigureerde installatiekopieën uit de galerie met virtuele machine. De volgende stappen laten zien hoe u een van de installatiekopieën van het SQL Server 2017 selecteren.

1. Meld u met uw account aan bij de [Azure Portal](https://portal.azure.com).

1. Klik in Azure Portal op **Een resource maken**. In de portal wordt het venster **Nieuw** geopend.

1. Klik in het venster **Nieuw** op **Berekenen** en klik vervolgens op **Alles weergeven**.

   ![Het venster Nieuwe berekening](./media/virtual-machines-windows-portal-sql-server-provision/azure-new-compute-blade.png)

1. Typ in het zoekveld **SQL Server 2017** en druk op ENTER.

1. Klik vervolgens op het pictogram met het **filter**.

1. Schakel in het venster Filter de subcategorie **Op Windows gebaseerd** en de uitgever **Microsoft** in. Klik op **Gereed** om de resultaten te filteren op door Microsoft gepubliceerde Windows SQL Server-installatiekopieën.

   ![Het venster Virtuele machines in Azure](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade2.png)

1. Bekijk de beschikbare SQL Server-installatiekopieën. Elke installatiekopie correspondeert met een bepaalde SQL Server-versie en een bepaald besturingssysteem.

1. Selecteer de installatiekopie met de naam **Gratis licentie voor SQL Server: SQL Server 2017 Developer op Windows Server 2016**.

   > [!TIP]
   > De ontwikkelaarsversie wordt gebruikt in dit scenario, omdat het een complete, gratis editie van SQL Server voor het testen van ontwikkeling. U betaalt alleen voor de kosten van het uitvoeren van de virtuele machine. U bent echter gratis een van de afbeeldingen die u wilt gebruiken in dit scenario kiezen. Zie voor een beschrijving van de beschikbare installatiekopieën, de [Windows virtuele Machines van SQL Server-overzicht](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo).

   > [!TIP]
   > Licentiekosten voor SQL Server zijn opgenomen in de prijzen voor de per minuut van de virtuele machine die u maakt en varieert met editie en kernen. De ontwikkelaarsversie van SQL Server is gratis voor ontwikkeling/tests (geen productie) en SQL Express is gratis voor lichte werkbelasting (minder dan 1 GB geheugen, kleiner is dan 10 GB aan opslagruimte). U kunt ook bring-your-eigenaar-license (BYOL) en betalen alleen voor de virtuele machine. De namen van de installatiekopieën worden voorafgegaan door {BYOL}. 
   >
   > Zie [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prijsrichtlijnen voor SQL Server Azure VM's) voor meer informatie over deze opties.

1. Controleer onder **Een implementatiemodel selecteren** of **Resource Manager** is geselecteerd. Resource Manager is het aanbevolen implementatiemodel voor nieuwe virtuele machines. 

1. Klik op **Create**.

    ![Virtuele SQL-machines maken met Resource Manager](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a id="configure"></a> Configuratie-opties
Er zijn vijf vensters voor het configureren van een virtuele SQL Server-machine.

| Stap | Beschrijving |
| --- | --- |
| **Basisinstellingen** |[Basisinstellingen configureren](#1-configure-basic-settings) |
| **Grootte** |[De grootte van de virtuele machine kiezen](#2-choose-virtual-machine-size) |
| **Instellingen** |[Optionele kenmerken configureren](#3-configure-optional-features) |
| **SQL Server-instellingen** |[SQL Server-instellingen configureren](#4-configure-sql-server-settings) |
| **Samenvatting** |[De samenvatting bekijken](#5-review-the-summary) |

## <a name="1-configure-basic-settings"></a>1. Basisinstellingen configureren

Geef op de pagina **Basisbeginselen** de volgende gegevens op:

* Voer een unieke **naam** in voor de virtuele machine.

* Selecteer **SSD** als schijftype voor de virtuele machine voor optimale prestaties.

* Geef een **gebruikersnaam** op voor het lokale beheerdersaccount op de virtuele machine. Dit account wordt ook toegevoegd aan de vaste serverrol **sysadmin** van de SQL Server.

* Geef een sterk **wachtwoord** op.

* Als u meerdere abonnementen hebt, controleert u of het juiste abonnement is gekoppeld aan de nieuwe virtuele machine.

* Typ in het vak **Resourcegroep** een naam voor een nieuwe resourcegroep. U kunt ook een bestaande resourcegroep gebruiken door op **Bestaande gebruiken** te klikken. Een resourcegroep is een verzameling verwante resources in Azure (virtuele machines, opslagaccounts, virtuele netwerken enz.).

  > [!NOTE]
  > Het is een goed idee om een nieuwe resourcegroep te maken als u het gebruik van SQL Server in Azure alleen wilt testen of hier meer over te weten wilt komen. Als u klaar bent met testen, verwijdert u gewoon de resourcegroep. De virtuele machine en alle resources die aan de resourcegroep zijn gekoppeld, worden dan automatisch verwijderd. Zie voor meer informatie over resourcegroepen [Overzicht van Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md).

* Selecteer een **locatie** voor de Azure-regio als host voor deze implementatie.

* Klik op **OK** om de instellingen op te slaan.

    ![Venster SQL-basisbeginselen](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. De grootte van de virtuele machine kiezen

In de stap **Grootte** kiest u de grootte van uw virtuele machine. Dit doet u in het venster **Een grootte kiezen**. Het venster bevat in eerste instantie aanbevolen grootten voor de machine op basis van de geselecteerde installatiekopie.

> [!IMPORTANT]
> De geschatte maandelijkse kosten die worden weergegeven in het venster **Grootte kiezen**, zijn niet inclusief de kosten voor SQL Server-licentieverlening. Deze schatting is de kosten van de virtuele machine alleen. Deze schatting is voor de snelle en Developer-edities van SQL Server, de totale geschatte kosten. Zie de [pagina met prijzen voor virtuele Windows-machines](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) en selecteer de gewenste editie van SQL server. Zie ook [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prijsrichtlijnen voor SQL Server Azure VM's).

![Opties voor de grootte van uw virtuele SQL-machine](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Zie [Performance best practices for SQL Server in Azure Virtual Machines (Best practices voor optimale prestaties van SQL Server in Azure Virtual Machines)](virtual-machines-windows-sql-performance.md) voor de aanbevolen machinegrootten en configuratie voor productieworkloads. Als u een machinegrootte nodig hebt die niet wordt vermeld, klikt u op de knop **Alles weergeven**.

> [!NOTE]
> Zie [Grootten van virtuele machines](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor meer informatie over de grootten van virtuele machines.

Kies de grootte van uw machine en klik vervolgens op **Selecteren**.

## <a name="3-configure-optional-features"></a>3. Optionele kenmerken configureren

Configureer in het venster **Instellingen** de Azure-opslag, -netwerken en -bewaking voor de virtuele machine.

* Onder **Opslag** selecteert u **Ja** onder **Managed Disks**.

   > [!NOTE]
   > Microsoft raadt Managed Disks aan voor SQL Server. Managed Disks verwerken de opslag achter de schermen. Bovendien distribueert Azure de opslagresources zodat voldoende redundantie wordt geboden wanneer virtuele machines met Managed Disks zich in dezelfde beschikbaarheidsset bevinden. Voor meer informatie Zie [Azure beheerd schijven Overview] [... -beheerde schijven overview.md.). Zie [Managed Disks gebruiken schijven voor virtuele machines in een beschikbaarheidsset](../manage-availability.md) voor meer informatie over Managed Disks in een beschikbaarheidsset.

* Onder **Netwerk** kunt u de automatisch ingevulde waarden accepteren. U kunt ook op elke functie klikken om het **Virtuele netwerk**, het **Subnet**, het **Openbaar IP-adres** en de **Netwerkbeveiligingsgroep** handmatig te configureren. Behoud de standaardwaarden voor de doeleinden van deze rondleiding.

* In Azure wordt **Bewaking** standaard ingeschakeld met hetzelfde opslagaccount als dat van de virtuele machine. U kunt deze instellingen hier wijzigen.

* Onder **beschikbaarheidsset**, kunt u de standaardwaarde van laten **geen** voor dit scenario. Als u van plan bent de SQL AlwaysOn-beschikbaarheidsgroepen in te stellen, moet u de beschikbaarheid configureren om te voorkomen dat de virtuele machine opnieuw wordt gemaakt.  Zie voor meer informatie [De beschikbaarheid van virtuele machines beheren](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Klik op **OK** wanneer u klaar bent met het configureren van deze instellingen.

## <a name="4-configure-sql-server-settings"></a>4. SQL Server-instellingen configureren
Configureer in het venster **SQL Server-instellingen** specifieke instellingen en optimalisaties voor SQL Server. U kunt onder meer de volgende instellingen voor SQL Server configureren:

| Instelling |
| --- |
| [Connectiviteit](#connectivity) |
| [Verificatie](#authentication) |
| [Opslagconfiguratie](#storage-configuration) |
| [Automatisch patch toepassen](#automated-patching) |
| [Automatische back-up](#automated-backup) |
| [Integratie van Azure Key Vault](#azure-key-vault-integration) |
| [SQL Server Machine Learning-services](#sql-server-machine-learning-services) |

### <a name="connectivity"></a>Connectiviteit

Geef onder **SQL-connectiviteit** het gewenste type toegang tot het SQL Server-exemplaar op deze virtuele machine op. Selecteer voor de doeleinden van deze rondleiding **openbaar (internet)** verbindingen naar SQL Server van de machines of services op internet toestaan. Als deze optie is geselecteerd, configureert Azure automatisch de firewall en de netwerkbeveiligingsgroep voor verkeer op poort 1433.

![SQL-connectiviteitsopties](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

> [!TIP]
> Standaard gebruikt SQL Server een bekende poort, namelijk **1433**. Wijzig de poort in het vorige dialoogvenster om een andere poort, zoals 1401, te gebruiken voor betere beveiliging. Als u de poort wijzigt, moet u verbinding maken met behulp van deze poort van elke client-hulpprogramma's, zoals SSMS.

Voor verbinding met SQL Server via internet moet u ook SQL Server-verificatie inschakelen. Dit wordt beschreven in het volgende gedeelte.

Als u de mogelijkheid om via internet verbinding te maken met de database-engine liever niet inschakelt, kiest u een van de volgende opties:

* **Lokaal (alleen binnen VM)** om alleen verbindingen met SQL Server vanuit de virtuele machine toe te staan.
* **Privé (binnen virtueel netwerk)** om verbindingen met SQL Server toe te staan vanaf machines of services in hetzelfde virtuele netwerk.

In het algemeen kunt u de beveiliging verbeteren door te kiezen voor de meest beperkende connectiviteit die voor uw scenario mogelijk is. Alle opties kunnen echter worden beveiligd via regels van de netwerkbeveiligingsgroep en SQL/Windows-verificatie. U kunt de netwerkbeveiligingsgroep bewerken nadat de virtuele machine is gemaakt. Zie [Veiligheidsoverwegingen voor SQL Server in virtuele Azure-machines](virtual-machines-windows-sql-security.md) voor meer informatie.

### <a name="authentication"></a>Verificatie

Als u SQL Server-verificatie vereist, klikt u onder **SQL-verificatie** op **Inschakelen**.

![SQL Server-verificatie](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

> [!NOTE]
> Als u van plan bent voor toegang tot SQL Server via internet (de optie voor het openbare-connectiviteit), moet u hier SQL-verificatie inschakelen. Voor openbare toegang tot de SQL Server is het gebruik van SQL-verificatie vereist.

Als u SQL Server-verificatie inschakelt, geeft u een **Aanmeldingsnaam** en een **Wachtwoord** op. Deze gebruikersnaam is geconfigureerd als aanmeldingsnaam voor SQL Server-verificatie en lid van de vaste serverrol **sysadmin**. Zie [Een verificatiemodus kiezen](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode) voor meer informatie over de verificatiemodi.

Als u SQL Server-verificatie niet inschakelt, kunt u het lokale beheerdersaccount op de virtuele machine gebruiken om verbinding te maken met het SQL Server-exemplaar.

### <a name="storage-configuration"></a>Opslagconfiguratie

Klik op **Opslagconfiguratie** om de opslagvereisten op te geven.

![SQL-opslagconfiguratie](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

> [!NOTE]
> Als u uw virtuele machine handmatig hebt geconfigureerd om standaardopslag te gebruiken, is deze optie niet beschikbaar. Automatische opslagoptimalisatie is alleen beschikbaar voor Premium Storage.

> [!TIP]
> Het aantal stops en de bovenste limieten van de schuifregelaars zijn afhankelijk van de grootte van de virtuele machine die u hebt geselecteerd. Een grotere en krachtigere virtuele machine kan verder opschalen.

U kunt vereisten opgeven als i/o-bewerkingen per seconde (IOP's), als doorvoer in MB/s en als totale grootte van de opslagruimte. Configureer deze waarden met behulp van de schuifregelaar. U kunt deze opslaginstellingen wijzigen op basis van de workload. De portal berekent automatisch het aantal schijven dat moet worden gekoppeld en geconfigureerd op basis van deze vereisten.

Selecteer onder **Opslag geoptimaliseerd voor** een van de volgende opties:

* **Algemeen** is de standaardinstelling en ondersteunt de meeste workloads.
* Met **Transactionele** verwerking wordt de opslag voor OLTP-workloads van traditionele databases geoptimaliseerd.
* **Gegevensopslag** optimaliseert de opslag voor analyse- en rapportageworkloads.

### <a name="automated-patching"></a>Automatisch patchen

**Automatisch patchen** is standaard ingeschakeld. Met automatisch patchen kan Azure automatisch een patch uitvoeren voor SQL Server en het besturingssysteem. Geef een dag van de week, een tijd en een periode op voor een onderhoudssessie. Azure voert de patch uit tijdens deze onderhoudssessie. Het onderhoudsschema maakt voor de tijd gebruik van de landinstellingen van de virtuele machine. Als u niet wilt dat Azure een automatische patch uitvoert voor SQL Server en het besturingssysteem, klikt u op **Uitschakelen**.  

![Automatisch patchen van SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Zie voor meer informatie [Automatisch patchen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automatische back-up

Schakel automatische databaseback-ups in voor alle databases onder **Automatische back-up**. Automatische back-up is standaard uitgeschakeld.

Als u automatische back-up van SQL inschakelt, kunt u de volgende instellingen configureren:

* De retentieperiode (dagen) voor back-ups
* Het opslagaccount dat voor back-ups moet worden gebruikt
* Versleutelingsoptie en wachtwoord voor back-ups
* Back-up maken van systeemdatabases
* Back-upschema configureren

Voor het versleutelen van de back-up klikt u op **Inschakelen**. Geef het **Wachtwoord** op. Azure maakt een certificaat voor het versleutelen van de back-ups en gebruikt het opgegeven wachtwoord om dit certificaat te beschermen.

![Automatische back-up van SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup2.png)

 Zie voor meer informatie [Automatische back-up voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Integratie van Azure Sleutelkluis

Voor het opslaan van beveiligingsgeheimen in Azure voor versleuteling klikt u op **Integratie van Azure Sleutelkluis**. Klik vervolgens op **Inschakelen**.

![Integratie van Azure Sleutelkluis in SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

De volgende tabel bevat de vereiste parameters voor het configureren van de Azure Sleutelkluis-integratie.

| PARAMETER | BESCHRIJVING | VOORBEELD |
| --- | --- | --- |
| **Key Vault-URL** |De locatie van de sleutelkluis. |https://contosokeyvault.vault.azure.net/ |
| **Principal-naam** |Principal-naam voor de Azure Active Directory-service. Deze naam wordt ook wel aangeduid als de Client-ID. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Principal-geheim** |Principal-geheim voor de Azure Active Directory-service. Dit geheim wordt ook wel aangeduid als het Clientgeheim. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Referentienaam** |**Referentienaam**: de Azure Sleutelkluis-integratie maakt een referentie in SQL Server, zodat de virtuele machine toegang heeft tot de sleutelkluis. Kies een naam voor deze referentie. |mycred1 |

Zie voor meer informatie [Integratie van Azure Sleutelkluis configureren voor SQL Server op Azure-VM's](virtual-machines-windows-ps-sql-keyvault.md).

### <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning-services

U kunt gebruikmaken van de optie om [SQL Server Machine Learning-services](https://msdn.microsoft.com/library/mt604845.aspx) in te schakelen. Deze optie kunt u geavanceerde analyses met SQL Server 2017 gebruiken. Klik op **Inschakelen** in het venster **SQL Server-instellingen**.

![SQL Server Machine Learning-services inschakelen](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

Wanneer u klaar bent met het configureren van de SQL Server-instellingen, klikt u op **OK**.

## <a name="5-review-the-summary"></a>5. De samenvatting bekijken

Bekijk in het venster **Samenvatting** de samenvatting en klik op **Kopen** om de SQL Server, de resourcegroep en de resources te maken die zijn opgegeven voor deze VM.

U kunt de implementatie bewaken vanuit Azure Portal. Met de knop **Meldingen** boven aan het scherm kunt u de algemene status van de implementatie weergeven.

> [!NOTE]
> Om u een idee te geven van de implementatietijden, heb ik een virtuele SQL-machine voor de regio VS - oost geïmplementeerd met standaardinstellingen. Deze testimplementatie nam in totaal ongeveer 12 minuten in beslag. Afhankelijk van uw regio en de geselecteerde instellingen bent u mogelijk meer of minder tijd kwijt aan de implementatie.

## <a id="remotedesktop"></a> De virtuele machine openen via Extern bureaublad

Doorloop de volgende stappen om via Extern bureaublad verbinding te maken met de virtuele SQL Server-machine:

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Nadat u verbinding hebt gemaakt met de virtuele SQL Server-machine, kunt u SQL Server Management Studio starten en verbinding maken met Windows-verificatie met behulp van de lokale beheerdersreferenties. Als u SQL Server-verificatie inschakelt, kunt u ook verbinding maken met SQL-verificatie. Dit kan met behulp van de SQL-aanmeldingsnaam en het wachtwoord die u hebt geconfigureerd tijdens het inrichten.

Met toegang tot de machine kunt u rechtstreeks de instellingen voor de machine en de SQL Server wijzigen op basis van uw vereisten. U kunt bijvoorbeeld de firewallinstellingen configureren of de SQL Server-configuratie-instellingen wijzigen.

## <a id="connect"></a> Op afstand verbinding maken met SQL Server

In dit scenario hebt geselecteerd **openbare** toegang voor de virtuele machine en **SQL Server-verificatie**. Door deze instellingen is de virtuele machine automatisch geconfigureerd voor het toestaan van SQL Server-verbindingen vanaf elke client via internet (ervan uitgaande dat deze beschikken over de juiste SQL-aanmeldgegevens).

> [!NOTE]
> Als u tijdens het inrichten niet de optie Openbaar hebt geselecteerd, kunt u de instellingen voor SQL-verbindingen achteraf wijzigen via de portal. Zie [Change your SQL connectivity settings](virtual-machines-windows-sql-connect.md#change) (SQL-verbindingsinstellingen wijzigen) voor meer informatie.

De volgende secties laten zien hoe verbinding maken via internet aan uw virtuele machine van SQL Server-exemplaar.

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van SQL Server in Azure [SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) en de [Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md).