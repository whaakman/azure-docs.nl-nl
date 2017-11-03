---
title: SQL-Servers op Azure Stack Hosting | Microsoft Docs
description: SQL-exemplaren om in te richten via de Bronprovider van de SQL-Adapter toevoegen
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: JeffGo
ms.openlocfilehash: 58c83b74041e0e2e82729f569c53aca59f3aed43
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="add-hosting-servers-for-use-by-the-sql-adapter"></a>Hosting-servers voor gebruik door de SQL-adapter toevoegen

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt SQL-exemplaren op virtuele machines in uw [Azure Stack](azure-stack-poc.md), of een exemplaar buiten uw Azure-Stack-omgeving, opgegeven van de resourceprovider verbinding mee kan maken. De algemene vereisten zijn:

* Het SQL-exemplaar moet zijn gereserveerd voor gebruik door de werkbelastingen RP en de gebruiker. U kunt een SQL-exemplaar dat wordt gebruikt door andere gebruiker, met inbegrip van App-Services niet gebruiken.
* De RP-adapter is niet verbonden met het domein en kan alleen verbinding maken via SQL-verificatie.
* U moet een account met voldoende rechten voor gebruik door de RP configureren.
* Netwerkverkeer van de RP aan SQL-poort 1433 gebruikt en kan niet worden gewijzigd.
* De RP en gebruikers, zoals Web-Apps gebruiken het gebruikersnetwerk, zodat de verbinding met de SQL-exemplaar op dit netwerk is vereist. Deze vereiste betekent meestal dat de IP-adres voor uw SQL-exemplaren moet op een openbaar netwerk.
* Beheer van de SQL-exemplaren en hun hosts is aan u; het RP niet uitvoeren van de toepassing van patches, back-up, referenties wisselen, enzovoort.
* SKU's kunnen worden gebruikt voor het maken van verschillende klassen van de SQL-mogelijkheden, zoals prestaties altijd actief, enzovoort.



Een aantal SQL IaaS virtuele machine afbeeldingen zijn beschikbaar via de Marketplace-Management-functie. Zorg ervoor dat u altijd de nieuwste versie van de SQL-IaaS-uitbreiding downloaden voordat u een VM die gebruikmaakt van een Marketplace-item implementeert. De SQL-afbeeldingen zijn hetzelfde als de SQL-VM's die beschikbaar in Azure zijn. Geef functies zoals automatisch patchen en Backup-functies voor virtuele SQL-machines gemaakt op basis van deze installatiekopieën, de IaaS-uitbreiding en de bijbehorende portal verbeteringen.

