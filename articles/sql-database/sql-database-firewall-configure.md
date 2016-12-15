---
title: Overzicht van de firewallregels voor SQL Database | Microsoft Docs
description: Informatie over het configureren van een SQL Database-firewall met firewallregels op server- en databaseniveau om toegang te beheren.
keywords: databasefirewall
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: ac57f84c-35c3-4975-9903-241c8059011e
ms.service: sql-database
ms.custom: auth and access
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/23/2016
ms.author: rickbyh;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 09c2332589b1170b411c6f45f4109fb8048887e2
ms.openlocfilehash: b5417e02f2c4e50c89afcfa007ccdd208775f374


---
# <a name="overview-of-azure-sql-database-firewall-rules"></a>Overzicht van de firewallregels voor Azure SQL Database 
> [!div class="op_single_selector"]
> * [Overzicht](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST-API](sql-database-configure-firewall-settings-rest.md)
> 
> 

Microsoft Azure SQL Database levert een relationele-databaseservice voor Azure en andere op internet gebaseerde toepassingen. Om uw gegevens te beschermen, verhinderen firewalls alle toegang tot de databaseserver totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.

U configureert de firewall door firewallregels te maken die bereiken opgeven van acceptabele IP-adressen. U kunt de firewallregels maken op server- en databaseniveau.

* **Firewallregels op serverniveau:** deze regels zorgen ervoor dat clients toegang krijgen tot de hele Azure SQL-server. Dat wil zeggen, tot alle databases binnen dezelfde logische server. Deze regels worden opgeslagen in de **hoofd**database. Firewallregels op serverniveau kunnen worden geconfigureerd met behulp van de portal of met behulp van Transact-SQL-instructies.
* **Firewallregels op databaseniveau:** deze regels zorgen ervoor dat clients toegang krijgen tot individuele databases in uw Azure SQL Database-server. U kunt deze regels voor elke database maken en ze worden in de desbetreffende databases opgeslagen. (U kunt maken firewallregels op databaseniveau maken voor de **hoofd**database.) Deze regels kunnen nuttig zijn om de toegang tot bepaalde (beveiligde) databases binnen dezelfde logische server te beperken. Firewallregels op databaseniveau kunnen alleen worden geconfigureerd met behulp van Transact-SQL-instructies.

**Aanbeveling:** voor een grotere veiligheid en om uw database draagbaarder te maken, adviseert Microsoft om zo veel mogelijk gebruik te maken van firewallregels op databaseniveau. Gebruik firewallregels op serverniveau voor beheerders en wanneer u veel databases met dezelfde toegangsvereisten hebt en u niet elke database afzonderlijk wilt configureren.

> [!Note]
> Voor meer informatie over draagbare databases in de context van bedrijfscontinuïteit raadpleegt u [Authentication requirements for disaster recovery](sql-database-geo-replication-security-config.md) (Verificatievereisten voor herstel na noodgevallen).
>

## <a name="firewall-overview"></a>Firewalloverzicht
In eerste instantie wordt alle Transact-SQL-toegang tot uw Azure SQL-server door de firewall geblokkeerd. Als u de Azure SQL-server wilt gaan gebruiken, gaat u naar Azure Portal en geeft u een of meer firewallregels op serverniveau op die toegang tot uw Azure SQL-server inschakelen. Gebruik de firewallregels om op te geven welke IP-adresbereiken van internet zijn toegestaan en of Azure-toepassingen kunnen proberen verbinding te maken met uw Azure SQL-server.

Als u selectief toegang wilt verlenen tot slechts een van de databases op uw Azure SQL-server, moet u voor de vereiste database een regel op databaseniveau maken. Geef voor de firewallregel van de database een IP-adresbereik op dat buiten het IP-adresbereik ligt dat is opgegeven in de firewallregel op serverniveau. Het IP-adres van de client moet binnen het bereik liggen dat is opgegeven in de regel op databaseniveau.

