---
title: SQL-Servers in Azure Stack Hosting | Microsoft Docs
description: Het SQL-exemplaren voor inrichting met de resourceprovider van SQL-Adapter toe te voegen.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: e8dd425bbb5839b1c2f5ad4e217c61dc50b38ce1
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37346821"
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>Hosting-servers voor de SQL-resourceprovider toevoegen

U kunt een SQL-exemplaar op een virtuele machine (VM) hosten [Azure Stack](azure-stack-poc.md), of op een virtuele machine buiten uw Azure Stack-omgeving, zolang de SQL-resourceprovider kunt verbinding maken met het exemplaar.

## <a name="overview"></a>Overzicht

Voordat u een SQL server die als host fungeert, Controleer u de volgende verplichte en algemene vereisten.

### <a name="mandatory-requirements"></a>Verplichte vereisten

* SQL-verificatie inschakelen op de SQL Server-exemplaar. Omdat de SQL-resourceprovider VM niet is toegevoegd aan een domein, kan deze alleen verbinding maken met een host-server met SQL-verificatie.
* De IP-adressen voor de SQL-exemplaren configureren als openbare wanneer geïnstalleerd in Azure Stack. De resourceprovider en gebruikers, zoals Web-Apps, communiceren via het gebruikersnetwerk, zodat de verbinding met de SQL-exemplaar op dit netwerk vereist is.

### <a name="general-requirements"></a>Algemene vereisten

* Toe te wijzen aan het SQL-exemplaar voor gebruik door de resource-provider en gebruiker werkbelastingen. U kunt een SQL-exemplaar dat wordt gebruikt door een andere consument niet gebruiken. Deze beperking geldt ook voor App Services.
* Een account met de juiste bevoegdheidsniveaus configureren voor de resourceprovider (Zie hieronder).
* U bent verantwoordelijk voor het beheren van de SQL-instanties en hun hosts zijn.  Bijvoorbeeld, de resourceprovider niet updates toe te passen, back-ups worden verwerkt of verwerken rotatie van referenties.

### <a name="sql-server-virtual-machine-images"></a>SQL Server-installatiekopieën voor virtuele machines

SQL IaaS-installatiekopieën voor virtuele machines zijn beschikbaar via de Marketplace-Management-functie. Deze installatiekopieën zijn hetzelfde als de SQL-VM's die beschikbaar in Azure zijn.

Zorg ervoor dat u altijd downloaden de nieuwste versie van de **SQL IaaS-extensie** voordat u een SQL-VM met behulp van een Marketplace-item implementeert. De IaaS-extensie en de bijbehorende portal verbeteringen bieden aanvullende functies, zoals automatische toepassing van patches en back-up. Zie voor meer informatie over deze extensie [automatiseren van beheertaken op Azure Virtual Machines met de SQL Server Agent-extensie](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension).

Er zijn andere opties voor het implementeren van de SQL-VM's, met inbegrip van sjablonen in de [Azure Stack-Snelstartgalerie](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Hosting servers geïnstalleerd op een Azure-Stack van meerdere knooppunten moeten worden gemaakt op basis van een gebruikersabonnement en niet de standaard-Provider van het abonnement. Ze moeten worden gemaakt vanuit de gebruikersportal of vanuit een PowerShell-sessie met een juiste aanmelding. Alle hosting-servers zijn factureerbaar VM's en moeten de juiste SQL-licenties. De servicebeheerder _kunt_ de eigenaar van het abonnement.

### <a name="required-privileges"></a>Vereiste bevoegdheden

U kunt een gebruiker met beheerdersrechten maken met minder bevoegdheden dan een SQL sysadmin. De gebruiker moet alleen machtigingen voor de volgende bewerkingen:

* Database: Maken, wijzigen, met Containment (voor Always On alleen), verwijderen, back-up maken
* Beschikbaarheid van groep: Alter, toevoegen, toevoegen of verwijderen-Database
* Aanmelding: Maken, selecteert u, Alter, Drop, intrekken
* Selecteer bewerkingen: \[master\].\[ sys\].\[ availability_group_listeners\] (AlwaysOn), uitvoeren op sys.availability_replicas (AlwaysOn), sys.databases, \[master\].\[ sys\].\[ dm_os_sys_memory\], SERVERPROPERTY, \[master\].\[ sys\].\[ availability_groups\] (AlwaysOn), sys.master_files

### <a name="additional-security-information"></a>Informatie over extra beveiliging

De volgende informatie geeft richtlijnen voor extra beveiliging:

