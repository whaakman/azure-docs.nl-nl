---
title: Azure SQL Database en datawarehouse-firewallregels | Microsoft Docs
description: Informatie over het configureren van een SQL-database en SQL Data Warehouse-firewall met firewallregels op serverniveau voor het beheren van toegang en SQL Database-firewallregels op databaseniveau configureren.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
manager: craigg
ms.date: 10/19/2018
ms.openlocfilehash: 18cd0ea4b6804edc11fc4f57c4327ef9d9a6f51e
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466730"
---
# <a name="azure-sql-database-and-sql-data-warehouse-firewall-rules"></a>Azure SQL-Database en SQL Data Warehouse firewall-regels

Microsoft Azure [SQL-Database](sql-database-technical-overview.md) en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) een relationele databaseservice voor Azure en andere toepassingen op basis van het Internet. Om uw gegevens te beschermen, verhinderen firewalls alle toegang tot de databaseserver totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.

> [!NOTE]
> Dit onderwerp is van toepassing op Azure SQL-servers en op SQL Database- en SQL Data Warehouse-databases die op deze Azure SQL-servers worden gemaakt. Voor het gemak wordt de term 'SQL Database' gebruikt wanneer er wordt verwezen naar zowel SQL Database als SQL Data Warehouse.

## <a name="virtual-network-rules-as-alternatives-to-ip-rules"></a>Regels voor virtueel netwerk als alternatief voor IP-regels

Naast de IP-regels, beheert de firewall ook *virtuele netwerkregels*. Virtual network-regels zijn gebaseerd op Virtual Network-service-eindpunten. Regels voor virtueel netwerk is mogelijk beter aan IP-regels in sommige gevallen. Zie voor meer informatie, [Virtual Network-service-eindpunten en regels voor Azure SQL Database](sql-database-vnet-service-endpoint-rule-overview.md).

## <a name="overview"></a>Overzicht

In eerste instantie wordt alle Transact-SQL-toegang tot uw Azure SQL-server door de firewall geblokkeerd. Als u wilt beginnen met uw Azure SQL-server, moet u een of meer firewallregels op serverniveau regels waarmee toegang tot uw Azure SQL-server opgeven. Gebruik de firewallregels om op te geven welke IP-adresbereiken van internet zijn toegestaan en of Azure-toepassingen kunnen proberen verbinding te maken met uw Azure SQL-server.

Als u selectief toegang wilt verlenen tot slechts een van de databases op uw Azure SQL-server, moet u voor de vereiste database een regel op databaseniveau maken. Geef voor de firewallregel van de database een IP-adresbereik op dat buiten het IP-adresbereik ligt dat is opgegeven in de firewallregel op serverniveau. Het IP-adres van de client moet binnen het bereik liggen dat is opgegeven in de regel op databaseniveau.

> [!IMPORTANT]
> SQL Data Warehouse wordt alleen ondersteuning biedt voor firewallregels op serverniveau en biedt geen ondersteuning voor firewallregels op databaseniveau.

Verbindingspogingen van internet en Azure moeten eerst de firewall passeren voordat ze de Azure SQL-server of SQL Database kunnen bereiken, zoals in het volgende diagram wordt weergegeven:

   ![Diagram met beschrijving van firewallconfiguratie.][1]

- **Firewallregels op serverniveau:**

  Deze regels zorgen ervoor dat clients toegang krijgen tot de hele Azure SQL-server, dat wil zeggen, alle databases binnen dezelfde logische server. Deze regels worden opgeslagen in de **hoofd**database. Firewallregels op serverniveau kunnen worden geconfigureerd met behulp van de portal of met behulp van Transact-SQL-instructies. Als u firewallregels op serverniveau wilt maken met Azure Portal of PowerShell, moet u de eigenaar van het abonnement of een bijdrager aan het abonnement zijn. Als u een firewallregel op serverniveau wilt maken met Transact-SQL, moet u verbinding maken met het SQL Database-exemplaar als de hoofdaanmelding op serviceniveau of als de beheerder van Azure Active Directory (wat betekent dat er eerst een firewallregel op serverniveau moet worden gemaakt door een gebruiker met machtigingen op Azure-niveau).

