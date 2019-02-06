---
title: Azure SQL-Database en Data Warehouse-IP-firewallregels | Microsoft Docs
description: Leer hoe u een SQL-database of SQL Data Warehouse-firewall configureren met het IP-firewallregels op serverniveau beheren van toegang en een enkele of gegroepeerde-database configureren met de IP-firewallregels op databaseniveau.
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
ms.date: 02/04/2019
ms.openlocfilehash: e4b217ada2aae159680b113b6ddcb41c9d121f24
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55753058"
---
# <a name="azure-sql-database-and-sql-data-warehouse-ip-firewall-rules"></a>Azure SQL-Database en SQL Data Warehouse IP-firewall-regels

Microsoft Azure [SQL-Database](sql-database-technical-overview.md) en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) een relationele databaseservice voor Azure en andere toepassingen op basis van het Internet. Om uw gegevens te beschermen, verhinderen firewalls alle toegang tot de databaseserver totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.

> [!NOTE]
> In dit artikel is van toepassing op Azure SQL-server en op zowel SQL Database en SQL Data Warehouse-databases die zijn gemaakt op de Azure SQL-server. Voor het gemak wordt de term 'SQL Database' gebruikt wanneer er wordt verwezen naar zowel SQL Database als SQL Data Warehouse.
> [!IMPORTANT]
> In dit artikel biedt *niet* zijn van toepassing op **Azure SQL Database Managed Instance**. Raadpleeg het volgende artikel over [verbinding te maken met een beheerd exemplaar](sql-database-managed-instance-connect-app.md) voor meer informatie over welke netwerkconfiguratie is vereist.

## <a name="virtual-network-rules-as-alternatives-to-ip-rules"></a>Regels voor virtueel netwerk als alternatief voor IP-regels

Naast de IP-regels, beheert de firewall ook *virtuele netwerkregels*. Virtual network-regels zijn gebaseerd op Virtual Network-service-eindpunten. Regels voor virtueel netwerk is mogelijk beter aan IP-regels in sommige gevallen. Zie voor meer informatie, [Virtual Network-service-eindpunten en regels voor Azure SQL Database](sql-database-vnet-service-endpoint-rule-overview.md).

## <a name="overview"></a>Overzicht

In eerste instantie wordt wordt alle toegang tot uw Azure SQL-server geblokkeerd door de SQL Database-firewall. Voor toegang tot een database-server, moet u een of meer op serverniveau IP-firewall-regels waarmee toegang tot uw Azure SQL-server opgeven. Gebruik de IP-firewallregels om op te geven welke IP-adresbereiken van Internet zijn toegestaan en of Azure-toepassingen kunnen proberen verbinding maken met uw Azure SQL-server.

Als u selectief toegang wilt verlenen tot slechts een van de databases op uw Azure SQL-server, moet u voor de vereiste database een regel op databaseniveau maken. Geef een IP-adresbereik voor de database IP-firewallregel die buiten het bereik van IP-adres opgegeven in de IP-firewallregel op serverniveau en zorg ervoor dat het IP-adres van de client binnen het bereik dat is opgegeven in de regel op databaseniveau valt.

> [!IMPORTANT]
> SQL Data Warehouse wordt alleen ondersteuning biedt voor IP-firewallregels op serverniveau en biedt geen ondersteuning voor de IP-firewallregels op databaseniveau.

Verbindingspogingen van internet en Azure moeten eerst de firewall passeren voordat ze de Azure SQL-server of SQL Database kunnen bereiken, zoals in het volgende diagram wordt weergegeven:

   ![Diagram met beschrijving van firewallconfiguratie.][1]

- **Firewallregels op serverniveau-IP:**

  Deze regels zorgen ervoor dat clients toegang krijgen tot de hele Azure SQL-server, dat wil zeggen, alle databases binnen dezelfde SQL-Database-server. Deze regels worden opgeslagen in de **hoofd**database. IP-firewallregels op serverniveau kunnen worden geconfigureerd met behulp van de portal of met behulp van Transact-SQL-instructies. Voor het maken van IP-firewallregels op serverniveau met de Azure portal of PowerShell, moet u eigenaar van het abonnement of een bijdrager aan het abonnement. Voor het maken van een niveau van de server IP-firewall-regel met behulp van Transact-SQL, moet u verbinding met de SQL-Database-exemplaar als de principal-aanmelding op serverniveau of met de Azure Active Directory-beheerder (wat betekent dat een IP-firewallregel op serverniveau moet eerst worden gemaakt door een gebruiker met machtigingen op entiteitsniveau Azure).

