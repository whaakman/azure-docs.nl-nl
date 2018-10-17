---
title: Snel starten met Azure SQL Data Warehouse Fivetran | Microsoft Docs
description: Snel aan de slag met Fivetran en Azure SQL Data Warehouse.
services: sql-data-warehouse
author: hirokib
manager: jrj
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 10/12/2018
ms.author: elbutter
ms.reviewer: craigg
ms.openlocfilehash: 8e738becfe356908af5baffc0ebf225916b2616e
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49355440"
---
# <a name="get-started-quickly-with-fivetran-and-sql-data-warehouse"></a>Snel aan de slag met Fivetran en SQL Data Warehouse

In deze snelstartgids wordt ervan uitgegaan dat u al een bestaand exemplaar van SQL Data Warehouse.

## <a name="setup-connection"></a>Verbinding instellen

1. Zoek de volledig gekwalificeerde servernaam en databasenaam voor de verbinding met Azure SQL Data Warehouse.

   [Hoe kunt u zoeken naar servernaam en databasenaam in de portal?](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-connect-overview)

2. In de wizard setup bepalen als u wilt verbinding maken met uw database rechtstreeks of via een SSH-tunnel.

   Als u rechtstreeks verbinding maken met uw database besluit, moet u een firewallregel om toegang te verlenen. Dit is de eenvoudigste en veiligste methode.

   Als u verbinden via een SSH-tunnel wilt, verbinding Fivetran met een afzonderlijke server in uw netwerk waarmee u een SSH-tunnel met uw database. Deze methode is nodig als uw database zich in een niet-toegankelijke subnet in een virtueel netwerk.

3. Voeg toe '52.0.2.4' IP-adres in de firewall van uw server om binnenkomende verbindingen naar uw Azure SQL Data Warehouse van Fivetran.

   [Het toevoegen van server-level firewall?](https://docs.microsoft.com/azure/sql-data-warehouse/create-data-warehouse-portal#create-a-server-level-firewall-rule)

## <a name="setup-user-credentials"></a>De referenties van gebruiker instellen

Verbinding maken met uw Azure SQL Data Warehouse met behulp van SQL Server Management Studio of hulpprogramma naar keuze als gebruiker met beheerdersrechten en voer de volgende SQL-opdrachten voor het maken van een gebruiker voor Fivetran:

In de database master: ` CREATE LOGIN fivetran WITH PASSWORD = '<password>'; `

In SQL Data Warehouse-database:

```
CREATE USER fivetran_user_without_login without login;
CREATE USER fivetran FOR LOGIN fivetran;
GRANT IMPERSONATE on USER::fivetran_user_without_login to fivetran;
```

Als gebruiker fivetran is gemaakt, verlenen deze de volgende machtigingen voor uw datawarehouse:

```
GRANT CONTROL to fivetran;
```

Een resourceklasse geschikt toevoegen aan de gebruiker, afhankelijk van de geheugenvereiste voor het maken van de columnstore-index. Integraties, zoals Marketo en Salesforce moeten bijvoorbeeld hogere resourceklasse vanwege het grote aantal kolommen / grotere hoeveelheid gegevens, waarvoor meer geheugen voor het maken van columnstore-indexen.

Gebruik van statische resourceklassen wordt aanbevolen. U kunt beginnen met resourceklasse `staticrc20`, die kan worden toegewezen 200 MB voor de gebruiker, ongeacht het prestatieniveau dat u gebruikt. Als columnstore indexeren is mislukt met de huidige resourceklasse, hebben we de resourceklasse verhogen.

```
EXEC sp_addrolemember '<resource_class_name>', 'fivetran';
```

Bekijk voor meer informatie de documenten voor [limieten voor geheugen en gelijktijdigheid](https://docs.microsoft.com/azure/sql-data-warehouse/memory-and-concurrency-limits#data-warehouse-limits) en [resourceklassen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-memory-optimizations-for-columnstore-compression#ways-to-allocate-more-memory)

Bevoegdheid is vereist voor het maken van de referenties van de database binnen het bereik dat wordt gebruikt tijdens het laden van bestanden van Blob-opslag met behulp van PolyBase.

Voer de referenties voor toegang tot Azure SQL Data Warehouse

1. Host (de naam van uw server)
2. Poort
3. Database
4. Gebruiker (gebruikersnaam moet `fivetran@<server_name>` waar `<server_name>` maakt deel uit van de uri voor de azure-host: `<server_name>.database.windows.net`)
5. Wachtwoord