- **Firewallregels op databaseniveau:**

  Deze regels zorgen ervoor dat clients toegang krijgen tot bepaalde (beveiligde) databases binnen dezelfde logische server. U kunt deze regels voor elke database maken (met inbegrip van de **master** database) en ze worden opgeslagen in de afzonderlijke databases. Firewallregels op databaseniveau voor hoofd- en gebruikersdatabases kunnen alleen worden gemaakt en beheerd met behulp van Transact-SQL-instructies en alleen nadat u de eerste firewall op serverniveau hebt geconfigureerd. Als u in de firewallregel op databaseniveau een IP-adresbereik opgeeft dat buiten het bereik ligt dat is opgegeven in de firewallregel op serverniveau, kunnen alleen clients met IP-adressen in het bereik op databaseniveau toegang krijgen tot de database. U kunt voor een database maximaal 128 firewallregels op databaseniveau opgeven. Zie het voorbeeld verderop in dit artikel en Zie voor meer informatie over het configureren van firewallregels op databaseniveau [sp_set_database_firewall_rule (Azure SQL Database)](https://msdn.microsoft.com/library/dn270010.aspx).

### <a name="recommendation"></a>Aanbeveling

Microsoft raadt u aan met behulp van firewallregels op databaseniveau indien mogelijk om beveiliging te verbeteren en uw database draagbaarder te maken. Gebruik firewallregels op serverniveau voor beheerders en wanneer u veel databases met dezelfde toegangsvereisten hebt en u niet elke database afzonderlijk wilt configureren.

> [!Important]
> Windows Azure SQL Database ondersteunt maximaal 128 firewallregels.
> [!Note]
> Voor meer informatie over draagbare databases in de context van bedrijfscontinuïteit raadpleegt u [Authentication requirements for disaster recovery](sql-database-geo-replication-security-config.md) (Verificatievereisten voor herstel na noodgevallen).

### <a name="connecting-from-the-internet"></a>Verbinding maken vanuit internet

Wanneer een computer via internet verbinding probeert te maken met de databaseserver, controleert de firewall eerst het oorspronkelijke IP-adres van de aanvraag aan de hand van de firewallregels voor de database waarmee verbinding moet worden gemaakt:

- Als het IP-adres van de aanvraag binnen een van de bereiken ligt die zijn opgegeven in de firewallregel op databaseniveau, wordt de verbinding met de SQL-database die de regel bevat, toegestaan.
- Als het IP-adres van de aanvraag niet binnen een van de bereiken ligt die zijn opgegeven in de firewallregel op databaseniveau, worden de firewallregels op serverniveau gecontroleerd. Als het IP-adres van de aanvraag binnen een van de bereiken ligt die zijn opgegeven in de firewallregels op serverniveau, wordt de verbinding toegestaan. Firewallregels op serverniveau zijn van toepassing op alle SQL-databases op de Azure SQL-server.  
- Als het IP-adres van de aanvraag niet binnen de bereiken die is opgegeven in een van de firewallregels op databaseniveau of op serverniveau, mislukt de verbindingsaanvraag.

> [!NOTE]
> Voor toegang tot Azure SQL Database vanuit de lokale computer moet u ervoor zorgen dat de firewall op uw netwerk en lokale computer uitgaande communicatie op TCP-poort 1433 toestaat.

### <a name="connecting-from-azure"></a>Verbinding maken vanuit Azure

Azure-verbindingen moeten zijn ingeschakeld opdat toepassingen vanuit Azure verbinding kunnen maken met uw Azure SQL-server. Wanneer een toepassing vanuit Azure probeert verbinding te maken met uw databaseserver, verifieert de firewall of Azure-verbindingen zijn toegestaan. Een firewallinstelling waarvan het begin- en eindadres gelijk zijn aan 0.0.0.0 geeft aan dat deze verbindingen zijn toegestaan. Als de verbindingspoging niet is toegestaan, zal de aanvraag de Azure SQL Database-server niet bereiken.

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.

## <a name="creating-and-managing-firewall-rules"></a>Het maken en beheren van firewall-regels

De eerste firewallinstelling op serverniveau kan worden gemaakt met de [Azure-portal](https://portal.azure.com/) of programmatisch met [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql), [Azure CLI](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create), of de [ REST-API](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate). Volgende firewallregels op serverniveau kunt u maken en beheren met deze methoden en via Transact-SQL.

> [!IMPORTANT]
> Firewallregels op databaseniveau kunnen alleen worden gemaakt en beheerd met behulp van Transact-SQL.

Voor betere prestaties worden firewallregels op serverniveau tijdelijk in de cache op databaseniveau opgeslagen. Zie [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx) als u de cache wilt vernieuwen.

> [!TIP]
> U kunt [SQL Database Auditing](sql-database-auditing.md) om te controleren van wijzigingen van de firewall op serverniveau en databaseniveau.

## <a name="manage-firewall-rules-using-the-azure-portal"></a>Firewallregels beheren met behulp van de Azure-portal

Om een firewallregel op serverniveau in Azure portal, kunt u ofwel gaan naar de pagina overzicht voor uw Azure SQL database of de overzichtspagina voor uw logische Azure-databaseserver.

> [!TIP]
> Zie voor een zelfstudie [maken van een database met behulp van de Azure-portal](sql-database-get-started-portal.md).

### <a name="from-database-overview-page"></a>Overzichtspagina voor database

1. Klik op de overzichtspagina voor de database te een firewallregel op serverniveau **serverfirewall instellen** op de werkbalk zoals weergegeven in de volgende afbeelding: de **Firewall-instellingen** pagina voor de SQL-Database-server wordt geopend.

      ![serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png)

2. Klik op **IP van client toevoegen** op de werkbalk om het toevoegen van het IP-adres van de computer u momenteel gebruikt en klik vervolgens op **opslaan**. Er wordt een firewallregel op serverniveau gemaakt voor uw huidige IP-adres.

      ![serverfirewallregel instellen](./media/sql-database-get-started-portal/server-firewall-rule-set.png)

### <a name="from-server-overview-page"></a>Overzichtspagina voor server

De overzichtspagina voor uw server wordt geopend, met de volledig gekwalificeerde servernaam (zoals **mynewserver20170403.database.windows.net**) en opties voor verdere configuratie.

1. Als een regel op serverniveau overzichtspagina voor de server, klikt u op **Firewall** in het linkermenu onder instellingen:

2. Klik op **IP van client toevoegen** op de werkbalk om het toevoegen van het IP-adres van de computer u momenteel gebruikt en klik vervolgens op **opslaan**. Er wordt een firewallregel op serverniveau gemaakt voor uw huidige IP-adres.

## <a name="manage-firewall-rules-using-transact-sql"></a>Firewallregels beheren met behulp van Transact-SQL

| Catalogusweergave of opgeslagen procedure | Niveau | Beschrijving |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Server |Geeft de huidige firewallregels op serverniveau weer |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Server |Hiermee worden de firewallregels op serverniveau gemaakt of bijgewerkt |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Server |Verwijdert firewallregels op serverniveau |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Database |Geeft de huidige firewallregels op databaseniveau weer |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Database |Hiermee worden de firewallregels op databaseniveau gemaakt of bijgewerkt |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Databases |Verwijdert firewallregels op databaseniveau |

Controleer de bestaande regels, een bereik van IP-adressen op de Contoso-server inschakelen en Hiermee verwijdert u een firewallregel in de volgende voorbeelden:

```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```

Voeg daarna een firewallregel toe.

```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

Als u een firewallregel op serverniveau wilt verwijderen, voert u de opgeslagen procedure sp_delete_firewall_rule uit. Het volgende voorbeeld wordt de regel contosofirewallrule verwijderd:

```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```

## <a name="manage-firewall-rules-using-azure-powershell"></a>Firewallregels beheren met behulp van Azure PowerShell

| Cmdlet | Niveau | Beschrijving |
| --- | --- | --- |
| [Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule) |Server |Retourneert de huidige firewallregels op serverniveau |
| [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) |Server |Maakt een nieuwe firewallregel op serverniveau |
| [Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule) |Server |Werkt de eigenschappen van een bestaande firewallregel op serverniveau bij |
| [Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule) |Server |Verwijdert firewallregels op serverniveau |

Het volgende voorbeeld wordt een firewall op serverniveau-regel met behulp van PowerShell:

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
```

> [!TIP]
> Zie voor PowerShell-voorbeelden in de context van een snel starten, [DB maken - PowerShell](sql-database-powershell-samples.md) en [een individuele database maken en configureren van een firewall-regel met behulp van PowerShell](scripts/sql-database-create-and-configure-database-powershell.md)

## <a name="manage-firewall-rules-using-azure-cli"></a>Firewallregels beheren met behulp van Azure CLI

| Cmdlet | Niveau | Beschrijving |
| --- | --- | --- |
|[AZ sql server firewall-regel maken](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create)|Server|Hiermee maakt u een serverfirewallregel|
|[AZ sql server firewall-rule list](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-list)|Server|Geeft een lijst van de firewall-regels op een server|
|[AZ sql server firewall-rule show](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-show)|Server|Geeft de details van een firewall-regel|
|[AZ sql server firewall-rule update](/cli/azure/sql/server/firewall-rule##az-sql-server-firewall-rule-update)|Server|Een firewallregel bijgewerkt|
|[AZ sql server firewall-rule delete](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-delete)|Server|Hiermee verwijdert u een firewall-regel|

Het volgende voorbeeld wordt een firewall op serverniveau-regel met de Azure CLI:

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
-n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Zie voor een Azure CLI-voorbeeld in de context van een snel starten, [DB maken - Azure CLI](sql-database-cli-samples.md) en [een individuele database maken en configureren van een firewallregel met behulp van de Azure CLI](scripts/sql-database-create-and-configure-database-cli.md)

## <a name="manage-firewall-rules-using-rest-api"></a>Firewallregels beheren met behulp van REST-API

| API | Niveau | Beschrijving |
| --- | --- | --- |
| [List Firewall Rules](https://docs.microsoft.com/rest/api/sql/firewallrules/listbyserver) (Lijst met firewallregels) |Server |Geeft de huidige firewallregels op serverniveau weer |
| [Firewallregels maken of bijwerken](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate) |Server |Hiermee worden de firewallregels op serverniveau gemaakt of bijgewerkt |
| [Delete Firewall Rule](https://docs.microsoft.com/rest/api/sql/firewallrules/delete) (Firewallregel verwijderen) |Server |Verwijdert firewallregels op serverniveau |
| [Firewallregels ophalen](https://docs.microsoft.com/rest/api/sql/firewallrules/get) | Server | Firewallregels op serverniveau opgehaald |

## <a name="server-level-firewall-rule-versus-a-database-level-firewall-rule"></a>Firewallregel op serverniveau ten opzichte van een firewallregel op databaseniveau

V. Gebruikers van één database worden volledig geïsoleerd van een andere database?
Als u Ja kiest, moet u toegang met behulp van firewallregels op databaseniveau verlenen. Hiermee voorkomt u met behulp van de firewall op serverniveau regels, die toegang via de firewall op alle databases, verminderen de diepte van de beveiliging.

V. Moeten gebruikers op het IP-adres toegang tot alle databases?
Firewallregels op server gebruiken om te beperken van het aantal keren dat die u moet de firewall-regels configureren.

V. Beschikt over de persoon die of het team configureren van de firewall-regels alleen toegang tot en met de Azure-portal, PowerShell of de REST-API?
U moet firewallregels op serverniveau. Firewallregels op databaseniveau kunnen alleen worden geconfigureerd met behulp van Transact-SQL.  

V. De persoon die of het team configureert u de firewall-regels niet toegestaan op hoog niveau machtigingen hebben op het databaseniveau van de?
Gebruik firewallregels op serverniveau. Configureren van firewallregels op databaseniveau met behulp van Transact-SQL, moet ten minste `CONTROL DATABASE` machtigingen op databaseniveau.  

V. Is de persoon die of het team configureren of controle van de firewall-regels, firewall-regels centraal te beheren voor veel (bijvoorbeeld 100) van databases?
Deze selectie is afhankelijk van uw behoeften en de omgeving. Firewallregels op server is mogelijk eenvoudiger te configureren, maar regels op databaseniveau-scripts kunt configureren. En zelfs als u firewallregels op serverniveau gebruikt, moet u mogelijk om te controleren van de database-firewallregels, om te controleren of gebruikers met `CONTROL` machtiging voor de database firewallregels op databaseniveau hebt gemaakt.

V. Kan ik een combinatie van beide firewallregels op serverniveau en databaseniveau gebruiken?
Ja. Sommige gebruikers, zoals beheerders mogelijk firewallregels op serverniveau. Andere gebruikers, zoals gebruikers van een databasetoepassing moet mogelijk firewallregels op databaseniveau.

## <a name="troubleshooting-the-database-firewall"></a>Problemen met de databasefirewall oplossen

Houd rekening met de volgende punten als toegang tot de service Microsoft Azure SQL Database niet werkt zoals verwacht:

- **Lokale firewallconfiguratie:**

  Voordat de computer toegang heeft tot Azure SQL Database, moet u mogelijk maken een firewall-uitzondering voor TCP-poort 1433 op de computer. Als u verbindingen maakt binnen de grenzen van de Azure-cloud, moet u mogelijk aanvullende poorten openen. Zie voor meer informatie de **SQL-Database: buiten vs binnen** sectie van [poorten boven 1433 voor ADO.NET 4.5 en SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md).

- **Netwerkadresomzetting (NAT):**

  Vanwege NAT kan het IP-adres gebruikt door de computer verbinding maken met Azure SQL Database mogelijk anders dan het IP-adres wordt weergegeven in uw computer IP-configuratie-instellingen. Als u het IP-adres wilt zien dat door uw computer wordt gebruikt om verbinding te maken met Azure, meldt u zich aan bij de portal en gaat u naar het tabblad **Configureren** op de server die als host fungeert voor uw database. In de sectie **Toegestane IP-adressen** wordt het **Huidige IP-adres van client** weergegeven. Klik op **Toevoegen** aan de **Toegestane IP-adressen** als u wilt toestaan dat deze computer toegang heeft tot de server.

- **Wijzigingen aan de acceptatielijst zijn nog niet doorgevoerd:**

  Mogelijk zijn er maar liefst vijf minuten vertraging voor wijzigingen in de configuratie van de Azure SQL Database-firewall pas van kracht.

- **De aanmelding is niet gemachtigd of is een onjuist wachtwoord gebruikt:**

  Als een aanmelding heeft geen machtigingen op de Azure SQL Database-server of het wachtwoord onjuist is, wordt de verbinding met de Azure SQL Database-server is geweigerd. Door een firewallinstellingen te maken, krijgen clients alleen de mogelijkheid om te proberen verbinding te maken met de server. Elke client moet alsnog de benodigde beveiligingsreferenties opgeven. Zie voor meer informatie over het voorbereiden van aanmeldingen, [beheren van Databases, aanmeldingen en gebruikers in Azure SQL Database](sql-database-manage-logins.md).

- **Dynamische IP-adres:**

  Als u een internetverbinding met dynamische IP-adressering hebt en u problemen ondervindt bij het ophalen van via de firewall, kan u een van de volgende oplossingen proberen:
  
  - Vraag uw internetprovider (ISP) naar het IP-adresbereik dat is toegewezen aan uw clientcomputers die toegang hebben tot de Azure SQL Database-server en voeg dit IP-adresbereik toe als een firewallregel.
  - Vraag in plaats daarvan statische IP-adressen voor uw clientcomputers en voeg de IP-adressen als firewallregels toe.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een snel starten voor het maken van een database en een firewallregel op serverniveau [maken van een Azure SQL database](sql-database-get-started-portal.md).
- Voor hulp bij het maken van een verbinding met een Azure SQL-database vanuit open-source toepassingen of toepassingen van derden raadpleegt u [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx) (Snelstartcodevoorbeelden voor clients met SQL Database).
- Zie voor informatie over de extra poorten die u mogelijk nodig hebt om te openen, de **SQL-Database: buiten vs binnen** sectie van [poorten boven 1433 voor ADO.NET 4.5 en SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md)
- Zie voor een overzicht van Azure SQL Database-beveiliging, [uw database beveiligen](sql-database-security-overview.md)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
