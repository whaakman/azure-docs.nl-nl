---
title: Beveiliging van de Azure SQL Database voor herstel na noodgevallen configureren | Microsoft Docs
description: Meer informatie over de beveiligingsoverwegingen voor het configureren en beheren van de beveiliging na het terugzetten van een database of een failover naar een secundaire server.
services: sql-database
documentationcenter: na
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: c7c898c9-69d4-4e16-8b7e-720bbb3353dd
ms.service: sql-database
ms.custom: business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 10/13/2016
ms.author: sashan
ms.openlocfilehash: 5bdcdba4956a12b54559b8accf822a4f41656045
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a>Configureren en beheren van Azure SQL Database-beveiliging voor geo-herstel of failover 

Dit onderwerp beschrijft de verificatievereisten voor het configureren en beheren van [actieve geo-replicatie](sql-database-geo-replication-overview.md) en de stappen die nodig zijn voor het instellen van de gebruikerstoegang tot de secundaire database. Ook wordt beschreven hoe u nadat u hebt gebruikt voor toegang tot de herstelde database [geo-restore](sql-database-recovery-using-backups.md#geo-restore). Zie voor meer informatie over opties voor Systeemherstel [Business Continuity Overview](sql-database-business-continuity.md).

> [!NOTE]
> [Actieve geo-replicatie](sql-database-geo-replication-overview.md) is nu beschikbaar voor alle databases in alle Servicelagen.
>  

## <a name="disaster-recovery-with-contained-users"></a>Herstel na noodgevallen met ingesloten gebruikers
In tegenstelling tot traditionele gebruikers dat moeten worden toegewezen aan aanmeldingen in de database master, een contained-gebruiker volledig beheerd door de database zelf. Dit heeft twee voordelen. De gebruikers verbinding maken met de nieuwe primaire database kunnen blijven in de noodherstelscenario of de database hersteld met behulp van geo-herstel zonder extra configuratie, omdat de database de gebruikers beheert. Er zijn ook potentiële schaalbaarheid en prestatievoordelen van deze configuratie vanuit het oogpunt van de aanmelding van. Zie [Ingesloten databasegebruikers: een draagbare database maken](https://msdn.microsoft.com/library/ff929188.aspx) voor meer informatie. 

De belangrijkste afweging is dat het proces van het herstel na noodgevallen op grote schaal beheren hoe moeilijker. Onderhoud van de referenties met een ingesloten gebruikers in meerdere databases mogelijk wanneer er meerdere databases die gebruikmaken van dezelfde aanmeldingsnaam, de voordelen van het ingesloten gebruikers uitsluiten. Bijvoorbeeld, het wachtwoordbeleid rotatie vereist wijzigingen consistent worden aangebracht in meerdere databases in plaats van het wijzigen van het wachtwoord voor de aanmelding eenmaal in de database master. Om deze reden als er meerdere databases die gebruikmaken van dezelfde gebruikersnaam en wachtwoord, wordt met behulp van de ingesloten gebruikers niet aanbevolen. 

## <a name="how-to-configure-logins-and-users"></a>Het configureren van aanmeldingen en gebruikers
Als u aanmeldingen en gebruikers (in plaats van ingesloten gebruikers), moet u extra stappen om ervoor te zorgen dat de dezelfde aanmeldingen bestaan in de database master uitvoeren. De volgende secties worden de stappen die betrokken zijn en aanvullende overwegingen.

### <a name="set-up-user-access-to-a-secondary-or-recovered-database"></a>Gebruikerstoegang tot een secundaire of door de herstelde database instellen
In de volgorde voor de secundaire database om te worden gebruikt als een secundaire database alleen-lezen en om te controleren of de juiste toegang heeft tot de nieuwe primaire database of de database herstellen met geo-herstel de hoofddatabase van de doelserver moet beschikken over de juiste beveiligingsconfiguratie voordat het herstel.

De specifieke machtigingen voor elke stap worden verderop in dit onderwerp beschreven.

Voorbereiden van de gebruikerstoegang tot een secundaire geo-replicatie moet worden uitgevoerd als onderdeel geo-replicatie configureren. Voorbereiden van de gebruikerstoegang tot de databases geo hersteld moet worden uitgevoerd op elk gewenst moment wanneer de oorspronkelijke server online is (bijvoorbeeld als onderdeel van de details voor Dr).

> [!NOTE]
> Als u een failover of geo-herstel naar een server waarop geen correct geconfigureerde aanmeldingen, toegang tot deze beperkt tot de server admin-account zijn.
> 
> 

Instellen van aanmeldingen op de doelserver omvat drie stappen die hieronder worden beschreven:

#### <a name="1-determine-logins-with-access-to-the-primary-database"></a>1. Aanmeldingen met toegang tot de primaire database bepalen:
De eerste stap van het proces is om te bepalen welke aanmeldingen op de doelserver moeten worden gedupliceerd. Dit wordt bereikt met een SELECT-instructies, één in de logische hoofddatabase op de bronserver en één in de primaire database zelf.

Alleen de serverbeheerder of een lid van de **LoginManager** serverfunctie kunt bepalen de aanmeldingen op de bronserver met de volgende SELECT-instructie. 

    SELECT [name], [sid] 
    FROM [sys].[sql_logins] 
    WHERE [type_desc] = 'SQL_Login'

Alleen een lid van de databaserol db_owner, de gebruiker dbo of serverbeheerder kan bepalen dat alle van de database gebruiker principals in de primaire database.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-the-sid-for-the-logins-identified-in-step-1"></a>2. De beveiligings-id niet vinden voor de aanmeldingen geïdentificeerd in stap 1:
Door het vergelijken van de uitvoer van de query's uit de vorige sectie en die overeenkomt met de SID's, kunt u de server-aanmelding toewijzen aan databasegebruiker. Aanmeldingen die een databasegebruiker met een overeenkomende SID hebben gebruikerstoegang hebben tot die database als die databasegebruiker principal. 

De volgende query kan worden gebruikt om alle van de gebruiker principals en hun SID's in een database te zien. Alleen een lid van de functie of servergroep-beheerder voor de database voor db_owner deze query uitvoeren kan.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> De **INFORMATION_SCHEMA** en **sys** gebruikers hebben *NULL* SID's, en de **Gast** SID is **0x00**. De **dbo** SID mogen beginnen met *0x01060000000001648000000000048454*als maker van de database is de serverbeheerder in plaats van een lid van **DbManager**.
> 
> 

#### <a name="3-create-the-logins-on-the-target-server"></a>3. Maak de aanmeldingen op de doelserver:
Er is de laatste stap gaat u naar de doelserver of servers en de aanmeldingen met de juiste beveiligings-id's genereren. De basis-syntaxis is als volgt.

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> Als u wilt dat gebruikerstoegang verlenen tot de secundaire, maar niet naar de primaire, u dat doen kunt door het wijzigen van de gebruikersaanmelding op de primaire server met behulp van de volgende syntaxis.
> 
> ALTER LOGIN <login name> UITSCHAKELEN
> 
> UITSCHAKELEN wordt niet het wachtwoord wijzigen zodat u deze altijd inschakelen kunt indien nodig.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het beheren van toegang tot de database en aanmeldingen [SQL Database-beveiliging: beveiliging van toegang en meld u aan de database beheren](sql-database-manage-logins.md).
* Zie voor meer informatie over ingesloten databasegebruikers [opgenomen databasegebruikers: maken van uw Database draagbare](https://msdn.microsoft.com/library/ff929188.aspx).
* Zie voor meer informatie over het gebruik en actieve geo-replicatie configureren [actieve geo-replicatie](sql-database-geo-replication-overview.md)
* Zie voor meer informatie over het gebruik van geo-restore [geo-herstel](sql-database-recovery-using-backups.md#geo-restore)