Verbindingspogingen van internet en Azure moeten eerst de firewall passeren voordat ze de Azure SQL-server of SQL Database kunnen bereiken, zoals in het volgende diagram wordt weergegeven:

   ![Diagram met beschrijving van firewallconfiguratie.][1]

## <a name="connecting-from-the-internet"></a>Verbinding maken vanuit internet
Wanneer een computer vanuit internet verbinding probeert te maken met de databaseserver, wordt eerst het IP-adres waar de aanvraag vandaan komt door de firewall vergeleken met de volledige set firewallregels:

* Als het IP-adres van de aanvraag binnen een van de bereiken ligt die zijn opgegeven in de firewallregels op serverniveau, wordt de verbinding aan uw Azure SQL Database-server toegestaan.
* Als het IP-adres van de aanvraag niet binnen een van de bereiken ligt die zijn opgegeven in de firewallregel op serverniveau, worden de firewallregels op databaseniveau gecontroleerd. Als het IP-adres van de aanvraag binnen een van de bereiken ligt die zijn opgegeven in de firewallregels op databaseniveau, wordt de verbinding alleen toegestaan aan de database met een corresponderende regel op databaseniveau.
* Als het IP-adres van de aanvraag niet binnen de bereiken ligt die zijn opgegeven in de firewallregels op server- of databaseniveau, kan er geen verbinding worden gemaakt.

> [!NOTE]
> Voor toegang tot Azure SQL Database vanuit de lokale computer moet u ervoor zorgen dat de firewall op uw netwerk en lokale computer uitgaande communicatie op TCP-poort 1433 toestaat.
> 

## <a name="connecting-from-azure"></a>Verbinding maken vanuit Azure
Azure-verbindingen moeten zijn ingeschakeld opdat toepassingen vanuit Azure verbinding kunnen maken met uw Azure SQL-server. Wanneer een toepassing vanuit Azure probeert verbinding te maken met uw databaseserver, verifieert de firewall of Azure-verbindingen zijn toegestaan. Een firewallinstelling waarvan het begin- en eindadres gelijk zijn aan 0.0.0.0 geeft aan dat deze verbindingen zijn toegestaan. Als de verbindingspoging niet is toegestaan, zal de aanvraag de Azure SQL Database-server niet bereiken.

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

> [!NOTE]
>  Voor meer informatie raadpleegt u de sectie **SQL Database: Outside vs inside** (SQL Database: buiten vs binnen) of [Ports beyond 1433 for ADO.NET 4.5 and SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md) (Andere poorten dan 1433 voor ADO.NET 4.5 en SQL Database)
>  

