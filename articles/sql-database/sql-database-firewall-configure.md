---
title: Firewallregels voor Azure SQL Database | Microsoft Docs
description: Informatie over het configureren van een SQL Database-firewall met firewallregels op server- en databaseniveau om toegang te beheren.
keywords: databasefirewall
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: ac57f84c-35c3-4975-9903-241c8059011e
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 10/11/2017
ms.author: carlrab
ms.openlocfilehash: 1988bc7ab5b498db32d7bb40623f1194d7290b94
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="azure-sql-database-server-level-and-database-level-firewall-rules"></a>Azure SQL Database-server- en databaseniveau firewall-regels 

Microsoft Azure SQL Database levert een relationele-databaseservice voor Azure en andere op internet gebaseerde toepassingen. Om uw gegevens te beschermen, verhinderen firewalls alle toegang tot de databaseserver totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.

#### <a name="virtual-network-rules-as-alternatives-to-ip-rules"></a>Virtueel netwerk regels als alternatief voor IP-regels

Naast het IP-regels, beheert de firewall ook *virtueel netwerk regels*. Virtueel netwerk regels zijn gebaseerd op Virtual Network service-eindpunten. Regels voor virtueel netwerk is mogelijk beter IP-regels in sommige gevallen. Zie voor meer informatie, [Virtual Network service-eindpunten en regels voor Azure SQL Database](sql-database-vnet-service-endpoint-rule-overview.md).

## <a name="overview"></a>Overzicht

In eerste instantie wordt alle Transact-SQL-toegang tot uw Azure SQL-server door de firewall geblokkeerd. Om uw Azure SQL-server te gebruiken, moet u een of meer serverniveau firewallregels waarmee toegang tot uw Azure SQL-server opgeven. Gebruik de firewallregels om op te geven welke IP-adresbereiken van internet zijn toegestaan en of Azure-toepassingen kunnen proberen verbinding te maken met uw Azure SQL-server.

Als u selectief toegang wilt verlenen tot slechts een van de databases op uw Azure SQL-server, moet u voor de vereiste database een regel op databaseniveau maken. Geef voor de firewallregel van de database een IP-adresbereik op dat buiten het IP-adresbereik ligt dat is opgegeven in de firewallregel op serverniveau. Het IP-adres van de client moet binnen het bereik liggen dat is opgegeven in de regel op databaseniveau.

Verbindingspogingen van internet en Azure moeten eerst de firewall passeren voordat ze de Azure SQL-server of SQL Database kunnen bereiken, zoals in het volgende diagram wordt weergegeven:

   ![Diagram met beschrijving van firewallconfiguratie.][1]

