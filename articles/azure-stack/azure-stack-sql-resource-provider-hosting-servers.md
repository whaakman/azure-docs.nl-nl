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
ms.date: 06/18/2018
ms.author: jeffgilb
ms.openlocfilehash: 183d9479ae18e557b00d0867cad79600145da7bd
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265225"
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>Hosting-servers toevoegen voor de SQL-resourceprovider

U kunt een SQL-exemplaar op een virtuele machine (VM) worden gehost in [Azure Stack](azure-stack-poc.md), of op een virtuele machine buiten uw Azure-Stack-omgeving, zolang de provider voor SQL-bron verbinding kan maken met het exemplaar.

## <a name="overview"></a>Overzicht

De algemene vereisten voor hosting SQL-servers zijn:

* Het SQL-exemplaar moet zijn gereserveerd voor gebruik door de resource provider en werkbelastingen. U kunt een SQL-exemplaar dat wordt gebruikt door een andere gebruiker niet gebruiken. Deze beperking geldt ook voor App-Services.
* De SQL-resourceprovider VM niet is verbonden met het domein en kan alleen verbinding maken via SQL-verificatie.
* U moet een account met voldoende rechten voor gebruik door de resourceprovider configureren.
* De resourceprovider en gebruikers, zoals Web-Apps, gebruikt het gebruikersnetwerk, zodat de verbinding met de SQL-exemplaar op dit netwerk is vereist. Deze vereiste betekent meestal dat de IP-adres voor uw SQL-exemplaren moet op een openbaar netwerk.
* Beheer van de SQL-exemplaren en hun hosts is aan u. Bijvoorbeeld de resourceprovider niet toepassen van updates, back-ups verwerken of verwerken rotatie van referenties.
* U kunt SKU's die ondersteuning bieden voor verschillende soorten SQL-mogelijkheden, zoals de prestaties en hoge beschikbaarheid die AlwaysOn gebruiken.

### <a name="sql-server-virtual-machine-images"></a>Installatiekopieën van virtuele machines van SQL Server

Installatiekopieën van de SQL-IaaS virtuele machines zijn beschikbaar via de Marketplace-Management-functie. Deze installatiekopieën zijn hetzelfde als de SQL-VM's die beschikbaar in Azure zijn.

Zorg ervoor dat u altijd downloaden de nieuwste versie van de **SQL IaaS extensie** voordat u een VM die gebruikmaakt van een Marketplace-item implementeert.  De IaaS-uitbreiding en de bijbehorende portal verbeteringen bieden extra functies zoals automatisch patchen en back-up.

