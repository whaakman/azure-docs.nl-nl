---
title: SQL-Servers op Azure Stack Hosting | Microsoft Docs
description: Het toevoegen van de SQL-exemplaren om in te richten via de bronprovider van SQL-Adapter.
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
ms.date: 06/27/2018
ms.author: jeffgilb
ms.openlocfilehash: af820f90c5d8822dbdaa768b16360d534fd47828
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060039"
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>Hosting-servers toevoegen voor de SQL-resourceprovider

U kunt een SQL-exemplaar op een virtuele machine (VM) worden gehost in [Azure Stack](azure-stack-poc.md), of op een virtuele machine buiten uw Azure-Stack-omgeving, zolang de provider voor SQL-bron verbinding kan maken met het exemplaar.

## <a name="overview"></a>Overzicht

Voordat u een SQL server die als host fungeert toevoegt, controleert u de volgende verplichte en algemene vereisten.

**Verplichte vereisten**

* SQL-verificatie inschakelen op de SQL Server-exemplaar. Omdat de SQL-resourceprovider VM niet is verbonden met het domein, kan deze alleen verbinding maken met een hostserver via SQL-verificatie.
* Configureer de IP-adressen voor de SQL-exemplaren als openbaar. De resourceprovider en gebruikers, zoals Web-Apps, communiceren via het gebruikersnetwerk, zodat de verbinding met de SQL-exemplaar op dit netwerk is vereist.

**Algemene vereisten**

* Toe te wijzen aan het SQL-exemplaar voor gebruik door de resource provider en werkbelastingen. U kunt een SQL-exemplaar dat wordt gebruikt door een andere gebruiker niet gebruiken. Deze beperking geldt ook voor App-Services.
* Configureer een account met het niveau van de juiste bevoegdheden voor de resourceprovider.
* U bent zelf verantwoordelijk voor het beheren van de SQL-exemplaren en hun hosts.  Bijvoorbeeld de resourceprovider niet toepassen van updates, back-ups verwerken of verwerken rotatie van referenties.

### <a name="sql-server-virtual-machine-images"></a>Installatiekopieën van virtuele machines van SQL Server

Installatiekopieën van de SQL-IaaS virtuele machines zijn beschikbaar via de Marketplace-Management-functie. Deze installatiekopieën zijn hetzelfde als de SQL-VM's die beschikbaar in Azure zijn.

Zorg ervoor dat u altijd downloaden de nieuwste versie van de **SQL IaaS extensie** voordat u een SQL-VM met een Marketplace-item implementeren. De IaaS-uitbreiding en de bijbehorende portal verbeteringen bieden extra functies zoals automatisch patchen en back-up. Zie voor meer informatie over deze extensie [automatiseren beheertaken op Azure Virtual Machines met de SQL Server Agent-extensie](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension).

