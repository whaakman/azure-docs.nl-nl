---
title: Hoge beschikbaarheid instellen voor virtuele machines van Azure Resource Manager | Microsoft Docs
description: Deze zelfstudie laat zien hoe u een AlwaysOn-beschikbaarheidsgroep maken met Azure virtuele machines in de modus Azure Resource Manager.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 64e85527-d5c8-40d9-bbe2-13045d25fc68
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: d430febee23081b26eee0a68d4beb43228549f52
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-always-on-availability-groups-in-azure-virtual-machines-automatically-resource-manager"></a>Altijd op beschikbaarheidsgroepen automatisch in Azure Virtual Machines configureren: Resource Manager

Deze zelfstudie laat zien hoe u een SQL Server-beschikbaarheidsgroep die gebruikmaakt van Azure Resource Manager virtuele machines maken. De zelfstudie maakt gebruik van Azure-blades voor het configureren van een sjabloon. U kunt controleren de standaardinstellingen, typt u vereiste instellingen en de blades in de portal bijwerken als u deze zelfstudie hebt doorlopen.

De volledige zelfstudie maakt een SQL Server-beschikbaarheidsgroep op Azure Virtual Machines die de volgende elementen bevatten:

* Een virtueel netwerk met meerdere subnetten, met inbegrip van een front-end- en een back-end-subnet
* Twee domeincontrollers waarvoor Active Directory-domein
* Twee virtuele machines die SQL Server wordt uitgevoerd en zijn geïmplementeerd met het subnet voor back-end en toegevoegd aan het Active Directory-domein
* Een failovercluster met drie knooppunten met het model van de Quorumconfiguratie Knooppuntmeerderheid
* Een beschikbaarheidsgroep met twee synchrone doorvoer replica's van een beschikbaarheidsdatabase

De volgende afbeelding geeft de volledige oplossing.