## <a name="creating-the-first-server-level-firewall-rule"></a>De eerste firewallregel op serverniveau maken
De eerste firewallinstelling op serverniveau kunt u maken met [Azure Portal](https://portal.azure.com/) of programmatisch met behulp van de REST-API of Azure PowerShell. Volgende firewallregels op serverniveau kunt u maken en beheren met deze methoden en via Transact-SQL. Voor betere prestaties worden firewallregels op serverniveau tijdelijk in de cache op databaseniveau opgeslagen. Zie [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx) als u de cache wilt vernieuwen. Voor meer informatie over firewallregels op serverniveau raadpleegt u [How to: Configure an Azure SQL server firewall using the Azure portal](sql-database-configure-firewall-settings.md) (Procedure: Een Azure SQL-serverfirewall configureren via Azure Portal).

## <a name="creating-database-level-firewall-rules"></a>Firewallregels op databaseniveau maken
Wanneer u de eerste firewall op serverniveau hebt geconfigureerd, is het raadzaam om de toegang tot bepaalde databases te beperken. Als u in de firewallregel op databaseniveau een IP-adresbereik opgeeft dat buiten het bereik ligt dat is opgegeven in de firewallregel op serverniveau, kunnen alleen clients met IP-adressen in het bereik op databaseniveau toegang krijgen tot de database. U kunt voor een database maximaal 128 firewallregels op databaseniveau opgeven. Firewallregels op databaseniveau voor hoofd- en gebruikersdatabases kunnen via Transact-SQL worden gemaakt en beheerd. Zie [sp_set_database_firewall_rule (Azure SQL Databases)](https://msdn.microsoft.com/library/dn270010.aspx) (sp_set_database_firewall_rule (Azure SQL Databases)) voor meer informatie over het configureren van firewallregels op databaseniveau.

## <a name="programmatically-managing-firewall-rules"></a>Firewallregels programmatisch beheren
U kunt firewallregels niet alleen beheren via Azure Portal maar ook programmatisch met Transact-SQL, REST-API en Azure PowerShell. De volgende tabellen beschrijven de reeks opdrachten die beschikbaar zijn voor elke methode.

### <a name="transact-sql"></a>Transact-SQL
| Catalogusweergave of opgeslagen procedure | Niveau | Beschrijving |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Server |Geeft de huidige firewallregels op serverniveau weer |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Server |Hiermee worden de firewallregels op serverniveau gemaakt of bijgewerkt |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Server |Verwijdert firewallregels op serverniveau |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Database |Geeft de huidige firewallregels op databaseniveau weer |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Database |Hiermee worden de firewallregels op databaseniveau gemaakt of bijgewerkt |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Databases |Verwijdert firewallregels op databaseniveau |

### <a name="rest-api"></a>REST API
| API | Niveau | Beschrijving |
| --- | --- | --- |
| [List Firewall Rules](https://msdn.microsoft.com/library/azure/dn505715.aspx) (Lijst met firewallregels) |Server |Geeft de huidige firewallregels op serverniveau weer |
| [Create Firewall Rule](https://msdn.microsoft.com/library/azure/dn505712.aspx) (Firewallregel maken) |Server |Hiermee worden de firewallregels op serverniveau gemaakt of bijgewerkt |
| [Set Firewall Rule](https://msdn.microsoft.com/library/azure/dn505707.aspx) (Firewallregel instellen) |Server |Werkt de eigenschappen van een bestaande firewallregel op serverniveau bij |
| [Delete Firewall Rule](https://msdn.microsoft.com/library/azure/dn505706.aspx) (Firewallregel verwijderen) |Server |Verwijdert firewallregels op serverniveau |

### <a name="azure-powershell"></a>Azure PowerShell
| Cmdlet | Niveau | Beschrijving |
| --- | --- | --- |
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx) |Server |Retourneert de huidige firewallregels op serverniveau |
| [New-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx) |Server |Maakt een nieuwe firewallregel op serverniveau |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx) |Server |Werkt de eigenschappen van een bestaande firewallregel op serverniveau bij |
| [Remove-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) |Server |Verwijdert firewallregels op serverniveau |

> [!NOTE]
> Het kan vijf minuten duren voordat wijzigingen in de firewallinstellingen van kracht zijn.
> 
> 

## <a name="troubleshooting-the-database-firewall"></a>Problemen met de databasefirewall oplossen
Houd rekening met de volgende punten als toegang tot de service Microsoft Azure SQL Database niet werkt zoals verwacht:

* **Lokale firewallconfiguratie:** voordat uw computer toegang krijgt tot Azure SQL Database moet u op uw computer misschien een firewalluitzondering maken voor TCP-poort 1433. Als u verbindingen maakt binnen de grenzen van de Azure-cloud, moet u mogelijk aanvullende poorten openen. Voor meer informatie raadpleegt u de sectie **V12 of SQL Database: Outside vs inside** (V12 SQL Database: buiten vs binnen) van [Ports beyond 1433 for ADO.NET 4.5 and SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md) (Andere poorten dan 1433 voor ADO.NET 4.5 en SQL Database V12).
* **Network Address Translation (NAT):** vanwege NAT kan het zijn dat het IP-adres waarmee de computer verbinding maakt met Azure SQL Database verschilt van het IP-adres dat wordt weergegeven in de IP-configuratie-instellingen van uw computer. Als u het IP-adres wilt zien dat door uw computer wordt gebruikt om verbinding te maken met Azure, meldt u zich aan bij de portal en gaat u naar het tabblad **Configureren** op de server die als host fungeert voor uw database. In de sectie **Toegestane IP-adressen** wordt het **Huidige IP-adres van client** weergegeven. Klik op **Toevoegen** aan de **Toegestane IP-adressen** als u wilt toestaan dat deze computer toegang heeft tot de server.
* **Wijzigingen in de acceptatielijst zijn nog niet doorgevoerd:** het kan vijf minuten duren voordat wijzigingen in de Azure SQL Database-firewallconfiguratie van kracht zijn.
* **De aanmelding is niet gemachtigd of er is een onjuist wachtwoord gebruikt:** als een aanmelding niet is gemachtigd op de Azure SQL Database-server of als het wachtwoord onjuist is, wordt de verbinding met de Azure SQL Database-server geweigerd. Door een firewallinstellingen te maken, krijgen clients alleen de mogelijkheid om te proberen verbinding te maken met de server. Elke client moet alsnog de benodigde beveiligingsreferenties opgeven. Zie Databases, aanmeldingen en gebruikers beheren in Azure SQL Database, voor meer informatie over het voorbereiden van aanmeldingen.
* **Dynamisch IP-adres:** als u een internetverbinding hebt met dynamische IP-adressering en problemen ondervindt bij het passeren van de firewall, kunt u een van de volgende oplossingen proberen:
  
  * Vraag uw internetprovider (ISP) naar het IP-adresbereik dat is toegewezen aan uw clientcomputers die toegang hebben tot de Azure SQL Database-server en voeg dit IP-adresbereik toe als een firewallregel.
  * Vraag in plaats daarvan statische IP-adressen voor uw clientcomputers en voeg de IP-adressen als firewallregels toe.

## <a name="next-steps"></a>Volgende stappen
Voor artikelen met procedures voor het maken van firewallregels op serverniveau en databaseniveau raadpleegt u:

* [Configure Azure SQL Database server-level firewall rules using the Azure Portal](sql-database-configure-firewall-settings.md) (Firewallregels voor Azure SQL Database op serverniveau configureren met Azure Portal)
* [Configure Azure SQL Database server-level and database-level firewall rules using T-SQL](sql-database-configure-firewall-settings-tsql.md) (Firewallregels voor Azure SQL Database op serverniveau en databaseniveau configureren met T-SQL)
* [Configure Azure SQL Database server-level firewall rules using PowerShell](sql-database-configure-firewall-settings-powershell.md) (Firewallregels voor Azure SQL Database op serverniveau configureren met PowerShell)
* [Configure Azure SQL Database server-level firewall rules using the REST API](sql-database-configure-firewall-settings-rest.md) (Firewallregels voor Azure SQL Database op serverniveau configureren met de REST API)

Voor een zelfstudie over het maken van een database raadpleegt u [Maak in slechts enkele minuten een SQL Database met behulp van Azure Portal](sql-database-get-started.md).
Voor hulp bij het maken van een verbinding met een Azure SQL-database vanuit open-source toepassingen of toepassingen van derden raadpleegt u [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx) (Snelstartcodevoorbeelden voor clients met SQL Database).
Zie [Manage database access and login security](https://msdn.microsoft.com/library/azure/ee336235.aspx) (Databasetoegang en aanmeldingsbeveiliging beheren) als u wilt weten hoe u naar databases navigeert.

## <a name="additional-resources"></a>Aanvullende bronnen
* [Securing your database](sql-database-security.md) (Uw database beveiligen)
* [Security Center for SQL Server Database Engine and Azure SQL Database](https://msdn.microsoft.com/library/bb510589) (Security Center voor SQL Server Database Engine en Azure SQL Database)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png



<!--HONumber=Dec16_HO1-->


