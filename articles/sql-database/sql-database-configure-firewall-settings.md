---
title: Een firewallregel op serverniveau voor SQL Database configureren | Microsoft Docs
description: Informatie over het configureren van de firewall voor IP-adressen die toegang hebben tot de Azure SQL-server.
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
ms.assetid: c3b206b5-af6e-41af-8306-db12ecfc1b5d
ms.service: sql-database
ms.custom: auth and access
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 11/28/2016
ms.author: rickbyh;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 867f06c1fae3715ab03ae4a3ff4ec381603e32f7
ms.openlocfilehash: 1479595fb7de2e0a37520c7d323624142e624af1


---
# <a name="create-and-manage-azure-sql-database-server-level-firewall-rules-using-the-azure-portal"></a>Een firewallregel op serverniveau voor Azure SQL Database maken en beheren via Azure Portal
> [!div class="op_single_selector"]
> * [Overzicht](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST-API](sql-database-configure-firewall-settings-rest.md)
> 

Met firewallregels op serverniveau kunnen beheerders een SQL Database-server openen vanaf een opgegeven IP-adres of reeks van IP-adressen. U kunt firewallregels op serverniveau ook gebruiken voor gebruikers wanneer u veel databases met dezelfde toegangsvereisten hebt en u niet elke database afzonderlijk wilt configureren. Voor een grotere veiligheid en om uw database draagbaarder te maken, adviseert Microsoft om zo veel mogelijk gebruik te maken van firewallregels op databaseniveau. Zie [Overview of SQL Database firewall rules](sql-database-firewall-configure.md) (Overzicht van de firewallregels voor SQL Database) voor een overzicht van firewallregels voor SQL Database.

> [!Note]
> Voor meer informatie over draagbare databases in de context van bedrijfscontinuïteit raadpleegt u [Authentication requirements for disaster recovery](sql-database-geo-replication-security-config.md) (Verificatievereisten voor herstel na noodgevallen).
>

[!INCLUDE [Create SQL Database firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Bestaande firewallregels op serverniveau beheren via Azure Portal
Herhaal de stappen om firewallregels op serverniveau te beheren.

* Klik op IP van client toevoegen om de huidige computer toe te voegen.
* As u extra IP-adressen wilt toevoegen, typt u de Regelnaam, het Eerste IP-adres en het Laatste IP-adres in.
* Als u een bestaande regel wilt wijzigen, klikt u op een willekeurig veld in de regel om dit aan te passen.
* Als u een bestaande regel wilt verwijderen, beweegt u de muisaanwijzer over de regel tot de X wordt weergegeven aan het eind van de regel. Klik op X om de regel te verwijderen.

Klik op **Opslaan** om de wijzigingen op te slaan.

## <a name="next-steps"></a>Volgende stappen

- Zie de [SQL Database-zelfstudie: een server, serverfirewallregel, voorbeelddatabase en databasefirewallregel maken en koppelen met SQL Server](sql-database-get-started.md) voor een zelfstudie.
- Zie [Get started with security](sql-database-get-started-security.md) (Aan de slag met beveiliging) voor een zelfstudie over beveiliging
- Voor hulp bij het maken van een verbinding met een Azure SQL-database vanuit open-source toepassingen of toepassingen van derden raadpleegt u [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx) (Snelstartcodevoorbeelden voor clients met SQL Database).
- Zie [SQL Database Authentication and Authorization: Granting Access](https://msdn.microsoft.com/library/azure/ee336235.aspx) (SQL Database-verificatie en -autorisatie: toegang verlenen) om te leren hoe u extra gebruikers maakt die met de databases verbinding kunnen maken.

## <a name="additional-resources"></a>Aanvullende bronnen
* [Securing your database](sql-database-security.md) (Uw database beveiligen)   
* [Security Center for SQL Server Database Engine and Azure SQL Database](https://msdn.microsoft.com/library/bb510589) (Security Center voor SQL Server Database Engine en Azure SQL Database)   






<!--HONumber=Dec16_HO1-->


