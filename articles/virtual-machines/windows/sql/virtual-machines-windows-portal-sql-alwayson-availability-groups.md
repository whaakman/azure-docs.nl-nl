---
title: Stel een hoge beschikbaarheid voor virtuele machines van Azure Resource Manager | Microsoft Docs
description: Deze zelfstudie leert u hoe u een AlwaysOn-beschikbaarheidsgroep maken met Azure virtual machines in Azure Resource Manager-modus.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: 64e85527-d5c8-40d9-bbe2-13045d25fc68
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: bddc83d55c8909412f7f935a4324a6f316a82cd7
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51238162"
---
# <a name="configure-always-on-availability-groups-in-azure-virtual-machines-automatically-resource-manager"></a>AlwaysOn-beschikbaarheidsgroepen automatisch in Azure Virtual Machines configureren: Resource Manager

Deze zelfstudie leert u over het maken van een beschikbaarheidsgroep van SQL Server die gebruikmaakt van virtuele machines van Azure Resource Manager. De zelfstudie maakt gebruik van Azure-blades sjabloon wilt configureren. U kunt de volgende instellingen, typt u vereiste instellingen en bijwerken van de blades in de portal als u deze zelfstudie hebt doorlopen.

De volledige zelfstudie maakt u een beschikbaarheidsgroep van SQL Server op Azure Virtual Machines met de volgende elementen:

* Een virtueel netwerk met meerdere subnetten, met inbegrip van een front-end- en een back endsubnet
* Twee domeincontrollers waarvoor Active Directory-domein
* Twee virtuele machines die SQL Server wordt uitgevoerd en zijn geïmplementeerd voor de back endsubnet en toegevoegd aan het Active Directory-domein
* Een failovercluster met drie knooppunten met het model van de Quorumconfiguratie Knooppuntmeerderheid
* Een beschikbaarheidsgroep die twee replica's met synchrone doorvoer van een beschikbaarheidsdatabase is

De volgende afbeelding is de volledige oplossing.