Er zijn andere opties voor het implementeren van virtuele machines SQL, met inbegrip van sjablonen in de [Azure Stack Snelstartgalerie](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Hosting-servers geïnstalleerd op een Azure-Stack met meerdere knooppunten moeten worden gemaakt van een gebruikerabonnement. Ze kunnen niet worden gemaakt van het standaard Provider-abonnement. Ze moeten worden gemaakt vanuit de gebruikersportal of vanuit een PowerShell-sessie met een juiste aanmelding. Alle hosting-servers toerekenbare VM's zijn en moeten de juiste SQL-licenties. De servicebeheerder _kunt_ de eigenaar van het abonnement.


### <a name="required-privileges"></a>Vereiste bevoegdheden

U kunt een nieuwe gebruiker met beheerdersrechten maken met minder dan de volledige sysadmin-bevoegdheden. De specifieke bewerkingen die moeten worden toegestaan zijn:

- Database: Maken, wijzigen met Containment (altijd op alleen), Drop, back-up
- Beschikbaarheidsgroep: Alter, toevoegen, toevoegen/verwijderen uit Database
- Aanmelding: Maken, selecteert u, Alter, Drop, intrekken
- Selecteer Operations: \[master\].\[ sys\].\[ availability_group_listeners\] (AlwaysOn) uitvoeren op sys.availability_replicas (AlwaysOn), sys.databases, \[master\].\[ sys\].\[ dm_os_sys_memory\], SERVERPROPERTY, \[master\].\[ sys\].\[ availability_groups\] (AlwaysOn) sys.master_files



## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>Een capaciteit door verbinding te maken naar een zelfstandige SQL server die als host fungeert
U kunt zelfstandige (niet-HA) met elke versie van SQL Server 2014 of SQL Server 2016 SQL-servers. Zorg ervoor dat u hebt de referenties voor een account met beheerdersbevoegdheden system.

Als u wilt toevoegen in een zelfstandige host server die al is ingericht, de volgende stappen uit:

1. Aanmelden bij de Azure-Stack-beheerportal als servicebeheerder

2. Klik op **Bladeren** &gt; **SERVERVIRTUALISATIE** &gt; **SQL Servers die als host fungeert**.

  ![](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

  De **SQL-Servers die als host fungeert** blade is waar u de Resource Provider voor SQL Server kunt verbinden met de daadwerkelijke exemplaren van SQL Server die als back-end van de resourceprovider dienen.

  ![Hosting-Servers](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. Vul het formulier met de details van de verbinding van uw SQL Server-exemplaar.

  ![Nieuwe hostserver](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

  > [!NOTE]
  > Als het SQL-exemplaar kan worden geopend door de gebruiker en Azure Resource Manager-beheerder, kan deze worden geplaatst onder het beheer van de resourceprovider. Het SQL-exemplaar __moet__ worden toegewezen aan de RP uitsluitend.

4. Als u servers toevoegt, moet u ze toewijzen aan een nieuwe of bestaande SKU om het te onderscheiden van serviceaanbiedingen. U kunt bijvoorbeeld hebben een Enterprise van SQL-exemplaar leveren:
  - capaciteit voor de database
  - Automatische back-up
  - servers met hoge prestaties voor afzonderlijke afdelingen reserveren
  - enzovoort.

  De SKU-naam moet overeenkomen met de eigenschappen zodat gebruikers hun databases op de juiste wijze kunnen plaatsen. Alle hosting-servers in een SKU moeten dezelfde mogelijkheden hebben.

    Een voorbeeld:

    ![SKU 's](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

>[!NOTE]
SKU's kunnen een uur duren zichtbaar in de portal. U kunt een database kan niet maken, totdat de SKU is gemaakt.

## <a name="provide-capacity-using-sql-always-on-availability-groups"></a>Een capaciteit met behulp van SQL AlwaysOn-beschikbaarheidsgroepen
Configuratie-exemplaren van SQL Always On zijn aanvullende stappen vereist en moet ten minste drie virtuele machines (of fysieke machines).

> [!NOTE]
> De SQL-adapter RP _alleen_ ondersteunt SQL 2016 SP1 Enterprise of hoger exemplaren voor Always On omdat hiervoor een nieuwe SQL-functies, zoals automatische seeding. Naast de voorgaande algemene lijst met vereisten:

* U moet een bestandsserver naast de SQL Always On computers opgeven. Er is een [snelstartsjabloon met de Azure-Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2016-ha) die voor u deze omgeving kunt maken. Het kan ook fungeren als een handleiding voor het bouwen van uw eigen exemplaar.

* U moet de SQL-servers instellen. U moet in het bijzonder [automatische Seeding](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) op elke beschikbaarheidsgroep voor elk exemplaar van SQL Server.

```
ALTER AVAILABILITY GROUP [<availability_group_name>]
    MODIFY REPLICA ON 'InstanceName'
    WITH (SEEDING_MODE = AUTOMATIC)
GO
```

Op secundaire exemplaren
```
ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
GO

```



SQL Always On hosting als servers wilt toevoegen, de volgende stappen uit:

1. Aanmelden bij de Azure-Stack-beheerportal als een servicebeheerder

2. Klik op **Bladeren** &gt; **SERVERVIRTUALISATIE** &gt; **SQL Servers die als host fungeert** &gt; **+toevoegen**.

    De **SQL-Servers die als host fungeert** blade is waar u de Resource Provider voor SQL Server kunt verbinden met de daadwerkelijke exemplaren van SQL Server die als back-end van de resourceprovider dienen.


3. Vul het formulier met de details van de verbinding van uw exemplaar van SQL Server, zorg ervoor dat u de FQDN-naam of IPv4-adres van de altijd op Listener wordt. Geef de accountgegevens voor de account die u hebt geconfigureerd met een systeemaccount van de beheerder.

4. Schakel dit selectievakje in als ondersteuning voor exemplaren van SQL AlwaysOn-beschikbaarheidsgroep wilt inschakelen.

    ![Hosting-Servers](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. Het exemplaar van SQL Always On toevoegen aan een SKU. U kunt zelfstandige servers niet samen met AlwaysOn-exemplaren in de dezelfde SKU. Die wordt bepaald wanneer u de eerste hostserver toevoegt. Bij mix typen daarna het leidt tot een fout opgetreden.


## <a name="making-sql-databases-available-to-users"></a>SQL-databases maken beschikbaar voor gebruikers

Maak plannen en aanbiedingen om SQL-databases beschikbaar voor gebruikers. De service Microsoft.SqlAdapter toevoegen aan het plan en toevoegen van een bestaande quotum of maak een nieuwe. Als u een quotum maakt, kunt u de capaciteit om de gebruiker.

![Plannen en aanbiedingen voor zijn onder meer databases maken](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)

## <a name="maintenance-of-the-sql-adapter-rp"></a>Onderhoud van de SQL-Adapter RP

Onderhoud van de SQL-exemplaren wordt niet hier, met uitzondering van de draaihoek wachtwoordgegevens besproken. U bent zelf verantwoordelijk voor patchen en back-up/herstel van de database-servers gebruikt met de SQL-Adapter.

### <a name="patching-and-updating"></a>Patchen en bijwerken
 De SQL-Adapter is niet verwerkt als onderdeel van Azure-Stack omdat deze een onderdeel van de invoegtoepassing. Microsoft zal updates leveren aan de SQL-Adapter indien nodig. De SQL-Adapter is gemaakt op een _gebruiker_ virtuele machine onder de standaard Provider-abonnement. Daarom is het nodig zijn voor het bieden van Windows-patches, antivirus handtekeningen, enzovoort. De Windows update-pakketten die worden geleverd als onderdeel van de patch-en updatecyclus updates toepassen op de virtuele machine van Windows kan worden gebruikt. Wanneer een bijgewerkte adapter wordt uitgebracht, worden een script is opgegeven voor de update toepassen. Dit script maakt een nieuwe RP VM en migreer elke status die u al hebt.

 ### <a name="backuprestoredisaster-recovery"></a>Backup/Restore/noodherstel
 De SQL-Adapter is geen back-up als onderdeel van Azure Stack BC-DR-proces, omdat deze een onderdeel van de invoegtoepassing. Scripts wordt aangeboden mogelijk te maken:
- Een back-up van de van de benodigde statusinformatie (opgeslagen in een Stack van Azure storage-account)
- Het RP herstellen mocht het volledige stack herstel niet meer nodig.
Database-servers moeten eerst worden hersteld (indien nodig), voordat de RP is hersteld.

### <a name="updating-sql-credentials"></a>SQL-referenties bijwerken

U bent zelf verantwoordelijk voor het maken en onderhouden van de beheerdersaccounts systeem op uw SQL-servers. Het RP moet een account met deze rechten voor het beheren van databases namens gebruikers - hoeft niet toegang tot de gegevens in deze databases. Als u bijwerken van de sa-wachtwoorden op de SQL-servers wilt, kunt u de mogelijkheid van de update van de RP-beheerinterface het opgeslagen wachtwoord gebruikt door de RP te wijzigen. Deze wachtwoorden worden opgeslagen in een Sleutelkluis voor uw Azure-Stack-exemplaar.

Om de instellingen wijzigt, klikt u op **Bladeren** &gt; **SERVERVIRTUALISATIE** &gt; **SQL-Servers die als host fungeert** &gt; **SQL-aanmeldingen** en selecteert u een aanmeldingsnaam. De wijziging moet eerst op de SQL-exemplaar worden gemaakt (en alle replica's, indien nodig). In de **instellingen** -scherm, klikt u op **wachtwoord**.

![Het beheerderswachtwoord bijwerken](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)


## <a name="next-steps"></a>Volgende stappen

[Databases toevoegen](azure-stack-sql-resource-provider-databases.md)