Er zijn andere opties voor het implementeren van virtuele machines SQL, met inbegrip van sjablonen in de [Azure Stack Snelstartgalerie](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Hosting-servers geïnstalleerd op een Azure-Stack met meerdere knooppunten moeten worden gemaakt van een gebruikerabonnement. Ze kunnen niet worden gemaakt van het standaard Provider-abonnement. Ze moeten worden gemaakt vanuit de gebruikersportal of vanuit een PowerShell-sessie met een juiste aanmelding. Alle hosting-servers factureerbare VM's zijn en moeten de juiste SQL-licenties. De servicebeheerder _kunt_ de eigenaar van het abonnement.

### <a name="required-privileges"></a>Vereiste bevoegdheden

U kunt een gebruiker met beheerdersrechten maken met lager is dan een SQL-sysadmin-bevoegdheden. De gebruiker moet alleen machtigingen voor de volgende bewerkingen:

* Database: Maken, wijzigen, met Containment (voor altijd op enige), verwijderen, back-up maken
* Beschikbaarheidsgroep: Alter, toevoegen, toevoegen/verwijderen uit Database
* Aanmelding: Maken, selecteert u, Alter, Drop, intrekken
* Selecteer Operations: \[master\].\[ sys\].\[ availability_group_listeners\] (AlwaysOn) uitvoeren op sys.availability_replicas (AlwaysOn), sys.databases, \[master\].\[ sys\].\[ dm_os_sys_memory\], SERVERPROPERTY, \[master\].\[ sys\].\[ availability_groups\] (AlwaysOn) sys.master_files

## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>Een capaciteit door verbinding te maken naar een zelfstandige SQL server die als host fungeert

U kunt zelfstandige (niet-HA) met elke versie van SQL Server 2014 of SQL Server 2016 SQL-servers. Zorg ervoor dat u de referenties voor een account met sysadmin-rechten hebben.

Een zelfstandige host als server wilt toevoegen die al ingesteld, de volgende stappen uit:

1. Meld u aan de operator Stack Azure portal als servicebeheerder.

2. Selecteer **Bladeren** &gt; **SERVERVIRTUALISATIE** &gt; **SQL Servers die als host fungeert**.

   ![SQL-Servers die als host fungeert](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

   Onder **SQL-Servers die als host fungeert**, kunt u de SQL-resourceprovider verbinden met exemplaren van SQL Server die als back-end van de resourceprovider dienen.

   ![Dashboard van de SQL-Adapter](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. Op **toevoegen van een SQL-Server die als host fungeert**, bieden de verbindingsdetails voor uw SQL Server-exemplaar.

   ![Toevoegen van een SQL Server die als host fungeert](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    Desgewenst de exemplaarnaam van een opgeven en geef een poortnummer als het exemplaar is niet aan de standaardpoort 1433 toegewezen.

   > [!NOTE]
   > Als het SQL-exemplaar kan worden geopend door de gebruiker en Azure Resource Manager-beheerder, kan deze worden geplaatst onder het beheer van de resourceprovider. Het SQL-exemplaar __moet__ worden toegewezen aan de resourceprovider.

4. Als u servers toevoegt, moet u deze toewijzen aan een bestaande SKU of maak een nieuwe SKU. Onder **toevoegen van een Server die als host fungeert**, selecteer **SKU's**.

   * Kies een beschikbare SKU voor het gebruik van een bestaande SKU, en selecteer vervolgens **maken**.
   * Selecteer voor het maken van een SKU **+ maken van nieuwe SKU**. In **SKU maken**, voer de vereiste gegevens in en selecteer vervolgens **OK**.

     > [!IMPORTANT]
     > Speciale tekens, inclusief spaties en perioden, worden niet ondersteund **naam** veld. Gebruik de voorbeelden in de volgende schermopname Voer waarden in voor de **familie**, **laag**, en **Edition** velden.

     ![Maken van een SKU](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

      SKU's kunnen een uur duren zichtbaar in de portal. Gebruikers kunnen geen een database maken totdat de SKU is gemaakt.

### <a name="sku-notes"></a>Opmerkingen bij de SKU

U kunt SKU's gebruiken om te onderscheiden van serviceaanbiedingen. U kunt bijvoorbeeld een Enterprise van SQL-exemplaar met de volgende kenmerken hebben:
  
* hoge capaciteit
* hoge prestaties
* Hoge beschikbaarheid

U kunt een SKU voor het vorige voorbeeld beperken van toegang tot specifieke groepen die een hoge prestaties database moet maken.

>[!TIP]
>Gebruik een SKU-naam die overeenkomt met beschrijft de mogelijkheden van de servers in de SKU, zoals de capaciteit en prestaties. De naam van de fungeert als hulpmiddel om gebruikers hun databases implementeren op de juiste SKU.

Als een best practice moeten alle hosting-servers in een SKU hebben dezelfde kenmerken resource en prestaties.

## <a name="provide-high-availability-using-sql-always-on-availability-groups"></a>Hoge beschikbaarheid met behulp van SQL AlwaysOn-beschikbaarheidsgroepen

Configuratie-exemplaren van SQL Always On zijn aanvullende stappen vereist en vereist drie virtuele machines (of fysieke machines). In dit artikel wordt ervan uitgegaan dat er al een goed begrip van AlwaysOn-beschikbaarheidsgroepen. Raadpleeg voor meer informatie de volgende artikelen:

* [Inleiding tot SQL Server altijd op beschikbaarheidsgroepen op virtuele machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)
* [Altijd op beschikbaarheidsgroepen (SQL Server)](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2017)

> [!NOTE]
> De resourceprovider voor SQL-adapter _alleen_ ondersteunt SQL 2016 SP1 Enterprise of later instanties voor Always On. De configuratie van deze adapter vereist een nieuwe SQL-functies, zoals automatische seeding.

Bovendien moet u inschakelen [automatische Seeding](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) op elke beschikbaarheidsgroep voor elk exemplaar van SQL Server.

Om automatische seeding op alle instanties, bewerken en voer de volgende SQL-opdracht voor elk exemplaar:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>]
      MODIFY REPLICA ON 'InstanceName'
      WITH (SEEDING_MODE = AUTOMATIC)
  GO
  ```

Op de secundaire exemplaren bewerken en voer vervolgens de volgende SQL-opdracht voor elk exemplaar:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
  GO
  ```

### <a name="to-add-sql-always-on-hosting-servers"></a>SQL Always On hosting-servers toevoegen

1. Aanmelden bij de Azure-Stack-beheerportal als een service-beheerder.

2. Selecteer **Bladeren** &gt; **SERVERVIRTUALISATIE** &gt; **SQL Servers die als host fungeert** &gt; **+toevoegen**.

   Onder **SQL-Servers die als host fungeert**, kunt u de SQL Server-Resourceprovider verbinden met de daadwerkelijke exemplaren van SQL Server die als back-end van de resourceprovider dienen.

3. Vul het formulier met de verbindingsdetails voor uw SQL Server-exemplaar. Zorg ervoor dat u de FQDN-adres van de altijd op de Listener (en optioneel poortnummer.) Geef de gegevens voor het account dat u hebt geconfigureerd met sysadmin-bevoegdheden.

4. Controleer de AlwaysOn-beschikbaarheidsgroep ondersteuning in te schakelen voor exemplaren van SQL AlwaysOn-beschikbaarheidsgroep.

   ![Altijd inschakelen](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. Het exemplaar van SQL Always On toevoegen aan een SKU.

   > [!IMPORTANT]
   > U kunt zelfstandige servers niet samen met AlwaysOn-exemplaren in de dezelfde SKU. Wilt mix typen na het toevoegen van de eerste hosting server, resulteert in een fout.

## <a name="make-the-sql-databases-available-to-users"></a>De SQL-databases beschikbaar stellen aan gebruikers

Maak plannen en aanbiedingen om SQL-databases beschikbaar voor gebruikers. Voeg de **Microsoft.SqlAdapter** service aan het plan, en de standaard quotum toevoegen of een nieuwe Quota maken.

![Plannen en aanbiedingen voor zijn onder meer databases maken](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)

## <a name="next-steps"></a>Volgende stappen

[Databases toevoegen](azure-stack-sql-resource-provider-databases.md)
