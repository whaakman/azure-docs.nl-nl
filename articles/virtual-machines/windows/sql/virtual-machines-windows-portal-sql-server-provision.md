---
title: Inrichting-handleiding voor Windows SQL Server-VM's in Azure portal | Microsoft Docs
description: In deze gebruiksaanwijzing wordt beschreven hoe u voor het maken van Windows SQL Server 2017 virtuele machines in Azure portal.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: 1aff691f-a40a-4de2-b6a0-def1384e086e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 05/04/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 6723adb3fb8987a127eee419c9ac188c7a33d50b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076046"
---
# <a name="how-to-provision-a-windows-sql-server-virtual-machine-in-the-azure-portal"></a>Hoe u een Windows SQL Server-machine inrichten in Azure portal

Deze handleiding bevat informatie over de verschillende opties beschikbaar wanneer u een Windows SQL Server-machine in Azure portal maakt. Dit artikel vindt u meer configuratieopties instellen dan de [SQL Server-VM-snelstartgids](quickstart-sql-vm-create-portal.md), die meer via één mogelijk inrichting taak gaat. 

Deze handleiding gebruiken om uw eigen SQL Server-VM maken. Of gebruik het als referentie voor de beschikbare opties in Azure portal.

> [!TIP]
> Als u vragen hebt over virtuele machines met SQL Server, raadpleegt u [Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a id="select"></a> Galerie met installatiekopieën van SQL Server virtuele machines

Als u een SQL Server-machine maakt, selecteert u een van verschillende vooraf geconfigureerde installatiekopieën uit de galerie met virtuele machines. De volgende stappen laten zien hoe u een van de SQL Server 2017-installatiekopieën selecteren.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met uw account.

1. Klik in Azure Portal op **Een resource maken**. In de portal wordt het venster **Nieuw** geopend.

1. Klik in het venster **Nieuw** op **Berekenen** en klik vervolgens op **Alles weergeven**.

1. Typ in het zoekveld **SQL Server 2017** en druk op ENTER.

1. Selecteer in de vervolgkeuzelijsten filter _Windows Server 2016_ voor de **besturingssysteem** en selecteer _Microsoft_ als de **Publisher**. 

     ![Het venster Nieuwe berekening](./media/virtual-machines-windows-portal-sql-server-provision/azure-new-compute-blade.png)

1. Bekijk de beschikbare SQL Server-installatiekopieën. Elke installatiekopie correspondeert met een bepaalde SQL Server-versie en een bepaald besturingssysteem.

1. Selecteer de installatiekopie met de naam **gratis licentie voor SQL Server: SQL Server 2017 Developer op WindowsServer 2016**.

   > [!TIP]
   > De Developer edition wordt gebruikt in dit scenario, omdat het is een volledig functionele, gratis editie van SQL Server voor het testen van ontwikkeling. U betaalt alleen voor de kosten van het uitvoeren van de virtuele machine. Echter, bent u een van de afbeeldingen te gebruiken in dit scenario kiezen. Zie voor een beschrijving van de beschikbare installatiekopieën, de [overzicht van SQL Server Windows Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo).

   > [!TIP]
   > Licentiekosten voor SQL Server zijn opgenomen in de prijzen per seconde van de virtuele machine die u maakt en varieert met edition en kernen. Editie van SQL Server Developer is gratis voor ontwikkeling/testen (niet voor productiedoeleinden) en SQL Express is gratis voor lichte werkbelasting (minder dan 1 GB geheugen, minder dan 10 GB aan opslagruimte). U kunt ook bring-your-own-license (BYOL) en betaal alleen voor de virtuele machine. De namen van de installatiekopieën worden voorafgegaan door {BYOL}. 
   >
   > Zie [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prijsrichtlijnen voor SQL Server Azure VM's) voor meer informatie over deze opties.

1. Controleer onder **Een implementatiemodel selecteren** of **Resource Manager** is geselecteerd. Resource Manager is het aanbevolen implementatiemodel voor nieuwe virtuele machines. 

1. Selecteer **Maken**.


## <a id="configure"></a> Configuratie-opties

Er zijn meerdere tabbladen voor het configureren van een SQL Server-machine. Ten behoeve van deze handleiding richten we ons op de volgende: 

| Stap | Description |
| --- | --- |
| **Basisinstellingen** |[Basisinstellingen configureren](#1-configure-basic-settings) |
| **Optionele functies** |[Optionele kenmerken configureren](#2-configure-optional-features) |
| **SQL Server-instellingen** |[SQL Server-instellingen configureren](#3-configure-sql-server-settings) |
| **Beoordelen en maken** | [De samenvatting bekijken](#4-review--create) |

## <a name="1-configure-basic-settings"></a>1. Basisinstellingen configureren


Op de **basisbeginselen** tabblad, geef de volgende informatie:

* Onder **projectdetails**, zorg ervoor dat het juiste abonnement is geselecteerd. 
*  In de **resourcegroep** sectie, selecteer een bestaande resource groep uit de lijst of kies **nieuw** om een nieuwe resourcegroep te maken. Een resourcegroep is een verzameling verwante resources in Azure (virtuele machines, opslagaccounts, virtuele netwerken enz.). 

    ![Abonnement](media/quickstart-sql-vm-create-portal/basics-project-details.png)

  > [!NOTE]
  > Het is een goed idee om een nieuwe resourcegroep te maken als u het gebruik van SQL Server in Azure alleen wilt testen of hier meer over te weten wilt komen. Als u klaar bent met testen, verwijdert u gewoon de resourcegroep. De virtuele machine en alle resources die aan de resourcegroep zijn gekoppeld, worden dan automatisch verwijderd. Zie voor meer informatie over resourcegroepen [Overzicht van Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md).


* Onder **exemplaar details**:
    1. Voer een unieke **virtuele-machinenaam**.  
    1. Kies een locatie voor uw **regio**. 
    1. Ten behoeve van deze handleiding laat **Beschikbaarheidsopties** ingesteld op _geen infrastructuur redundantie vereist_. Zie voor meer informatie over opties voor beschikbaarheid, [Azure-regio's en beschikbaarheid](../../windows/regions-and-availability.md). 
    1. In de **installatiekopie** in de lijst met _gratis licentie voor SQL Server: SQL Server 2017 Developer op WindowsServer 2016_.  
    1. Ervoor kiezen om te **grootte wijzigen** voor de **grootte** van de virtuele machine en selecteer de **A2 Basic** bieden. Zorg ervoor dat uw resources opschonen wanneer u klaar bent met eventuele onverwachte kosten voorkomen. Zie [Performance best practices for SQL Server in Azure Virtual Machines (Best practices voor optimale prestaties van SQL Server in Azure Virtual Machines)](virtual-machines-windows-sql-performance.md) voor de aanbevolen machinegrootten en configuratie voor productieworkloads.

    ![Exemplaardetails](media/quickstart-sql-vm-create-portal/basics-instance-details.png)

> [!IMPORTANT]
> De geschatte maandelijkse kosten die worden weergegeven in het venster **Grootte kiezen**, zijn niet inclusief de kosten voor SQL Server-licentieverlening. Deze schatting is de kosten van de virtuele machine alleen. Deze schatting is voor de Express- en Developer-edities van SQL Server, de totale geschatte kosten. Zie de [pagina met prijzen voor virtuele Windows-machines](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) en selecteer de gewenste editie van SQL server. Zie ook de [prijsinformatie voor SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) en [grootten voor virtuele machines](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Onder **Administrator-account**, Geef een gebruikersnaam en een wachtwoord. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../../windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

   ![Administrator-account](media/quickstart-sql-vm-create-portal/basics-administrator-account.png)

* Onder **regels voor binnenkomende poort**, kiest u **geselecteerde poorten toestaat** en selecteer vervolgens **RDP (3389)** in de vervolgkeuzelijst. 

   ![Regels voor binnenkomende poort](media/quickstart-sql-vm-create-portal/basics-inbound-port-rules.png)


## <a name="2-configure-optional-features"></a>2. Optionele kenmerken configureren

### <a name="disks"></a>Disks

Op de **schijven** tabblad, configureer dan uw schijfopties. 

* Onder **OS schijftype**, selecteer het type van de schijf die u wilt gebruiken voor uw besturingssysteem in de vervolgkeuzelijst. Premium wordt aanbevolen voor productiesystemen, maar is niet beschikbaar voor een basis-VM. Als u wilt gebruikmaken van Premium-SSD, de grootte van de virtuele machine te wijzigen. 
* Onder **Geavanceerd**, selecteer **Ja** onder Gebruik **Managed Disks**.

   > [!NOTE]
   > Microsoft raadt Managed Disks aan voor SQL Server. Managed Disks verwerken de opslag achter de schermen. Bovendien distribueert Azure de opslagresources zodat voldoende redundantie wordt geboden wanneer virtuele machines met Managed Disks zich in dezelfde beschikbaarheidsset bevinden. Voor meer informatie, Zie [Azure Managed Disks Overview] [.. / beheerde schijven overview.md.). Zie voor meer informatie over beheerde schijven in een beschikbaarheidsset [beheerde schijven gebruiken voor virtuele machines in de beschikbaarheidsset] (.. /Manage-Availability.MD.

![Instellingen voor SQL-VM-schijf](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-disks.png)
  
  
### <a name="networking"></a>Netwerken

Op de **netwerken** tabblad, configureer dan uw netwerkopties. 

* Maak een nieuwe **virtueel netwerk**, of een bestaand vNet gebruiken voor uw SQL Server-VM. Wijst een **Subnet** ook. 

* Onder **NIC beveiligingsgroep**, selecteert u een groep basisvereisten voor beveiliging of de groep geavanceerde beveiliging. De optie te kiezen, kunt u selecteren van poorten voor inkomend verkeer voor de SQL Server-VM (dezelfde waarden die zijn geconfigureerd op de **Basic** tabblad). Als u de geavanceerde optie selecteert, kunt u een bestaande netwerkbeveiligingsgroep kiezen of een nieuwe maken. 

* U kunt andere wijzigingen aanbrengen in netwerkinstellingen, of behoud de standaardwaarden.

![SQL VM-netwerkinstellingen](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-networking.png)

#### <a name="monitoring"></a>Bewaking

Op de **bewaking** tabblad, configureer dan bewakings- en autoshutdown. 

* Met Azure kunnen **opstarten bewaking** aangewezen standaard met hetzelfde opslagaccount voor de virtuele machine. U kunt deze instellingen hier, evenals inschakelen wijzigen **besturingssysteem Gast diagnostics**. 
* U kunt inschakelen **door het systeem toegewezen beheerde identiteit** en **autoshutdown** op dit tabblad ook. 

![Instellingen voor SQL-VM](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-management.png)


## <a name="3-configure-sql-server-settings"></a>3. SQL Server-instellingen configureren

Op de **SQL Server-instellingen** tabblad, configureer dan specifieke instellingen en optimalisaties voor SQL Server. De instellingen die u voor SQL Server configureren kunt omvatten het volgende:



| Instelling |
| --- |
| [Connectiviteit](#connectivity) |
| [Verificatie](#authentication) |
| [Integratie van Azure Key Vault](#azure-key-vault-integration) |
| [Opslagconfiguratie](#storage-configuration) |
| [Automatisch patch toepassen](#automated-patching) |
| [Automatische back-up](#automated-backup) |
| [R Services (Advanced Analytics)](#r-services-advanced-analytics) |


### <a name="connectivity"></a>Connectiviteit

Geef onder **SQL-connectiviteit** het gewenste type toegang tot het SQL Server-exemplaar op deze virtuele machine op. Selecteer voor het doel van dit scenario, **openbaar (internet)** verbindingen met SQL Server vanaf machines of services op internet is toegestaan. Met deze optie is geselecteerd, configureert Azure automatisch de firewall en de netwerkbeveiligingsgroep voor verkeer op de geselecteerde poort.

> [!TIP]
> Standaard gebruikt SQL Server een bekende poort, namelijk **1433**. Wijzig de poort in het vorige dialoogvenster om een andere poort, zoals 1401, te gebruiken voor betere beveiliging. Als u de poort wijzigt, moet u verbinding maken met behulp van deze poort vanaf elk clienthulpprogramma, zoals SSMS.

![SQL VM-beveiliging](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-security.png)

Voor verbinding met SQL Server via internet moet u ook SQL Server-verificatie inschakelen. Dit wordt beschreven in het volgende gedeelte.

Als u de mogelijkheid om via internet verbinding te maken met de database-engine liever niet inschakelt, kiest u een van de volgende opties:

* **Lokaal (alleen binnen VM)** om alleen verbindingen met SQL Server vanuit de virtuele machine toe te staan.
* **Privé (binnen virtueel netwerk)** om verbindingen met SQL Server toe te staan vanaf machines of services in hetzelfde virtuele netwerk.

In het algemeen kunt u de beveiliging verbeteren door te kiezen voor de meest beperkende connectiviteit die voor uw scenario mogelijk is. Alle opties kunnen echter worden beveiligd via regels van de netwerkbeveiligingsgroep en SQL/Windows-verificatie. U kunt de netwerkbeveiligingsgroep bewerken nadat de virtuele machine is gemaakt. Zie [Veiligheidsoverwegingen voor SQL Server in virtuele Azure-machines](virtual-machines-windows-sql-security.md) voor meer informatie.



### <a name="authentication"></a>Verificatie

Als u SQL Server-verificatie vereist, klikt u op **inschakelen** onder **SQL-verificatie** op de **SQL Server-instellingen** tabblad.

![SQL Server-verificatie](./media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-authentication.png)

> [!NOTE]
> Als u van plan bent voor toegang tot SQL Server via internet (de optie voor het openbare verbinding), moet u hier SQL-verificatie inschakelen. Voor openbare toegang tot de SQL Server is het gebruik van SQL-verificatie vereist.

Als u SQL Server-verificatie inschakelt, geeft u een **Aanmeldingsnaam** en een **Wachtwoord** op. Deze aanmeldingsnaam is geconfigureerd als een aanmelding voor SQL Server-verificatie en lid van de **sysadmin** vaste serverrol. Zie [Een verificatiemodus kiezen](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode) voor meer informatie over de verificatiemodi.

Als u SQL Server-verificatie niet inschakelt, kunt u het lokale beheerdersaccount op de virtuele machine gebruiken om verbinding te maken met het SQL Server-exemplaar.


### <a name="azure-key-vault-integration"></a>Integratie van Azure Sleutelkluis

Voor het opslaan van beveiligingsgeheimen in Azure voor versleuteling, selecteer **SQL Server-instellingen**, en schuif omlaag naar **Azure key vault-integratie**. Selecteer **inschakelen** en vul de aangevraagde gegevens. 

![Integratie van Azure Sleutelkluis](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-akv.png)

De volgende tabel bevat de vereiste parameters voor het configureren van de Azure Sleutelkluis-integratie.

| PARAMETER | BESCHRIJVING | VOORBEELD |
| --- | --- | --- |
| **Key Vault-URL** |De locatie van de sleutelkluis. |https:\//contosokeyvault.vault.azure.net/ |
| **Principal-naam** |Principal-naam voor de Azure Active Directory-service. Deze naam wordt ook wel aangeduid als de Client-ID. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Principal-geheim** |Principal-geheim voor de Azure Active Directory-service. Dit geheim wordt ook wel aangeduid als het Clientgeheim. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Referentienaam** |**Referentienaam**: Azure Sleutelkluis-integratie maakt u een referentie binnen SQL Server, zodat de virtuele machine toegang heeft tot de key vault. Kies een naam voor deze referentie. |mycred1 |

Zie voor meer informatie [Integratie van Azure Sleutelkluis configureren voor SQL Server op Azure-VM's](virtual-machines-windows-ps-sql-keyvault.md).

### <a name="storage-configuration"></a>Opslagconfiguratie

Op de **SQL Server-instellingen** tabblad onder **opslagconfiguratie**, selecteer **-configuratie wijzigen** om op te geven van de opslagvereisten.


> [!NOTE]
> Als u uw virtuele machine handmatig hebt geconfigureerd om standaardopslag te gebruiken, is deze optie niet beschikbaar. Automatische opslagoptimalisatie is alleen beschikbaar voor Premium Storage.

> [!TIP]
> Het aantal stops en de bovenste limieten van de schuifregelaars zijn afhankelijk van de grootte van de virtuele machine die u hebt geselecteerd. Een grotere en krachtigere virtuele machine kan verder opschalen.

U kunt vereisten opgeven als i/o-bewerkingen per seconde (IOP's), als doorvoer in MB/s en als totale grootte van de opslagruimte. Configureer deze waarden met behulp van de schuifregelaar. U kunt deze opslaginstellingen wijzigen op basis van de workload. De portal berekent automatisch het aantal schijven dat moet worden gekoppeld en geconfigureerd op basis van deze vereisten.

Selecteer onder **Opslag geoptimaliseerd voor** een van de volgende opties:

* **Algemeen** is de standaardinstelling en ondersteunt de meeste workloads.
* Met **Transactionele** verwerking wordt de opslag voor OLTP-workloads van traditionele databases geoptimaliseerd.
* **Gegevensopslag** optimaliseert de opslag voor analyse- en rapportageworkloads.

![Configuratie van SQL VM-opslag](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-storage-configuration.png)

### <a name="sql-server-license"></a>SQL Server License
Als u een Software Assurance-klant bent, u kunt gebruikmaken van de [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) aan uw eigen SQL Server-licentie meenemen en sla op resources. 

![SQL VM-licentie](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-license.png)

### <a name="automated-patching"></a>Automatisch patchen

**Automatisch patchen** is standaard ingeschakeld. Met automatisch patchen kan Azure automatisch een patch uitvoeren voor SQL Server en het besturingssysteem. Geef een dag van de week, een tijd en een periode op voor een onderhoudssessie. Azure voert de patch uit tijdens deze onderhoudssessie. Het onderhoudsschema maakt voor de tijd gebruik van de landinstellingen van de virtuele machine. Als u niet wilt dat Azure een automatische patch uitvoert voor SQL Server en het besturingssysteem, klikt u op **Uitschakelen**.  

![SQL-VM automatisch patchen](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-automated-patching.png)

Zie voor meer informatie [Automatisch patchen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automatische back-up

Schakel automatische databaseback-ups in voor alle databases onder **Automatische back-up**. Automatische back-up is standaard uitgeschakeld.

Als u automatische back-up van SQL inschakelt, kunt u de volgende instellingen configureren:

* De retentieperiode (dagen) voor back-ups
* Het opslagaccount dat voor back-ups moet worden gebruikt
* Versleutelingsoptie en wachtwoord voor back-ups
* Back-up maken van systeemdatabases
* Back-upschema configureren

Voor het versleutelen van de back-up klikt u op **Inschakelen**. Geef het **Wachtwoord** op. Azure maakt een certificaat voor het versleutelen van de back-ups en gebruikt het opgegeven wachtwoord om dit certificaat te beschermen. Standaard wordt het schema automatisch ingesteld, maar u kunt een handmatige schema maken door te selecteren **handmatige**. 

![Geautomatiseerde back-ups van SQL-VM](media/virtual-machines-windows-portal-sql-server-provision/automated-backup.png)

Zie voor meer informatie [Automatische back-up voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).


### <a name="r-services-advanced-analytics"></a>R Services (Advanced Analytics)

U hebt de mogelijkheid om in te schakelen [SQL Server R Services (Advanced Analytics)](/sql/advanced-analytics/r/sql-server-r-services/). Deze optie kunt u geavanceerde analyses gebruiken met SQL Server 2017. Selecteer **inschakelen** op de **SQL Server-instellingen** venster.


## <a name="4-review--create"></a>4. Beoordelen en maken

Op de **bekijken + maken** tabblad, controleert u de samenvatting en selecteert u **maken** om SQL Server, resourcegroep en resources die zijn opgegeven voor deze virtuele machine te maken.

U kunt de implementatie bewaken vanuit Azure Portal. Met de knop **Meldingen** boven aan het scherm kunt u de algemene status van de implementatie weergeven.

> [!NOTE]
> Om u een idee te geven van de implementatietijden, heb ik een virtuele SQL-machine voor de regio US - oost geïmplementeerd met standaardinstellingen. Deze testimplementatie nam in totaal ongeveer 12 minuten in beslag. Afhankelijk van uw regio en de geselecteerde instellingen bent u mogelijk meer of minder tijd kwijt aan de implementatie.

## <a id="remotedesktop"></a> De virtuele machine openen via Extern bureaublad

Doorloop de volgende stappen om via Extern bureaublad verbinding te maken met de virtuele SQL Server-machine:

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Nadat u verbinding hebt gemaakt met de virtuele SQL Server-machine, kunt u SQL Server Management Studio starten en verbinding maken met Windows-verificatie met behulp van de lokale beheerdersreferenties. Als u SQL Server-verificatie inschakelt, kunt u ook verbinding maken met SQL-verificatie. Dit kan met behulp van de SQL-aanmeldingsnaam en het wachtwoord die u hebt geconfigureerd tijdens het inrichten.

Met toegang tot de machine kunt u rechtstreeks de instellingen voor de machine en de SQL Server wijzigen op basis van uw vereisten. U kunt bijvoorbeeld de firewallinstellingen configureren of de SQL Server-configuratie-instellingen wijzigen.

## <a id="connect"></a> Op afstand verbinding maken met SQL Server

In dit scenario hebt geselecteerd **openbare** toegang voor de virtuele machine en **SQL Server-verificatie**. Door deze instellingen is de virtuele machine automatisch geconfigureerd voor het toestaan van SQL Server-verbindingen vanaf elke client via internet (ervan uitgaande dat deze beschikken over de juiste SQL-aanmeldgegevens).

> [!NOTE]
> Als u tijdens het inrichten niet de optie Openbaar hebt geselecteerd, kunt u de instellingen voor SQL-verbindingen achteraf wijzigen via de portal. Zie [Change your SQL connectivity settings](virtual-machines-windows-sql-connect.md#change) (SQL-verbindingsinstellingen wijzigen) voor meer informatie.

De volgende secties tonen hoe u verbinding maakt via internet met uw SQL Server-VM-exemplaar.

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

  > [!NOTE]
  > Dit voorbeeld wordt de algemene poort 1433. Deze waarde moet echter worden gewijzigd als er een andere poort (zoals 1401) is opgegeven tijdens de implementatie van de SQL Server-VM. 


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van SQL Server in Azure [SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) en de [Veelgestelde vragen](virtual-machines-windows-sql-server-iaas-faq.md).