* Alle Azure Stack-opslag is versleuteld met BitLocker, zodat een SQL-exemplaar in Azure Stack wordt versleutelde blob-opslag gebruiken.
* De SQL-Resourceprovider biedt volledige ondersteuning voor TLS 1.2. Zorg ervoor dat alle SQL-Server die wordt beheerd via de RP SQL is geconfigureerd voor TLS 1.2 _alleen_ en de relying Party die standaard. Alle ondersteunde versies van SQL Server ondersteuning voor TLS 1.2, Zie [TLS 1.2-ondersteuning voor Microsoft SQL Server](https://support.microsoft.com/en-us/help/3135244/tls-1-2-support-for-microsoft-sql-server).
* SQL Server Configuration Manager gebruiken om in te stellen de **ForceEncryption** optie om te controleren of alle communicatie met de SQL server worden altijd versleuteld. Zie [configureren van de server om af te dwingen versleutelde verbindingen](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine?view=sql-server-2017#ConfigureServerConnections).
* Zorg ervoor dat elke clienttoepassing ook communiceert via een gecodeerde verbinding.
* De relying Party is geconfigureerd voor het vertrouwen van de certificaten die worden gebruikt door de SQL Server-exemplaren.

## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>Capaciteit bieden door verbinding te maken met een zelfstandige SQL server die als host fungeert

U kunt zelfstandige (niet-HA) SQL-servers met behulp van een versie van SQL Server 2014 of SQL Server 2016. Zorg ervoor dat u de referenties voor een account met de sysadmin-bevoegdheden hebben.

Als u wilt toevoegen van een zelfstandige host-server die al ingesteld, de volgende stappen uit:

1. Meld u aan de portal van Azure Stack-operator als de servicebeheerder van een.

2. Selecteer **Bladeren** &gt; **BEHEERDERSRESOURCES** &gt; **SQL Servers die als host fungeert**.

   ![SQL-Servers die als host fungeert](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

   Onder **SQL-Servers die als host fungeert**, u kunt de SQL-resourceprovider verbinding maken met exemplaren van SQL Server, die als back-end van de resourceprovider dienen.

   ![SQL-Adapter-dashboard](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. Op **toevoegen van een SQL-Server die als host fungeert**, bieden de verbindingsgegevens voor uw SQL Server-exemplaar.

   ![Toevoegen van een SQL Server die als host fungeert](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    (Optioneel) Geef de exemplaarnaam van een en een poortnummer opgeven als het exemplaar is niet aan de standaardpoort 1433 toegewezen.

   > [!NOTE]
   > Als het SQL-exemplaar kan worden geopend door de gebruiker en de Azure Resource Manager-beheerder, kan deze onder beheer van de resourceprovider worden geplaatst. Het SQL-exemplaar __moet__ worden toegewezen aan de resourceprovider.

4. Als u servers hebt toegevoegd, moet u deze toewijzen aan een bestaande SKU of maken van een nieuwe SKU. Onder **toevoegen van een Server die als host fungeert**, selecteer **SKU's**.

   * Kies een beschikbare SKU's voor het gebruik van een bestaande SKU, en selecteer vervolgens **maken**.
   * Voor het maken van een SKU selecteert **+ maken van nieuwe Voorraadeenheid**. In **SKU maken**, voer de vereiste gegevens in en selecteer vervolgens **OK**.

     > [!IMPORTANT]
     > Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in **naam** veld. Gebruik de voorbeelden in de volgende schermopname Voer waarden in voor de **familie**, **laag**, en **Edition** velden.

     ![Een SKU maken](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

## <a name="provide-high-availability-using-sql-always-on-availability-groups"></a>Zorgen voor hoge beschikbaarheid met behulp van SQL Always On Availability Groups

Exemplaren van SQL Always On configureren zijn extra stappen vereist en moet drie VM's (of fysieke machines.) In dit artikel wordt ervan uitgegaan dat u al een goed begrip van AlwaysOn-beschikbaarheidsgroepen. Raadpleeg voor meer informatie de volgende artikelen:

* [Maak kennis met SQL Server Always On-beschikbaarheidsgroepen op Azure virtual machines](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)
* [Always On Availability Groups (SQL Server)](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2017)

> [!NOTE]
> De resourceprovider van de SQL-adapter _alleen_ biedt ondersteuning voor SQL 2016 SP1 Enterprise of hoger voor Always On Availability Groups-exemplaren. Deze adapterconfiguratie moet de nieuwe SQL-functies, zoals automatische seeding.

### <a name="automatic-seeding"></a>Automatische seeding

U moet inschakelen [automatische Seeding](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) op elke beschikbaarheidsgroep voor elk exemplaar van SQL Server.

Als u wilt inschakelen automatische seeding op alle instanties, bewerken en voer vervolgens de volgende SQL-opdracht voor elk exemplaar:

  ```sql
  ALTER AVAILABILITY GROUP [<availability_group_name>]
      MODIFY REPLICA ON 'InstanceName'
      WITH (SEEDING_MODE = AUTOMATIC)
  GO
  ```

Op de secundaire exemplaren, bewerken en voer vervolgens de volgende SQL-opdracht voor elk exemplaar:

  ```sql
  ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
  GO
  ```

### <a name="configure-contained-database-authentication"></a>Ingesloten database-verificatie configureren

Voordat u een ingesloten database aan een beschikbaarheidsgroep toevoegt, zorg ervoor dat de ingesloten database authentication server-optie is ingesteld op 1 op elke serverexemplaar dat als host fungeert voor een beschikbaarheidsreplica voor de beschikbaarheidsgroep. Zie voor meer informatie, [databaseverificatie serverconfiguratieoptie opgenomen](https://docs.microsoft.com/sql/database-engine/configure-windows/contained-database-authentication-server-configuration-option?view=sql-server-2017).

Deze opdrachten gebruiken om in te stellen van de ingesloten database-server-optie voor verificatie voor elk exemplaar:

  ```sql
  EXEC sp_configure 'contained database authentication', 1
  GO
  RECONFIGURE
  GO
  ```

### <a name="to-add-sql-always-on-hosting-servers"></a>Om toe te voegen SQL Always On servers die als host fungeert

1. Aanmelden bij de Azure Stack-beheerportal als een service-beheerder.

2. Selecteer **Bladeren** &gt; **BEHEERDERSRESOURCES** &gt; **SQL Servers die als host fungeert** &gt; **+ toevoegen**.

   Onder **SQL-Servers die als host fungeert**, kunt u de Resourceprovider van de SQL-Server verbinding met de daadwerkelijke exemplaren van SQL Server, die als back-end van de resourceprovider dienen.

3. Vul het formulier in met de details van de verbinding voor uw SQL Server-exemplaar. Zorg ervoor dat u de FQDN-adres van de Always On-Listener (en optioneel poortnummer.) Geef de informatie voor het account dat u hebt geconfigureerd met sysadmin-bevoegdheden.

4. Controleer de AlwaysOn-beschikbaarheidsgroep ondersteuning in te schakelen voor exemplaren van SQL AlwaysOn-beschikbaarheidsgroep.

   ![Altijd inschakelen](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. Het exemplaar van SQL Always On toevoegen aan een SKU.

   > [!IMPORTANT]
   > Zelfstandige servers kunnen niet worden gecombineerd met AlwaysOn-exemplaren in de dezelfde SKU. Er wordt geprobeerd om typen na het toevoegen van de eerste hosting server, resulteert in een fout.

## <a name="sku-notes"></a>Opmerkingen bij de SKU

SKU's kunt u onderscheid maken tussen service-aanbiedingen. U kunt bijvoorbeeld een SQL Enterprise-exemplaar met de volgende kenmerken hebben:
  
* hoge capaciteit
* hoge prestaties
* hoge beschikbaarheid

SKU's kunnen niet worden toegewezen aan specifieke gebruikers of groepen in deze release.

 SKU's kunnen een uur duren om te worden weergegeven in de portal. Gebruikers kunnen geen een database maken totdat de SKU is volledig gemaakt.

>[!TIP]
>Gebruik een SKU-naam die overeenkomt met beschrijft de mogelijkheden van de servers in de SKU, zoals de capaciteit en prestaties. De naam fungeert als hulpmiddel om gebruikers hun databases implementeren op de juiste SKU te helpen.

Als een best practice, moeten de host-servers in een SKU dezelfde resource- en kenmerken hebben.

## <a name="make-the-sql-databases-available-to-users"></a>De SQL-databases beschikbaar maken voor gebruikers

Maak plannen en aanbiedingen om SQL-databases beschikbaar maken voor gebruikers. Voeg de **Microsoft.SqlAdapter** service aan het plan en maak een nieuw quotum.

## <a name="next-steps"></a>Volgende stappen

[Databases toevoegen](azure-stack-sql-resource-provider-databases.md)