![Testlab-architectuur voor beschikbaarheidsgroepen in Azure](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Alle resources in deze oplossing deel uitmaken van één resourcegroep bestaan.

Voordat u deze zelfstudie begint, controleert u het volgende:

* U hebt al een Azure-account. Als u nog geen hebt, [zich aanmelden voor een proefaccount](https://azure.microsoft.com/pricing/free-trial/).
* Weet u al het gebruik van de gebruikersinterface voor het inrichten van een SQL-Server-machine uit de galerie met virtuele machines. Zie voor meer informatie, [inrichten van een virtuele machine van SQL Server op Azure](virtual-machines-windows-portal-sql-server-provision.md).
* U hebt al een goed begrip van beschikbaarheidsgroepen. Zie voor meer informatie, [AlwaysOn-beschikbaarheidsgroepen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Als u geïnteresseerd bent in het gebruik van beschikbaarheidsgroepen met SharePoint, zien ook de [configureren van SQL Server 2012 Always On-beschikbaarheidsgroepen voor SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx).
>
>

In deze zelfstudie gebruikt u Azure portal:

* Kies de sjabloon altijd op vanuit de portal.
* Controleer de sjablooninstellingen en een aantal configuratie-instellingen voor uw omgeving bijwerken.
* Azure monitor wordt de gehele omgeving gemaakt.
* Verbinding maken met een domeincontroller en vervolgens naar een server waarop SQL Server wordt uitgevoerd.

[!INCLUDE [availability-group-template](../../../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]

## <a name="provision-the-cluster-from-the-gallery"></a>Inrichten van het cluster op basis van de galerie
Azure biedt een afbeelding voor de gehele oplossing. Op zoek naar de sjabloon:

1. Meld u bij Azure portal aan met uw account.
2. Klik in de Azure-portal op **een resource maken** openen de **nieuw** deelvenster.
3. Op de **nieuw** deelvenster, zoek naar **AlwaysOn**.
   ![AlwaysOn-sjabloon](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
4. Zoek in de lijst met zoekresultaten **SQL Server AlwaysOn-Cluster**.
   ![AlwaysOn-sjabloon](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
5. Op **een implementatiemodel selecteren**, kiest u **Resource Manager**.

### <a name="basics"></a>Basisbeginselen
Klik op **basisbeginselen** en configureer de volgende instellingen:

* **Gebruikersnaam van beheerder** is een gebruikersaccount met beheerdersmachtigingen voor domein en is lid van de SQL Server vaste serverrol sysadmin op beide exemplaren van SQL Server. Voor deze zelfstudie gebruiken we **DomainAdmin**.
* **Wachtwoord** is het wachtwoord voor het domein administrator-account. Gebruik een complex wachtwoord in. Bevestig het wachtwoord.
* **Abonnement** is het abonnement dat Azure facturen om uit te voeren van alle geïmplementeerde resources voor de beschikbaarheidsgroep. Als uw account meerdere abonnementen heeft, kunt u een ander abonnement.
* **Resourcegroep** is de naam voor de groep waartoe alle Azure-resources die zijn gemaakt met deze sjabloon behoren. Voor deze zelfstudie gebruiken we **SQL-HA-RG**. Zie voor meer informatie [Overzicht van Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md#resource-groups).
* **Locatie** is de Azure-regio waar de zelfstudie maakt u de resources. Kies een Azure-regio.

De volgende schermafbeelding is een voltooide **basisbeginselen** blade:

![Basisbeginselen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

Klik op **OK**.

### <a name="domain-and-network-settings"></a>Domein-en netwerkinstellingen
Deze Azure-galerie-sjabloon maakt u een domein en domeincontrollers. Het maakt ook een netwerk en twee subnetten. De sjabloon kan maken van servers in een bestaand domein of het virtuele netwerk. De volgende stap configureert de domein- en -instellingen.

Op de **domein-en netwerkinstellingen** blade, controleert u de vooraf ingestelde waarden voor het domein en -instellingen:

* **Foresthoofddomeinnaam** de domeinnaam voor het Active Directory-domein dat als host fungeert voor het cluster. Gebruik voor de zelfstudie **contoso.com**.
* **Virtuele-netwerknaam** is de naam van het netwerk voor de Azure-netwerk. Gebruik voor de zelfstudie **autohaVNET**.
* **Naam van de domeincontroller subnet** is de naam van een gedeelte van het virtuele netwerk dat als host fungeert voor de domeincontroller. Gebruik **subnet-1**. Dit subnet gebruikt adresvoorvoegsel **10.0.0.0/24**.
* **De naam van de SQL Server-subnet** is de naam van een gedeelte van het virtuele netwerk dat als host fungeert voor de servers dat Voer SQL Server en de witness bestandsshare. Gebruik **subnet 2**. Dit subnet gebruikt adresvoorvoegsel **10.0.1.0/26**.

Zie voor meer informatie over virtuele netwerken in Azure, [overzicht van Virtual network](../../../virtual-network/virtual-networks-overview.md).  

De **domein-en netwerkinstellingen** moet eruitzien als in de volgende schermafbeelding:

![Domein-en netwerkinstellingen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Indien nodig, kunt u deze waarden wijzigen. Voor deze zelfstudie gebruikt u de vooraf gedefinieerde waarden.

Controleer de instellingen en klik vervolgens op **OK**.

### <a name="availability-group-settings"></a>Instellingen voor de groep beschikbaarheid
Op **beschikbaarheid groepsinstellingen**, controleert u de vooraf ingestelde waarden voor de beschikbaarheidsgroep en de listener.

* **Naam van de beschikbaarheidsgroep** is de naam van de geclusterde bron voor de beschikbaarheidsgroep. Voor deze zelfstudie gebruiken we **Contoso-ag**.
* **Naam van de beschikbaarheidsgroep-listener** wordt gebruikt door het cluster en de interne load balancer. Clients die verbinding met SQL Server maken, kunnen deze naam gebruiken verbinding maken met de juiste replica van de database. Voor deze zelfstudie gebruiken we **Contoso-listener**.
* **Poort beschikbaarheidsgroeplistener** Hiermee geeft u de TCP-poort van de listener voor de SQL Server. Voor deze zelfstudie gebruikt u de standaardpoort **1433**.

Indien nodig, kunt u deze waarden wijzigen. Voor deze zelfstudie gebruikt u de vooraf gedefinieerde waarden.  

![instellingen voor de groep beschikbaarheid](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

Klik op **OK**.

### <a name="virtual-machine-size-storage-settings"></a>De grootte van de virtuele machine, instellingen voor de opslag
Op **VM-grootte, instellingen voor de opslag**, kiest u de grootte van de virtuele machine van een SQL Server en controleer de andere instellingen.

* **De grootte van de SQL Server-virtuele machine** is de grootte voor zowel virtuele machines waarop SQL Server wordt uitgevoerd. Kies een grootte van de juiste virtuele machine voor uw workload. Als u deze omgeving voor de zelfstudie bouwt, gebruikt u **DS2**. Kies een grootte van virtuele machine die ondersteuning voor de werkbelasting bieden voor productieworkloads. Veel productieworkloads vereisen **DS4** of groter. De sjabloon twee virtuele machines van deze grootte is gebaseerd en installeert SQL Server op elk. Zie voor meer informatie, [grootten voor virtuele machines](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Azure installeert de Enterprise-editie van SQL Server. De kosten zijn afhankelijk van de editie en de grootte van de virtuele machine. Zie voor gedetailleerde informatie over de huidige kosten [prijzen voor virtual machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).
>
>

* **Grootte van de virtuele machine domain controller** is de grootte van de virtuele machine voor de domeincontrollers. Voor deze zelfstudie wordt gebruikgemaakt van **D2**.
* **Virtuele machine voor een bestandsshare-Witness bestandsgrootte** de grootte van de virtuele machine is voor de bestandsshare-witness. Voor deze zelfstudie gebruiken we **A1**.
* **SQL-Storage-account** is de naam van het opslagaccount waarin de SQL Server-gegevens en besturingssysteem-schijven. Voor deze zelfstudie gebruiken we **alwaysonsql01**.
* **DC-Storage-account** is de naam van het opslagaccount voor de domeincontrollers. Voor deze zelfstudie gebruiken we **alwaysondc01**.
* **SQL Server-gegevens schijfgrootte** in TB is de grootte van de SQL Server-gegevensschijf in TB. Geef een getal van 1 tot en met 4. Voor deze zelfstudie gebruiken we **1**.
* **Opslagoptimalisatie** stelt configuratie-instellingen voor specifieke opslag voor de virtuele machines met SQL Server op basis van het type werkbelasting. Alle virtuele machines met SQL Server in dit scenario wordt premium storage gebruiken met Azure disk host cache ingesteld op alleen-lezen. Bovendien kunt u SQL Server-instellingen voor de werkbelasting optimaliseren door een van deze drie instellingen te kiezen:

  * **Algemene workload** stelt geen specifieke configuratie-instellingen.
  * **Transactionele verwerking** sets vlag 1117 en 1118 traceren.
  * **Datawarehousing** sets vlag 1117 traceren en 610.

Voor deze zelfstudie gebruiken we **algemene workload**.

![Opslaginstellingen voor VM-grootte](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

Controleer de instellingen en klik vervolgens op **OK**.

#### <a name="a-note-about-storage"></a>Een opmerking over storage
Aanvullende optimaliseringen zijn afhankelijk van de grootte van de SQL Server-gegevensschijven. Voor elke terabyte aan gegevensschijf voegt Azure een extra 1 TB aan premium-opslag. Wanneer een server 2 TB of meer vereist, maakt de sjabloon een opslaggroep op elke virtuele machine van SQL Server. Een opslaggroep is een soort virtualisatie van opslagruimte waar meerdere schijven zijn geconfigureerd om een hogere capaciteit, tolerantie en prestaties.  De sjabloon vervolgens maakt een opslagruimte op de opslaggroep en één gegevensschijf aan het besturingssysteem geeft. De sjabloon wordt deze schijf als de gegevensschijf voor SQL Server. De sjabloon afgestemd de opslaggroep voor SQL Server met behulp van de volgende instellingen:

* Stripe-grootte is de interleave-instelling voor de virtuele schijf. Transactionele werkbelastingen gebruik 64 KB. Datawarehousing-workloads gebruik 256 KB.
* Flexibiliteit is eenvoudig (geen tolerantie).

> [!NOTE]
> Azure premium storage is lokaal redundante en houdt drie kopieën van de gegevens binnen één regio, zodat u meer tolerantie aan de opslaggroep niet vereist is.
>
>

* Het aantal kolommen is gelijk aan het aantal schijven in de opslaggroep.

Zie voor meer informatie over de opslagruimte en opslaggroepen:

* [Overzicht van opslagruimten](https://technet.microsoft.com/library/hh831739.aspx)
* [Windows Server back-up- en opslaggroepen](https://technet.microsoft.com/library/dn390929.aspx)

Zie voor meer informatie over aanbevolen procedures voor SQL Server-configuratie, [best practices prestaties for SQL Server in virtuele Azure-machines](virtual-machines-windows-sql-performance.md).

### <a name="sql-server-settings"></a>SQL Server-instellingen
Op **SQL Server-instellingen**, bekijken en wijzigen van het voorvoegsel van de SQL Server virtuele machine, SQL Server-versie, SQL Server-serviceaccount en wachtwoord en de onderhoudsplanning van SQL auto patching.

* **SQL Server de naam van het voorvoegsel** wordt gebruikt om een naam voor elke virtuele machine van SQL Server te maken. Voor deze zelfstudie gebruiken we **sqlserver**. De sjabloon-namen van de SQL Server-machines *sqlserver-0* en *sqlserver-1*.
* **SQL Server-versie** is de versie van SQL Server. Voor deze zelfstudie wordt gebruikgemaakt van **SQL Server 2014**. U kunt ook **SQL Server 2012** of **SQL Server 2016**.
* **Gebruikersnaam voor SQL Server service account** is de naam van de domein-account voor de SQL Server-service. Voor deze zelfstudie gebruiken we **sqlservice**.
* **Wachtwoord** is het wachtwoord voor de SQL Server-serviceaccount.  Gebruik een complex wachtwoord in. Bevestig het wachtwoord.
* **Onderhoudsplanning SQL Auto Patching** identificeert de dag van de week dat de SQL-Servers die door Azure automatisch patches. Voor deze zelfstudie, typt u **zondag**.
* **SQL Auto Patching onderhoud starten uur** is de tijd van de dag voor de Azure-regio als de automatische toepassing van patches begint.

> [!NOTE]
> Het tijdvenster voor elke virtuele machine is gespreid door één uur. Slechts één virtuele machine is een patch uitgevoerd op een moment om te voorkomen dat tot een onderbreking van services.
>
>

![SQL Server-instellingen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Controleer de instellingen en klik vervolgens op **OK**.

### <a name="summary"></a>Samenvatting
Azure valideert op de pagina overzicht de instellingen. U kunt ook de sjabloon downloaden. Bekijk de samenvatting. Klik op **OK**.

### <a name="buy"></a>Kopen
Deze laatste blade bevat **gebruiksvoorwaarden**, en **privacybeleid**. Bekijk deze informatie. Wanneer u gereed bent voor Azure nodig om te beginnen met het maken van de virtuele machines en alle andere resources voor de beschikbaarheidsgroep, klikt u op **maken**.

De Azure-portal de resourcegroep en alle resources gemaakt.

## <a name="monitor-deployment"></a>Monitor voor implementatie
Controleer de voortgang van de implementatie van de Azure-portal. Een pictogram dat staat voor de implementatie is automatisch vastgemaakt aan het dashboard van de Azure portal.

![Azure Dashboard](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

## <a name="connect-to-sql-server"></a>Verbinding maken met SQL Server
De nieuwe exemplaren van SQL Server worden uitgevoerd op virtuele machines die met internet verbonden IP-adressen. U kunt Extern bureaublad (RDP) rechtstreeks naar elke virtuele machine van SQL Server.

Volg deze stappen voor RDP naar een SQL Server:

1. Controleer in de Azure portal-dashboard, of dat de implementatie is voltooid.
2. Klik op **Resources**.
3. In de **Resources** blade, klikt u op **sqlserver-0**, dit is de naam van de computer van een van de virtuele machines die SQL Server wordt uitgevoerd.
4. Op de blade voor **sqlserver-0**, klikt u op **Connect**. Uw browser wordt gevraagd of u wilt openen of opslaan van de verbinding met extern-object. Klik op **Openen**.
5. **Verbinding met extern bureaublad** gewaarschuwd dat de uitgever van deze externe verbinding kan niet worden geïdentificeerd. Klik op **Verbinden**.
6. Windows-beveiliging vraagt u uw referenties verbinding maken met het IP-adres van de primaire domeincontroller in te voeren. Klik op **gebruik een ander account**. Voor **gebruikersnaam**, type **contoso\DomainAdmin**. U kunt dit account geconfigureerd wanneer u de beheerder van de gebruikersnaam in de sjabloon. Het complex wachtwoord dat u hebt gekozen bij het configureren van de sjabloon gebruiken.
7. **Extern bureaublad** gewaarschuwd dat de externe computer niet kan worden geverifieerd vanwege problemen met het beveiligingscertificaat. Hier ziet u de naam van de beveiliging. Als u de zelfstudie hebt gevolgd, is de naam **sqlserver 0.contoso.com**. Klik op **Ja**.

U bent nu verbonden met RDP naar de SQL Server-machine. U kunt SQL Server Management Studio opent, verbinding maken met het standaardexemplaar van SQL Server en controleren of de beschikbaarheidsgroep is geconfigureerd.