* **Firewallregels op serverniveau:** deze regels zorgen ervoor dat clients toegang krijgen tot de hele Azure SQL-server. Dat wil zeggen, tot alle databases binnen dezelfde logische server. Deze regels worden opgeslagen in de **hoofd**database. Firewallregels op serverniveau kunnen worden geconfigureerd met behulp van de portal of met behulp van Transact-SQL-instructies. Als u firewallregels op serverniveau wilt maken met Azure Portal of PowerShell, moet u de eigenaar van het abonnement of een bijdrager aan het abonnement zijn. Als u een firewallregel op serverniveau wilt maken met Transact-SQL, moet u verbinding maken met het SQL Database-exemplaar als de hoofdaanmelding op serviceniveau of als de beheerder van Azure Active Directory (wat betekent dat er eerst een firewallregel op serverniveau moet worden gemaakt door een gebruiker met machtigingen op Azure-niveau).
* **Databaseniveau firewallregels:** deze regels kunnen clients toegang tot bepaalde (beveiligde) databases binnen dezelfde logische server. U kunt deze regels voor elke database maken (inclusief de **master** database) en ze worden opgeslagen in de afzonderlijke databases. Databaseniveau firewallregels voor hoofd- en -databases kunnen alleen worden gemaakt en beheerd met behulp van Transact-SQL-instructies en pas nadat u de firewall van het eerste niveau van de server hebt geconfigureerd. Als u in de firewallregel op databaseniveau een IP-adresbereik opgeeft dat buiten het bereik ligt dat is opgegeven in de firewallregel op serverniveau, kunnen alleen clients met IP-adressen in het bereik op databaseniveau toegang krijgen tot de database. U kunt voor een database maximaal 128 firewallregels op databaseniveau opgeven. Zie voor meer informatie over het configureren van de firewallregel op databaseniveau regels in het voorbeeld verderop in dit artikel en Zie [sp_set_database_firewall_rule (Azure SQL-Databases)](https://msdn.microsoft.com/library/dn270010.aspx).

**Aanbeveling:** voor een grotere veiligheid en om uw database draagbaarder te maken, adviseert Microsoft om zo veel mogelijk gebruik te maken van firewallregels op databaseniveau. Gebruik firewallregels op serverniveau voor beheerders en wanneer u veel databases met dezelfde toegangsvereisten hebt en u niet elke database afzonderlijk wilt configureren.

> [!Important]
> Windows Azure SQL Database ondersteunt maximaal 128 firewallregels.
>

> [!Note]
> Voor meer informatie over draagbare databases in de context van bedrijfscontinuïteit raadpleegt u [Authentication requirements for disaster recovery](sql-database-geo-replication-security-config.md) (Verificatievereisten voor herstel na noodgevallen).
>

### <a name="connecting-from-the-internet"></a>Verbinding maken vanuit internet

Wanneer een computer via internet verbinding probeert te maken met de databaseserver, controleert de firewall eerst het oorspronkelijke IP-adres van de aanvraag aan de hand van de firewallregels voor de database waarmee verbinding moet worden gemaakt:

* Als het IP-adres van de aanvraag binnen een van de bereiken ligt die zijn opgegeven in de firewallregel op databaseniveau, wordt de verbinding met de SQL-database die de regel bevat, toegestaan.
* Als het IP-adres van de aanvraag niet binnen een van de bereiken ligt die zijn opgegeven in de firewallregel op databaseniveau, worden de firewallregels op serverniveau gecontroleerd. Als het IP-adres van de aanvraag binnen een van de bereiken ligt die zijn opgegeven in de firewallregels op serverniveau, wordt de verbinding toegestaan. Firewallregels op serverniveau zijn van toepassing op alle SQL-databases op de Azure SQL-server.  
* Als het IP-adres van de aanvraag niet binnen de bereiken die in de database- of server niveau firewallregels zijn opgegeven is, wordt de verbindingsaanvraag mislukt.

> [!NOTE]
> Voor toegang tot Azure SQL Database vanuit de lokale computer moet u ervoor zorgen dat de firewall op uw netwerk en lokale computer uitgaande communicatie op TCP-poort 1433 toestaat.
> 

### <a name="connecting-from-azure"></a>Verbinding maken vanuit Azure
Azure-verbindingen moeten zijn ingeschakeld opdat toepassingen vanuit Azure verbinding kunnen maken met uw Azure SQL-server. Wanneer een toepassing vanuit Azure probeert verbinding te maken met uw databaseserver, verifieert de firewall of Azure-verbindingen zijn toegestaan. Een firewallinstelling waarvan het begin- en eindadres gelijk zijn aan 0.0.0.0 geeft aan dat deze verbindingen zijn toegestaan. Als de verbindingspoging niet is toegestaan, zal de aanvraag de Azure SQL Database-server niet bereiken.

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

## <a name="creating-and-managing-firewall-rules"></a>Maken en beheren van de firewall-regels
Het eerste niveau van de server firewall-instellingen kan worden gemaakt met de [Azure-portal](https://portal.azure.com/) of programmatisch met [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql), [Azure CLI](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create), of de [REST-API](https://docs.microsoft.com/rest/api/sql/firewallrules). Volgende firewallregels op serverniveau kunt u maken en beheren met deze methoden en via Transact-SQL. 

> [!IMPORTANT]
> Firewallregel op databaseniveau regels kunnen alleen worden gemaakt en beheerd met behulp van Transact-SQL. 
>

Voor betere prestaties worden firewallregels op serverniveau tijdelijk in de cache op databaseniveau opgeslagen. Zie [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx) als u de cache wilt vernieuwen. 

> [!TIP]
> U kunt [SQL Database Auditing](sql-database-auditing.md) wijzigingen van de server- en databaseniveau firewall controleren.
>

## <a name="manage-firewall-rules-using-the-azure-portal"></a>Firewallregels met de Azure portal beheren

Als een firewallregel op serverniveau in de Azure-portal, kunt u ofwel gaan naar de pagina overzicht voor uw Azure SQL database of de pagina overzicht voor de logische server van uw Azure-Database.

> [!TIP]
> Zie voor een zelfstudie [maken van een database met de Azure portal](sql-database-get-started-portal.md).
>

**Van de overzichtspagina van database**

1. Als een firewallregel op serverniveau van de overzichtspagina van database, klikt u op **ingesteld serverfirewall** op de werkbalk zoals weergegeven in de volgende afbeelding: de **Firewall-instellingen** pagina voor de Database van SQL server wordt geopend.

      ![serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png) 

2. Klik op **client-IP toevoegen** op de werkbalk om het toevoegen van het IP-adres van de computer u momenteel gebruikt en klik vervolgens op **opslaan**. Er wordt een firewallregel op serverniveau gemaakt voor uw huidige IP-adres.

      ![serverfirewallregel instellen](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

**Van de overzichtspagina van server**

De overzichtspagina voor uw server wordt geopend, waarin u de volledig gekwalificeerde servernaam (zoals **mynewserver20170403.database.windows.net**) en biedt opties voor verdere configuratie.

1. Als een regel van het niveau van de server van de overzichtspagina van server, klikt u op **Firewall** links in het menu onder instellingen: 

2. Klik op **client-IP toevoegen** op de werkbalk om het toevoegen van het IP-adres van de computer u momenteel gebruikt en klik vervolgens op **opslaan**. Er wordt een firewallregel op serverniveau gemaakt voor uw huidige IP-adres.

## <a name="manage-firewall-rules-using-transact-sql"></a>Firewallregels met Transact-SQL beheren
| Catalogusweergave of opgeslagen procedure | Niveau | Beschrijving |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Server |Geeft de huidige firewallregels op serverniveau weer |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Server |Hiermee worden de firewallregels op serverniveau gemaakt of bijgewerkt |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Server |Verwijdert firewallregels op serverniveau |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Database |Geeft de huidige firewallregels op databaseniveau weer |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Database |Hiermee worden de firewallregels op databaseniveau gemaakt of bijgewerkt |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Databases |Verwijdert firewallregels op databaseniveau |


Controleer de bestaande regels in de volgende voorbeelden, zorgen dat een bereik met IP-adressen op de server van Contoso en Hiermee verwijdert u een firewallregel:
   
```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```
  
Voeg daarna een firewallregel toe.
   
```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

Als u een firewallregel op serverniveau wilt verwijderen, voert u de opgeslagen procedure sp_delete_firewall_rule uit. Het volgende voorbeeld wordt de regel voor licentiecontrole ContosoFirewallRule verwijderd:
   
```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```   

## <a name="manage-firewall-rules-using-azure-powershell"></a>Firewallregels met Azure PowerShell beheren
| Cmdlet | Niveau | Beschrijving |
| --- | --- | --- |
| [Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule) |Server |Retourneert de huidige firewallregels op serverniveau |
| [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) |Server |Maakt een nieuwe firewallregel op serverniveau |
| [Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule) |Server |Werkt de eigenschappen van een bestaande firewallregel op serverniveau bij |
| [Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule) |Server |Verwijdert firewallregels op serverniveau |


Het volgende voorbeeld wordt een firewallregel op serverniveau met behulp van PowerShell:

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
```

> [!TIP]
> Zie voor PowerShell-voorbeelden in de context van een snel starten, [maken DB - PowerShell](sql-database-get-started-powershell.md) en [een individuele database maken en configureren van een firewallregel met behulp van PowerShell](scripts/sql-database-create-and-configure-database-powershell.md)
>

## <a name="manage-firewall-rules-using-azure-cli"></a>Firewallregels met Azure CLI beheren
| Cmdlet | Niveau | Beschrijving |
| --- | --- | --- |
|[AZ sql server-firewallregel maken](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Server|Hiermee maakt u een firewallregel op server|
|[lijst van AZ sql server-firewallregel](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Server|Geeft een lijst van de firewall-regels op een server|
|[AZ sql server-firewallregel weergeven](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Server|Geeft de details van een firewallregel|
|[update van AZ sql server-firewallregel](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Server|Een firewallregel bijgewerkt|
|[AZ sql server-firewallregel verwijderen](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Server|Hiermee verwijdert u een firewallregel|

Het volgende voorbeeld wordt een firewallregel op serverniveau met de Azure CLI: 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Zie voor een voorbeeld Azure CLI in de context van een snel starten [maken DDB - Azure CLI](sql-database-get-started-cli.md) en [een individuele database maken en configureren van een firewallregel op met de Azure CLI](scripts/sql-database-create-and-configure-database-cli.md)
>

## <a name="manage-firewall-rules-using-rest-api"></a>Firewallregels met REST API beheren
| API | Niveau | Beschrijving |
| --- | --- | --- |
| [List Firewall Rules](https://docs.microsoft.com/rest/api/sql/FirewallRules/ListByServer) (Lijst met firewallregels) |Server |Geeft de huidige firewallregels op serverniveau weer |
| [Firewallregels maken of bijwerken](https://docs.microsoft.com/rest/api/sql/FirewallRules/CreateOrUpdate) |Server |Hiermee worden de firewallregels op serverniveau gemaakt of bijgewerkt |
| [Delete Firewall Rule](https://docs.microsoft.com/rest/api/sql/FirewallRules/Delete) (Firewallregel verwijderen) |Server |Verwijdert firewallregels op serverniveau |

## <a name="server-level-firewall-rule-versus-a-database-level-firewall-rule"></a>Firewallregel op serverniveau ten opzichte van een firewallregel op databaseniveau
Q. Gebruikers van één database worden volledig geïsoleerd van een andere database?   
  Zo ja, verlenen toegang met behulp van de firewallregel op databaseniveau regels. Zo voorkomt u met behulp van serverniveau firewallregels, die toegang via de firewall op alle databases, waardoor de diepte van de beveiliging.   
 
Q. Hebben gebruikers op het IP-adres toegang nodig tot alle databases?   
  Firewallregels op serverniveau gebruiken om te beperken van het aantal keren die moet u firewallregels configureren.   

Q. Toegang via de Azure-portal, PowerShell of de REST-API hebben de persoon of het team dat alleen de firewallregels configureren?   
  Hebt u firewallregels op serverniveau. Firewallregels databaseniveau kunnen alleen worden geconfigureerd met behulp van Transact-SQL.  

Q. De firewallregels verboden opheft op hoog niveau machtiging op databaseniveau configureert de persoon of het team?   
  Gebruik de firewallregels op serverniveau. Configureren van de firewallregel op databaseniveau regels met Transact-SQL vereist ten minste `CONTROL DATABASE` machtiging op databaseniveau.  

Q. Is de persoon of het team configureren of de firewallregels controle centraal beheren van de firewallregels voor veel (bijvoorbeeld 100) van databases?   
  Deze selectie zijn afhankelijk van uw behoeften en omgeving. Firewallregels op serverniveau mogelijk eenvoudiger te configureren, maar scripting kunt regels configureren op het database-niveau. En zelfs als u firewallregels op serverniveau gebruikt, moet u mogelijk om te controleren van de database-firewall-regels, om te controleren of gebruikers met `CONTROL` toegang tot de database firewallregel op databaseniveau regels hebt gemaakt.   

Q. Kan ik een combinatie van beide firewallregels voor server- en databaseniveau gebruiken?   
  Ja. Sommige gebruikers, zoals beheerders mogelijk firewallregels op serverniveau. Andere gebruikers, zoals gebruikers van een databasetoepassing mogelijk databaseniveau firewallregels.   

## <a name="troubleshooting-the-database-firewall"></a>Problemen met de databasefirewall oplossen
Houd rekening met de volgende punten als toegang tot de service Microsoft Azure SQL Database niet werkt zoals verwacht:

* **Lokale firewallconfiguratie:** voordat uw computer toegang krijgt tot Azure SQL Database moet u op uw computer misschien een firewalluitzondering maken voor TCP-poort 1433. Als u verbindingen maakt binnen de grenzen van de Azure-cloud, moet u mogelijk aanvullende poorten openen. Zie voor meer informatie de **SQL-Database: buiten tegenover binnen** sectie van [poorten buiten 1433 voor ADO.NET 4.5 en SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).
* **Network Address Translation (NAT):** vanwege NAT kan het zijn dat het IP-adres waarmee de computer verbinding maakt met Azure SQL Database verschilt van het IP-adres dat wordt weergegeven in de IP-configuratie-instellingen van uw computer. Als u het IP-adres wilt zien dat door uw computer wordt gebruikt om verbinding te maken met Azure, meldt u zich aan bij de portal en gaat u naar het tabblad **Configureren** op de server die als host fungeert voor uw database. In de sectie **Toegestane IP-adressen** wordt het **Huidige IP-adres van client** weergegeven. Klik op **Toevoegen** aan de **Toegestane IP-adressen** als u wilt toestaan dat deze computer toegang heeft tot de server.
* **Wijzigingen in de acceptatielijst zijn nog niet doorgevoerd:** het kan vijf minuten duren voordat wijzigingen in de Azure SQL Database-firewallconfiguratie van kracht zijn.
* **De aanmelding is niet gemachtigd of er is een onjuist wachtwoord gebruikt:** als een aanmelding niet is gemachtigd op de Azure SQL Database-server of als het wachtwoord onjuist is, wordt de verbinding met de Azure SQL Database-server geweigerd. Door een firewallinstellingen te maken, krijgen clients alleen de mogelijkheid om te proberen verbinding te maken met de server. Elke client moet alsnog de benodigde beveiligingsreferenties opgeven. Zie Databases, aanmeldingen en gebruikers beheren in Azure SQL Database, voor meer informatie over het voorbereiden van aanmeldingen.
* **Dynamisch IP-adres:** als u een internetverbinding hebt met dynamische IP-adressering en problemen ondervindt bij het passeren van de firewall, kunt u een van de volgende oplossingen proberen:
  
  * Vraag uw internetprovider (ISP) naar het IP-adresbereik dat is toegewezen aan uw clientcomputers die toegang hebben tot de Azure SQL Database-server en voeg dit IP-adresbereik toe als een firewallregel.
  * Vraag in plaats daarvan statische IP-adressen voor uw clientcomputers en voeg de IP-adressen als firewallregels toe.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een snel starten over het maken van een database en een firewallregel op serverniveau [maken van een Azure SQL database](sql-database-get-started-portal.md).
- Voor hulp bij het maken van een verbinding met een Azure SQL-database vanuit open-source toepassingen of toepassingen van derden raadpleegt u [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx) (Snelstartcodevoorbeelden voor clients met SQL Database).
- Zie voor informatie over extra poorten die u wilt openen, de **SQL-Database: buiten tegenover binnen** sectie van [poorten buiten 1433 voor ADO.NET 4.5 en SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md)
- Zie voor een overzicht van Azure SQL Database-beveiliging [beveiliging van uw database](sql-database-security-overview.md)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