- **Firewallregels op databaseniveau-IP:**

  Deze regels zorgen ervoor dat clients toegang krijgen tot bepaalde (beveiligde) databases binnen dezelfde SQL-Database-server. U kunt deze regels voor elke database maken (met inbegrip van de **master** database) en ze worden opgeslagen in de afzonderlijke databases. Het IP-firewallregels op databaseniveau voor hoofd- en gebruikersdatabases kunnen alleen worden gemaakt en beheerd met behulp van Transact-SQL-instructies en alleen nadat u de eerste firewall op serverniveau hebt geconfigureerd. Als u een IP-adresbereik in de regel op databaseniveau IP-firewall die zich buiten het bereik dat is opgegeven in de IP-firewallregel op serverniveau opgeeft, kunnen alleen clients met IP-adressen in het bereik op databaseniveau toegang krijgen tot de database. U kunt een maximum van 128 op databaseniveau IP-firewallregels voor een database hebben. Zie het voorbeeld verderop in dit artikel en Zie voor meer informatie over het configureren van IP-firewallregels op databaseniveau [sp_set_database_firewall_rule (Azure SQL Database)](https://msdn.microsoft.com/library/dn270010.aspx).

### <a name="recommendation"></a>Aanbeveling

Microsoft raadt u aan met behulp van IP-firewallregels op databaseniveau indien mogelijk om beveiliging te verbeteren en uw database draagbaarder te maken. Gebruik de IP-firewallregels op serverniveau voor beheerders en wanneer u veel databases met dezelfde toegangsvereisten hebt en u niet wilt dat elke database afzonderlijk configureren.

> [!IMPORTANT]
> Windows Azure SQL Database ondersteunt maximaal 128 IP-firewallregels.
> [!NOTE]
> Voor meer informatie over draagbare databases in de context van bedrijfscontinuïteit raadpleegt u [Authentication requirements for disaster recovery](sql-database-geo-replication-security-config.md) (Verificatievereisten voor herstel na noodgevallen).

### <a name="connecting-from-the-internet"></a>Verbinding maken vanuit internet

Wanneer een computer probeert verbinding maken met uw database-server vanaf het Internet, controleert de firewall eerst het oorspronkelijke IP-adres van de aanvraag op basis van de database op serverniveau IP-firewall-regels, voor de database die de verbinding moet worden gemaakt:

- Als het IP-adres van de aanvraag binnen een van de opgegeven in de firewallregels op databaseniveau IP-bereiken, wordt de verbinding toegestaan met de SQL-Database die de regel bevat.
- Als het IP-adres van de aanvraag niet binnen een van de opgegeven in de firewallregel op databaseniveau IP-bereiken, worden de IP-firewallregels op serverniveau gecontroleerd. Als het IP-adres van de aanvraag binnen een van de bereiken die is opgegeven in de IP-firewallregels op serverniveau, wordt de verbinding toegestaan. IP-firewallregels op serverniveau zijn van toepassing op alle SQL-databases op de Azure SQL-server.  
- Als het IP-adres van de aanvraag niet binnen de bereiken die zijn opgegeven in een van de database-niveau of firewallregels op serverniveau, IP-: de verbindingsaanvraag is mislukt.

> [!NOTE]
> Voor toegang tot Azure SQL Database vanuit de lokale computer moet u ervoor zorgen dat de firewall op uw netwerk en lokale computer uitgaande communicatie op TCP-poort 1433 toestaat.

### <a name="connecting-from-azure"></a>Verbinding maken vanuit Azure

Azure-verbindingen moeten zijn ingeschakeld opdat toepassingen vanuit Azure verbinding kunnen maken met uw Azure SQL-server. Wanneer een toepassing vanuit Azure probeert verbinding te maken met uw databaseserver, verifieert de firewall of Azure-verbindingen zijn toegestaan. Een firewallinstelling waarvan het begin- en eindadres gelijk zijn aan 0.0.0.0 geeft aan dat Azure-verbindingen zijn toegestaan. Als de verbindingspoging niet is toegestaan, zal de aanvraag de Azure SQL Database-server niet bereiken.

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.

## <a name="creating-and-managing-ip-firewall-rules"></a>Het maken en beheren van IP-firewallregels

De eerste firewallinstelling op serverniveau kan worden gemaakt met de [Azure-portal](https://portal.azure.com/) of programmatisch met [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql), [Azure CLI](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create), of de [ REST-API](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate). Volgende niveau van de server IP-firewall-regels kunnen worden gemaakt en beheerd met behulp van deze methoden en via Transact-SQL.

> [!IMPORTANT]
> IP-firewallregels op databaseniveau kunnen alleen worden gemaakt en beheerd met behulp van Transact-SQL.

Voor betere prestaties, de IP-firewallregels op serverniveau tijdelijk worden opgeslagen op het databaseniveau van de. Zie [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx) als u de cache wilt vernieuwen.

> [!TIP]
> U kunt [SQL Database Auditing](sql-database-auditing.md) om te controleren van wijzigingen van de firewall op serverniveau en databaseniveau.

## <a name="manage-ip-firewall-rules-using-the-azure-portal"></a>IP-firewallregels met behulp van de Azure-portal beheren

Om in te stellen een IP-firewallregel op serverniveau in Azure portal, kunt u ofwel gaat u naar de pagina overzicht voor uw Azure SQL database of de overzichtspagina voor uw SQL Database-server.

> [!TIP]
> Zie voor een zelfstudie [maken van een database met behulp van de Azure-portal](sql-database-single-database-get-started.md).

### <a name="from-database-overview-page"></a>Overzichtspagina voor database

1. Klik op de overzichtspagina voor de database te een IP-firewallregel op serverniveau **serverfirewall instellen** op de werkbalk zoals weergegeven in de volgende afbeelding: De pagina **Firewallinstellingen** voor de SQL Database-server wordt geopend.

      ![Server IP-firewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png)

2. Klik op **IP van client toevoegen** op de werkbalk om het toevoegen van het IP-adres van de computer u momenteel gebruikt en klik vervolgens op **opslaan**. Een IP-firewallregel op serverniveau is voor uw huidige IP-adres gemaakt.

      ![IP-firewallregel op serverniveau instellen](./media/sql-database-get-started-portal/server-firewall-rule-set.png)

### <a name="from-server-overview-page"></a>Overzichtspagina voor server

De overzichtspagina voor uw server wordt geopend, met de volledig gekwalificeerde servernaam (zoals **mynewserver20170403.database.windows.net**) en opties voor verdere configuratie.

1. Als een regel op serverniveau overzichtspagina voor de server, klikt u op **Firewall** in het linkermenu onder instellingen:

2. Klik op **IP van client toevoegen** op de werkbalk om het toevoegen van het IP-adres van de computer u momenteel gebruikt en klik vervolgens op **opslaan**. Een IP-firewallregel op serverniveau is voor uw huidige IP-adres gemaakt.

## <a name="manage-ip-firewall-rules-using-transact-sql"></a>Beheren van IP-firewallregels met behulp van Transact-SQL

| Catalogusweergave of opgeslagen procedure | Niveau | Description |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Server |Geeft de huidige niveau van de server IP-firewall-regels |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Server |Hiermee maken of bijwerken van de IP-firewallregels op serverniveau |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Server |Hiermee verwijdert u de IP-firewallregels op serverniveau |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Database |Geeft de huidige op databaseniveau IP-firewall-regels |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Database |Hiermee maken of bijwerken van de IP-firewallregels op databaseniveau |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Databases |Hiermee verwijdert u op databaseniveau IP-firewall-regels |

De volgende voorbeelden bekijken van de bestaande regels, een bereik van IP-adressen op de Contoso-server inschakelen en Hiermee verwijdert u een IP-firewallregel:

```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```

Voeg vervolgens een IP-firewallregel op serverniveau.

```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

Opgeslagen procedure verwijderen van een IP-firewallregel op serverniveau, voert u de sp_delete_firewall_rule uit. Het volgende voorbeeld wordt de regel contosofirewallrule verwijderd:

```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```

## <a name="manage-server-level-ip-firewall-rules-using-azure-powershell"></a>IP-firewallregels op serverniveau met behulp van Azure PowerShell beheren

| Cmdlet | Niveau | Description |
| --- | --- | --- |
| [Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule) |Server |Retourneert de huidige niveau van de server IP-firewall-regels |
| [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) |Server |Hiermee maakt u een nieuwe op serverniveau IP-firewallregel |
| [Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule) |Server |De eigenschappen van een bestaande op serverniveau IP-firewallregel |
| [Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule) |Server |Hiermee verwijdert u de IP-firewallregels op serverniveau |

Het volgende voorbeeld wordt een niveau van de server IP-firewall-regel met behulp van PowerShell:

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
```

> [!TIP]
> Zie voor PowerShell-voorbeelden in de context van een snel starten, [DB maken - PowerShell](sql-database-powershell-samples.md) en [een individuele database maken en een SQL-Database op serverniveau IP-firewallregel configureren met behulp van PowerShell](scripts/sql-database-create-and-configure-database-powershell.md)

## <a name="manage-server-level-ip-firewall-rules-using-azure-cli"></a>IP-firewallregels op serverniveau met behulp van Azure CLI beheren

| Cmdlet | Niveau | Description |
| --- | --- | --- |
|[AZ sql server firewall-regel maken](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create)|Server|Hiermee maakt u een server IP-firewallregel|
|[AZ sql server firewall-rule list](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-list)|Server|Geeft een lijst van de IP-firewall-regels op een server|
|[AZ sql server firewall-rule show](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-show)|Server|Geeft de details van een IP-firewallregel|
|[AZ sql server firewall-rule update](/cli/azure/sql/server/firewall-rule##az-sql-server-firewall-rule-update)|Server|Updates van een IP-firewallregel|
|[AZ sql server firewall-rule delete](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-delete)|Server|Hiermee verwijdert u een IP-firewallregel|

Het volgende voorbeeld wordt een niveau van de server IP-firewall-regel met de Azure CLI:

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
-n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Zie voor een Azure CLI-voorbeeld in de context van een snel starten, [DB maken - Azure CLI](sql-database-cli-samples.md) en [een individuele database maken en configureren van een SQL Database-IP-firewall-regel met de Azure CLI](scripts/sql-database-create-and-configure-database-cli.md)

## <a name="manage-server-level-ip-firewall-rules-using-rest-api"></a>Beheren van IP-firewallregels op serverniveau met behulp van REST-API

| API | Niveau | Description |
| --- | --- | --- |
| [List Firewall Rules](https://docs.microsoft.com/rest/api/sql/firewallrules/listbyserver) (Lijst met firewallregels) |Server |Geeft de huidige niveau van de server IP-firewall-regels |
| [Firewallregels maken of bijwerken](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate) |Server |Hiermee maken of bijwerken van de IP-firewallregels op serverniveau |
| [Delete Firewall Rule](https://docs.microsoft.com/rest/api/sql/firewallrules/delete) (Firewallregel verwijderen) |Server |Hiermee verwijdert u de IP-firewallregels op serverniveau |
| [Firewallregels ophalen](https://docs.microsoft.com/rest/api/sql/firewallrules/get) | Server | IP-firewallregels op serverniveau opgehaald |

## <a name="server-level-ip-firewall-rule-versus-a-database-level-ip-firewall-rule"></a>Het IP-firewallregel op serverniveau ten opzichte van een IP-firewallregel op databaseniveau

V. Gebruikers van één database worden volledig geïsoleerd van een andere database?
Als u Ja kiest, moet u toegang met behulp van IP-firewallregels op databaseniveau verlenen. Hiermee voorkomt u met behulp van op serverniveau IP-firewallregels, die via de firewall op alle databases, verminderen de diepte van uw beveiliging toestaan.

V. Moeten gebruikers op het IP-adres toegang tot alle databases?
IP-firewallregels op serverniveau gebruiken om te beperken van het aantal keren die moet u IP-firewallregels configureren.

V. Beschikt over de persoon die of het team configureren van de IP-firewallregels alleen toegang tot en met de Azure-portal, PowerShell of de REST-API?
U moet de IP-firewallregels op serverniveau. IP-firewallregels op databaseniveau kunnen alleen worden geconfigureerd met behulp van Transact-SQL.  

V. De persoon die of het team configureert u de IP-firewall-regels niet toegestaan op hoog niveau machtigingen hebben op het databaseniveau van de?
Gebruik de IP-firewallregels op serverniveau. Configureren van IP-firewallregels op databaseniveau met behulp van Transact-SQL, moet ten minste `CONTROL DATABASE` machtigingen op databaseniveau.  

V. Is de persoon die of het team configureren of controle van de IP-firewallregels, Centraal beheer van IP-firewallregels voor veel (bijvoorbeeld 100) van databases?
Deze selectie is afhankelijk van uw behoeften en de omgeving. IP-firewallregels op serverniveau is mogelijk eenvoudiger te configureren, maar regels op databaseniveau-scripts kunt configureren. En zelfs als u de IP-firewallregels op serverniveau gebruikt, moet u mogelijk om te controleren van de database op serverniveau IP-firewall-regels, om te controleren of gebruikers met `CONTROL` machtiging voor de database IP-firewallregels op databaseniveau hebt gemaakt.

V. Kan ik een combinatie van IP-firewallregels op serverniveau en databaseniveau zowel gebruiken?
Ja. Sommige gebruikers, zoals beheerders mogelijk IP-firewallregels op serverniveau. Andere gebruikers, zoals gebruikers van een databasetoepassing mogelijk nodig hebt op databaseniveau IP-firewallregels.

## <a name="troubleshooting-the-database-firewall"></a>Problemen met de databasefirewall oplossen

Houd rekening met de volgende punten als toegang tot de service Microsoft Azure SQL Database niet werkt zoals verwacht:

- **Lokale firewallconfiguratie:**

  Voordat de computer toegang heeft tot Azure SQL Database, moet u mogelijk maken een firewall-uitzondering voor TCP-poort 1433 op de computer. Als u verbindingen maakt binnen de grenzen van de Azure-cloud, moet u mogelijk aanvullende poorten openen. Zie voor meer informatie de **SQL-Database: Buiten vs binnen** sectie van [poorten boven 1433 voor ADO.NET 4.5 en SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md).

- **Netwerkadresomzetting (NAT):**

  Vanwege NAT kan het IP-adres gebruikt door de computer verbinding maken met Azure SQL Database mogelijk anders dan het IP-adres wordt weergegeven in uw computer IP-configuratie-instellingen. Als u het IP-adres wilt zien dat door uw computer wordt gebruikt om verbinding te maken met Azure, meldt u zich aan bij de portal en gaat u naar het tabblad **Configureren** op de server die als host fungeert voor uw database. In de sectie **Toegestane IP-adressen** wordt het **Huidige IP-adres van client** weergegeven. Klik op **Toevoegen** aan de **Toegestane IP-adressen** als u wilt toestaan dat deze computer toegang heeft tot de server.

- **Wijzigingen aan de acceptatielijst zijn nog niet doorgevoerd:**

  Mogelijk zijn er maar liefst vijf minuten vertraging voor wijzigingen in de configuratie van de Azure SQL Database-firewall pas van kracht.

- **De aanmelding is niet gemachtigd of is een onjuist wachtwoord gebruikt:**

  Als een aanmelding heeft geen machtigingen op de Azure SQL Database-server of het wachtwoord onjuist is, wordt de verbinding met de Azure SQL Database-server is geweigerd. Door een firewallinstellingen te maken, krijgen clients alleen de mogelijkheid om te proberen verbinding te maken met de server. Elke client moet alsnog de benodigde beveiligingsreferenties opgeven. Zie voor meer informatie over het voorbereiden van aanmeldingen, [beheren van Databases, aanmeldingen en gebruikers in Azure SQL Database](sql-database-manage-logins.md).

- **Dynamische IP-adres:**

  Als u een internetverbinding met dynamische IP-adressering hebt en u problemen ondervindt bij het ophalen van via de firewall, kan u een van de volgende oplossingen proberen:
  
  - Vraag uw Internetproviderverbindingen (ISP) voor het IP-adresbereik is toegewezen aan uw clientcomputers die toegang hebben tot de Azure SQL Database-server en vervolgens als een IP-firewallregel toevoegen het IP-adresbereik.
  - Ophalen van statische IP-adressen in plaats daarvan voor uw clientcomputers, en voeg de IP-adressen als IP-firewallregels.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een snel starten voor het maken van een IP-firewallregel op serverniveau [maken van een Azure SQL database](sql-database-single-database-get-started.md).
- Voor hulp bij het maken van een verbinding met een Azure SQL-database vanuit open-source toepassingen of toepassingen van derden raadpleegt u [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx) (Snelstartcodevoorbeelden voor clients met SQL Database).
- Zie voor informatie over de extra poorten die u mogelijk nodig hebt om te openen, de **SQL-Database: Buiten vs binnen** sectie van [poorten boven 1433 voor ADO.NET 4.5 en SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md)
- Zie voor een overzicht van Azure SQL Database-beveiliging, [uw database beveiligen](sql-database-security-overview.md)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