Er zijn andere opties voor het implementeren van virtuele machines SQL, met inbegrip van sjablonen in de [Azure Stack Snelstartgalerie](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Hosting-servers geïnstalleerd op een Azure-Stack met meerdere knooppunten moeten worden gemaakt van een gebruikerabonnement. Ze kunnen niet worden gemaakt van het standaard Provider-abonnement. Ze moeten worden gemaakt vanuit de gebruikersportal of vanuit een PowerShell-sessie met een juiste aanmelding. Alle hosting-servers factureerbare VM's zijn en moeten de juiste SQL-licenties. De servicebeheerder _kunt_ de eigenaar van het abonnement.

### <a name="required-privileges"></a>Vereiste bevoegdheden

Kunt u een gebruiker met beheerdersrechten met minder bevoegdheden die een SQL-sysadmin. De gebruiker moet alleen machtigingen voor de volgende bewerkingen:

- Database: Maken, wijzigen, met Containment (voor altijd op enige), verwijderen, back-up maken
- Beschikbaarheidsgroep: Alter, toevoegen, toevoegen/verwijderen uit Database
- Aanmelding: Maken, selecteert u, Alter, Drop, intrekken
- Selecteer Operations: \[master\].\[ sys\].\[ availability_group_listeners\] (AlwaysOn) uitvoeren op sys.availability_replicas (AlwaysOn), sys.databases, \[master\].\[ sys\].\[ dm_os_sys_memory\], SERVERPROPERTY, \[master\].\[ sys\].\[ availability_groups\] (AlwaysOn) sys.master_files

## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>Een capaciteit door verbinding te maken naar een zelfstandige SQL server die als host fungeert

U kunt zelfstandige (niet-HA) met elke versie van SQL Server 2014 of SQL Server 2016 SQL-servers. Zorg ervoor dat u de referenties voor een account met sysadmin-rechten hebben.

Een zelfstandige host als server wilt toevoegen die al is ingesteld, de volgende stappen uit:

1. Meld u aan de operator Stack Azure portal als servicebeheerder.

2. Selecteer **Bladeren** &gt; **SERVERVIRTUALISATIE** &gt; **SQL Servers die als host fungeert**.

   ![SQL-Servers die als host fungeert](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

   Onder **SQL-Servers die als host fungeert**, kunt u de SQL-resourceprovider verbinden met exemplaren van SQL Server die als back-end van de resourceprovider dienen.

   ![Dashboard van de SQL-Adapter](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. Vul het formulier met de details van de verbinding van uw SQL Server-exemplaar.

   ![Toevoegen van een SQL Server die als host fungeert](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    U kunt eventueel een instantienaam bevatten en een poortnummer opgeven als het exemplaar is niet aan de standaardpoort 1433 toegewezen.

   > [!NOTE]
   > Als het SQL-exemplaar kan worden geopend door de gebruiker en Azure Resource Manager-beheerder, kan deze worden geplaatst onder het beheer van de resourceprovider. Het SQL-exemplaar __moet__ worden toegewezen aan de resourceprovider.

4. Als u servers toevoegt, moet u ze toewijzen aan een nieuwe of bestaande SKU om het te onderscheiden van serviceaanbiedingen. U kunt bijvoorbeeld een exemplaar van SQL Enterprise waarmee hebben:
  
   - capaciteit voor de database
   - Automatische back-up
   - servers met hoge prestaties voor afzonderlijke afdelingen reserveren

   Alle hosting-servers in een SKU moeten dezelfde mogelijkheden hebben. De **naam** moet overeenkomen met de eigenschappen van de SKU zodat gebruikers hun databases op de juiste SKU implementeren kunnen.

   > [!IMPORTANT]
   > Speciale tekens, inclusief spaties en perioden, worden niet ondersteund in de **familie** of **laag** namen bij het maken van een SKU voor de resourceproviders SQL en MySQL.

   Bijvoorbeeld:

   ![SKU maken](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

   >[!NOTE]
   > SKU's kunnen een uur duren zichtbaar in de portal. Gebruikers kunnen geen een database maken totdat de SKU is gemaakt.

## <a name="provide-high-availability-using-sql-always-on-availability-groups"></a>Hoge beschikbaarheid met behulp van SQL AlwaysOn-beschikbaarheidsgroepen

Configureren van SQL Always On exemplaren zijn aanvullende stappen vereist en vereist een minimum van drie virtuele machines (of fysieke machines). In dit artikel wordt ervan uitgegaan dat er al een goed begrip van AlwaysOn-beschikbaarheidsgroepen. Zie voor meer informatie:

* [Inleiding tot SQL Server altijd op beschikbaarheidsgroepen op virtuele machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)
* [Altijd op beschikbaarheidsgroepen (SQL Server)](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2017)

> [!NOTE]
> De resourceprovider voor SQL-adapter _alleen_ ondersteunt SQL 2016 SP1 Enterprise of later instanties voor Always On. De configuratie van deze adapter vereist een nieuwe SQL-functies, zoals automatische seeding.

Naast de voorgaande lijst met vereisten moet u inschakelen [automatische Seeding](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) op elke beschikbaarheidsgroep voor elk exemplaar van SQL Server.

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

   Onder **SQL-Servers die als host fungeert** kunt u de SQL Server-Resourceprovider verbinden met de daadwerkelijke exemplaren van SQL Server die als back-end van de resourceprovider dienen.

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