![Testlab-architectuur voor beschikbaarheidsgroepen in Azure](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Alle resources in deze oplossing behoren tot één resourcegroep.

Voordat u deze zelfstudie begint, controleert u het volgende:

* U hebt al een Azure-account. Als u nog geen hebt, [aanmelden voor een proefaccount](http://azure.microsoft.com/pricing/free-trial/).
* Weet u al het gebruik van de gebruikersinterface voor het inrichten van een virtuele machine van SQL Server uit de galerie met virtuele machine. Zie voor meer informatie [inrichten van een virtuele machine van SQL Server op Azure](virtual-machines-windows-portal-sql-server-provision.md).
* U hebt al een goed begrip van beschikbaarheidsgroepen. Zie voor meer informatie [altijd op beschikbaarheidsgroepen (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Als u geïnteresseerd bent in het gebruik van beschikbaarheidsgroepen met SharePoint, zien ook de [configureren van SQL Server 2012 Always On beschikbaarheidsgroepen voor SharePoint 2013](http://technet.microsoft.com/library/jj715261.aspx).
>
>

In deze zelfstudie gebruikt u de Azure-portal:

* Kies de sjabloon altijd op in de portal.
* Controleer de sjablooninstellingen en een aantal configuratie-instellingen voor uw omgeving bijwerken.
* Azure bewaken tijdens de gehele omgeving wordt gemaakt.
* Verbinding maken met een domeincontroller en vervolgens naar een server waarop SQL Server wordt uitgevoerd.

[!INCLUDE [availability-group-template](../../../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]

## <a name="provision-the-cluster-from-the-gallery"></a>Het cluster uit de galerie inrichten
Azure biedt een afbeelding voor de hele oplossing. De sjabloon zoeken:

1. Meld u aan bij de Azure portal met behulp van uw account.
2. Klik in de Azure-portal op **+ nieuw** openen de **nieuw** blade.
3. Op de **nieuw** blade, zoekt u **AlwaysOn**.
   ![AlwaysOn-sjabloon zoeken](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
4. Zoek in de lijst met zoekresultaten **SQL Server AlwaysOn-Cluster**.
   ![AlwaysOn-sjabloon](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
5. Op **een implementatiemodel selecteren**, kies **Resource Manager**.

### <a name="basics"></a>Basisbeginselen
Klik op **basisbeginselen** en configureer de volgende instellingen:

* **Gebruikersnaam van beheerder** is een gebruikersaccount met beheerdersmachtigingen voor domein- en lid is van de SQL Server vaste serverrol sysadmin voor beide exemplaren van SQL Server. Gebruik voor deze zelfstudie **DomainAdmin**.
* **Wachtwoord** is het wachtwoord voor het administrator-account van het domein. Een complex wachtwoord gebruiken. Bevestig het wachtwoord.
* **Abonnement** is het abonnement dat Azure facturen om uit te voeren van alle geïmplementeerde resources voor de beschikbaarheidsgroep. Als uw account meerdere abonnementen heeft, kunt u een ander abonnement.
* **Resourcegroep** is de naam voor de groep waartoe alle Azure-resources die zijn gemaakt met deze sjabloon behoren. Gebruik voor deze zelfstudie **SQL-HA-RG**. Zie voor meer informatie [Overzicht van Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md#resource-groups).
* **Locatie** is de Azure-regio waar de resources in de zelfstudie wordt gemaakt. Kies een Azure-regio.

De volgende schermafbeelding is een voltooide **basisbeginselen** blade:

![Basisbeginselen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

Klik op **OK**.

### <a name="domain-and-network-settings"></a>Domein-en netwerkinstellingen
Deze Azure-galerie-sjabloon maakt een domein en domeincontrollers. Dit leidt ook tot een netwerk en twee subnets. De sjabloon kan maken van servers in een bestaand domein of een virtueel netwerk. De volgende stap configureert de domein- en -instellingen.

Op de **domein-en netwerkinstellingen** blade, controleert u de vooraf gedefinieerde waarden voor het domein en -instellingen:

* **Foresthoofddomeinnaam** de domeinnaam voor het Active Directory-domein dat als host fungeert voor het cluster. Gebruik voor de zelfstudie **contoso.com**.
* **Virtuele-netwerknaam** is de naam van het netwerk voor de virtuele Azure-netwerk. Gebruik voor de zelfstudie **autohaVNET**.
* **Naam van de domeincontroller subnet** is de naam van een deel van het virtuele netwerk die als host fungeert voor de domeincontroller. Gebruik **subnet 1**. Dit subnet gebruikt adresvoorvoegsel **10.0.0.0/24**.
* **De naam van de SQL Server-subnet** is de naam van een deel van het virtuele netwerk die als host fungeert voor de servers of voer SQL Server en de witness bestandsshare. Gebruik **subnet 2**. Dit subnet gebruikt adresvoorvoegsel **10.0.1.0/26**.

Zie voor meer informatie over virtuele netwerken in Azure, [Virtual network-overzicht](../../../virtual-network/virtual-networks-overview.md).  

De **domein-en netwerkinstellingen** moet eruitzien als in de volgende schermafbeelding:

![Domein-en netwerkinstellingen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Indien nodig, kunt u deze waarden te wijzigen. Gebruik de vooraf gedefinieerde waarden voor deze zelfstudie.

Controleer de instellingen en klik vervolgens op **OK**.

### <a name="availability-group-settings"></a>Instellingen voor beschikbaarheid
Op **beschikbaarheid groepsinstellingen**, controleert u de vooraf gedefinieerde waarden voor de beschikbaarheidsgroep en de listener.

* **Naam van de beschikbaarheidsgroep** is de naam van de geclusterde bron voor de beschikbaarheidsgroep. Gebruik voor deze zelfstudie **Contoso ag**.
* **Naam van de beschikbaarheidsgroep-listener** wordt gebruikt door het cluster en de interne load balancer. Clients die verbinding met SQL Server maken, kunnen deze naam gebruiken verbinding maken met de juiste replica van de database. Gebruik voor deze zelfstudie **Contoso-listener**.
* **Poort beschikbaarheidsgroeplistener** de TCP-poort van de SQL Server-listener. De standaardpoort gebruikt die voor deze zelfstudie **1433**.

Indien nodig, kunt u deze waarden te wijzigen. Gebruik de vooraf gedefinieerde waarden voor deze zelfstudie.  

![Instellingen voor beschikbaarheid](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

Klik op **OK**.

### <a name="virtual-machine-size-storage-settings"></a>De grootte van de virtuele machine, instellingen voor de opslag
Op **VM-grootte, instellingen voor de opslag**, kies de grootte van een SQL Server-virtuele machine en bekijk de andere instellingen.

* **De grootte van de SQL Server-virtuele machine** is de grootte voor zowel virtuele machines waarop SQL Server wordt uitgevoerd. Kies de grootte van een juiste virtuele machine voor uw workload. Als u deze omgeving voor de zelfstudie maakt, gebruikt u **DS2**. Kies de grootte van een virtuele machine die ondersteuning voor de werkbelasting biedt voor productieworkloads. Veel productieworkloads vereisen **DS4** of hoger. De sjabloon maakt twee virtuele machines van deze omvang en installeert SQL Server op elke service. Zie voor meer informatie [grootten voor virtuele machines](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Azure installeert de Enterprise-editie van SQL Server. De kosten zijn afhankelijk van de editie en de grootte van de virtuele machine. Zie voor gedetailleerde informatie over de kosten van de huidige [virtuele machines prijzen](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).
>
>

* **Grootte van de virtuele machine domain controller** is de grootte van de virtuele machine voor de domeincontrollers. Voor deze zelfstudie gebruik **D2**.
* **De grootte van de virtuele machine delen Witness bestand** is de grootte van de virtuele machine voor de bestandsshare-witness. Gebruik voor deze zelfstudie **A1**.
* **SQL-opslagaccount** is de naam van het opslagaccount waarin de SQL Server-gegevens en het besturingssysteem schijven. Gebruik voor deze zelfstudie **alwaysonsql01**.
* **DC-opslagaccount** is de naam van het opslagaccount voor de domeincontrollers. Gebruik voor deze zelfstudie **alwaysondc01**.
* **SQL Server-gegevens schijfgrootte** in TB is de grootte van de SQL Server-gegevensschijf in TB. Geef een getal van 1 tot en met 4. Gebruik voor deze zelfstudie **1**.
* **Opslagoptimalisatie** opslag voor specifieke configuratie-instellingen voor de SQL Server-virtuele machines op basis van het type werkbelasting ingesteld. Alle SQL Server-virtuele machines in dit scenario gebruik premium-opslag met Azure host schijfcache ingesteld op alleen-lezen. Bovendien kunt u SQL Server-instellingen voor de werkbelasting optimaliseren door een van deze drie instellingen te kiezen:

  * **Algemene werkbelasting** stelt geen specifieke configuratie-instellingen.
  * **Transactionele verwerking** sets vlag 1117 en 1118 traceren.
  * **Gegevensopslag** sets vlag 1117 traceren en 610.

Gebruik voor deze zelfstudie **algemene werkbelasting**.

![Opslaginstellingen voor VM-grootte](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

Controleer de instellingen en klik vervolgens op **OK**.

#### <a name="a-note-about-storage"></a>Een opmerking over opslag
Aanvullende optimaliseringen zijn afhankelijk van de grootte van de SQL Server data-schijven. Voor elke status van de gegevensschijf toegevoegd Azure een extra 1 TB premium-opslag. Wanneer een server 2 TB of meer vereist, maakt de sjabloon u een opslaggroep op elke virtuele machine van SQL Server. Een opslaggroep is een vorm van opslagvirtualisatie waar meerdere schijven zijn geconfigureerd voor hogere capaciteit, tolerantie en prestaties.  De sjabloon vervolgens maakt een opslagruimte op de opslaggroep en biedt een enkel gegevensschijf voor het besturingssysteem. De sjabloon wordt deze schijf als de gegevensschijf voor SQL Server. De sjabloon verbetert de de opslaggroep prestatie voor SQL Server met behulp van de volgende instellingen:

* Stripe-grootte is de interleave-instelling voor de virtuele schijf. Transactionele werkbelastingen gebruikt 64 KB. Gegevensopslag werkbelastingen gebruiken 256 KB.
* Tolerantie is eenvoudig (geen tolerantie).

> [!NOTE]
> Azure premium-opslag is lokaal redundant en houdt drie kopieën van de gegevens in één regio, zodat u meer tolerantie aan de opslaggroep niet vereist is.
>
>

* Aantal kolommen is gelijk aan het aantal schijven in de opslaggroep.

Zie voor meer informatie over de opslagruimte en opslaggroepen:

* [Overzicht van opslagruimten](http://technet.microsoft.com/library/hh831739.aspx)
* [Windows Server back-up- en opslaggroepen](http://technet.microsoft.com/library/dn390929.aspx)

Zie voor meer informatie over aanbevolen procedures voor SQL Server-configuratie, [best practices prestaties for SQL Server op Azure virtual machines](virtual-machines-windows-sql-performance.md).

### <a name="sql-server-settings"></a>SQL Server-instellingen
Op **SQL Server-instellingen**, bekijken en wijzigen van de virtuele machine voorvoegsel van de SQL Server, SQL Server-versie, SQL Server-serviceaccount en wachtwoord en de onderhoudsplanning van SQL auto patching.

* **SQL Server voorvoegsel** gebruikt voor het maken van een naam op voor elke virtuele machine van SQL Server. Gebruik voor deze zelfstudie **sqlserver**. De sjabloon de namen van de virtuele machines van SQL Server *sqlserver 0* en *sqlserver 1*.
* **SQL Server-versie** is de versie van SQL Server. Voor deze zelfstudie gebruik **SQL Server 2014**. U kunt ook **SQL Server 2012** of **SQL Server 2016**.
* **Gebruikersnaam voor SQL Server serviceaccount** de domeinnaam van de account voor de SQL Server-service. Gebruik voor deze zelfstudie **sqlservice**.
* **Wachtwoord** is het wachtwoord voor de SQL Server-serviceaccount.  Een complex wachtwoord gebruiken. Bevestig het wachtwoord.
* **Onderhoudsplanning SQL Auto Patching** identificeert de dag van de week dat Azure automatisch patches van de SQL-Servers. Typ voor deze zelfstudie **zondag**.
* **Beginuur van SQL Auto Patching onderhoud** is de tijd voor de Azure-regio als de automatische patchen begint.

> [!NOTE]
> Het tijdvenster voor elke virtuele machine is gespreid van één uur. Slechts één virtuele machine is op een tijdstip om onderbreking van services te voorkomen dat hersteld.
>
>

![SQL Server-instellingen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Controleer de instellingen en klik vervolgens op **OK**.

### <a name="summary"></a>Samenvatting
Azure valideert op de pagina overzicht de instellingen. U kunt ook de sjabloon downloaden. Bekijk de samenvatting. Klik op **OK**.

### <a name="buy"></a>Kopen
Deze laatste blade bevat **gebruiksvoorwaarden**, en **privacybeleid**. Bekijk deze informatie. Wanneer u gereed bent voor Azure vereist om te beginnen met het maken van de virtuele machines en alle andere bronnen voor de beschikbaarheidsgroep, klikt u op **maken**.

De Azure-portal maakt de resourcegroep en alle resources.

## <a name="monitor-deployment"></a>Monitor voor implementatie
De implementatie voortgang van de Azure-portal. Een pictogram dat staat voor de implementatie is automatisch vastgemaakt aan de Azure-portaldashboard.

![Azure-Dashboard](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

## <a name="connect-to-sql-server"></a>Verbinding maken met SQL Server
De nieuwe exemplaren van SQL Server worden uitgevoerd op virtuele machines met internet verbonden IP-adressen. U kunt Extern bureaublad (RDP) rechtstreeks aan elke virtuele machine van SQL Server.

Volg deze stappen voor RDP naar een SQL Server:

1. Controleren dat de implementatie is verwijderd uit de Azure-portaldashboard.
2. Klik op **Resources**.
3. In de **Resources** blade, klikt u op **sqlserver 0**, dit is de naam van de computer van een van de virtuele machines die SQL Server wordt uitgevoerd.
4. Op de blade voor **sqlserver 0**, klikt u op **Connect**. Uw browser wordt gevraagd of u wilt openen of opslaan van de verbinding met extern object. Klik op **Open**.
5. **Verbinding met extern bureaublad** worden gewaarschuwd dat de uitgever van deze externe verbinding kan niet worden vastgesteld. Klik op **Verbinden**.
6. Windows-beveiliging vraagt u uw referenties verbinding maken met het IP-adres van de primaire domeincontroller in te voeren. Klik op **gebruik een ander account**. Voor **gebruikersnaam**, type **contoso\DomainAdmin**. U kunt dit account geconfigureerd wanneer u de beheerder van de gebruikersnaam in de sjabloon. Het complex wachtwoord dat u hebt gekozen bij het instellen van de sjabloon gebruiken.
7. **Extern bureaublad** worden gewaarschuwd dat de externe computer niet kan worden geverifieerd vanwege problemen met het beveiligingscertificaat. Hier ziet u de naam van de beveiliging. Als u de zelfstudie hebt gevolgd, is de naam **sqlserver 0.contoso.com**. Klik op **Ja**.

U hebt nu verbinding met RDP met de virtuele machine van SQL Server. U kunt SQL Server Management Studio te openen, verbinding maken met het standaardexemplaar van SQL Server en controleren of de beschikbaarheidsgroep is geconfigureerd.
