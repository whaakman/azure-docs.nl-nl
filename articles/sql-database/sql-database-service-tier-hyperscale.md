---
title: Overzicht van grootschalige voor Azure SQL Database | Microsoft Docs
description: In dit artikel beschrijft de grootschalige service tier in het op vCore gebaseerde aankoopmodel in Azure SQL Database en wordt uitgelegd wat is het verschil met de Servicelagen voor algemeen gebruik en bedrijfskritiek.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 05/06/2019
ms.openlocfilehash: a953af3d9cd5a6748b79465a59b4a4284e58714c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67070130"
---
# <a name="hyperscale-service-tier-for-up-to-100-tb"></a>Zeer grootschalige servicelaag voor maximaal 100 TB

Azure SQL Database is gebaseerd op SQL Server Database Engine-architectuur die wordt aangepast aan de cloudomgeving om ervoor te zorgen, zelfs in het geval van infrastructuuruitval voor 99,99% beschikbaarheid. Er zijn drie architectuur modellen die worden gebruikt in Azure SQL Database:
- Algemeen doel/Standard 
-  Hyperscale
-  Business Critical/Premium

De servicelaag grootschalige in Azure SQL Database is de nieuwste service tier in het op vCore gebaseerde aankoopmodel. Deze servicelaag is een zeer schaalbare opslag en compute-prestatielaag die gebruikmaakt van de Azure-architectuur om te schalen om de opslag en rekenresources voor een Azure SQL-Database aanzienlijk buiten de grenzen beschikbaar voor de algemeen gebruik en het bedrijf Kritieke Servicelagen.

> 
> [!NOTE]
> Zie voor meer informatie over de Servicelagen voor algemeen gebruik en bedrijfskritiek in het op vCore gebaseerde aankoopmodel [algemeen](sql-database-service-tier-general-purpose.md) en [bedrijfskritiek](sql-database-service-tier-business-critical.md) Servicelagen. Zie voor een vergelijking van de vCore gebaseerde aankoopmodel met het op DTU gebaseerde aankoopmodel [Azure SQL Database-modellen en -bronnen aanschaffen](sql-database-service-tiers.md).


## <a name="what-are-the-hyperscale-capabilities"></a>Wat zijn de mogelijkheden voor grootschalige

De servicelaag grootschalige in Azure SQL Database biedt de volgende aanvullende mogelijkheden:

- Ondersteuning voor maximaal 100 TB grootte van de database
- Nagenoeg onmiddellijke back-ups (op basis van bestand momentopnamen die zijn opgeslagen in Azure Blob-opslag), ongeacht de grootte geen i/o-invloed hebben op Compute-database   
- Database terugzetten (op basis van momentopnamen van het bestand) in minuten in plaats van uren of dagen snel (niet een grootte van gegevens)
- Betere algehele prestaties vanwege de hogere doorvoer van het logboek en snellere transactie doorvoeren tijden, ongeacht de gegevensvolumes
- Snelle scale-out - kunt u een of meer alleen-lezen knooppunten voor het offloaden van uw workload voor lezen en voor gebruik als hot-stand-bys inrichten
- Snelle schaal omhoog - u kunt, constante tijdstip opschalen uw rekenresources voor zware werklasten, wanneer nodig, en de compute-resources worden vervolgens weer omlaag schalen wanneer u niet nodig hebt.

De servicelaag grootschalige Hiermee verwijdert u veel van de praktische grenzen traditioneel gezien in clouddatabases. Waar de meeste andere databases zijn beperkt tot door de beschikbare resources in een enkel knooppunt hebben databases in de zeer grootschalige service tier geen dergelijke beperkingen. Met de flexibele opslagarchitectuur groeit opslag naar behoefte. In feite niet worden zeer grootschalige databases gemaakt met een gedefinieerde maximale grootte. De database van een grootschalige neemt toe naarmate er nodig - en worden in rekening gebracht voor de capaciteit die u gebruikt. Voor lees-intensieve werkbelastingen biedt de servicelaag grootschalige snelle scale-out in te richten extra lezen-replica's zo nodig voor offloading Lees-workloads.

Bovendien is de tijd nodig is om te maken van databaseback-ups of om omhoog te schalen of omlaag is niet langer gekoppeld aan het volume van gegevens in de database. Zeer grootschalige databases kunnen worden back-ups bijna onmiddellijk. Ook kunt u een database in de tientallen terabytes omhoog of omlaag schalen binnen enkele minuten. Deze mogelijkheid hoeft u geen zorgen maakt over worden vakken door de initiële configuratie-opties.

Zie voor meer informatie over de compute-grootten voor de servicelaag van grootschalige, [Service tier kenmerken](sql-database-service-tiers-vcore.md#service-tier-characteristics).

## <a name="who-should-consider-the-hyperscale-service-tier"></a>Wie moet rekening houden met de servicelaag grootschalige

De servicelaag is voornamelijk bedoeld voor klanten met grote databases grootschalige hetzij on-premises en wilt laten hun toepassingen moderniseren door over te stappen naar de cloud of voor klanten die zich al in de cloud en worden beperkt door de maximale databasegrootte beperkingen (1-4 TB). Het is ook bedoeld voor klanten die hoge prestaties en hoge schaalbaarheid voor opslag en compute.

De servicelaag grootschalige biedt ondersteuning voor alle SQL Server-workloads, maar is voornamelijk geoptimaliseerd voor OLTP. De servicelaag grootschalige biedt ook ondersteuning voor hybride en analytische workloads (datamart).

> [!IMPORTANT]
> Elastische pools bieden geen ondersteuning voor de grootschalige service tier.

## <a name="hyperscale-pricing-model"></a>Zeer grootschalige prijsmodel

Zeer grootschalige servicelaag is alleen beschikbaar in [vCore-model](sql-database-service-tiers-vcore.md). Zodat deze overeenkomt met de nieuwe architectuur, is het prijsmodel enigszins afwijken van de service-lagen voor algemeen gebruik en bedrijfskritiek:

- **COMPUTE**:

  De prijs van grootschalige compute-eenheid is per replica. De [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) prijs als u wilt lezen schaal replica's automatisch wordt toegepast. We maken een primaire replica en een alleen-lezen replica per database zeer grootschalige standaard.  Gebruikers kunnen het totale aantal replica's, waaronder de primaire van 1-5 aanpassen.

- **Opslag**:

  U hoeft niet te geven van de maximale gegevensgrootte bij het configureren van een grootschalige-database. In de laag Hyperscale wordt opslag voor uw database in rekening gebracht op basis van daadwerkelijk gebruik. Opslag wordt automatisch toegewezen tussen 10 GB en 100 TB in stappen die dynamisch tussen 10GB en 40GB aangepast worden.  

Zie voor meer informatie over de prijzen voor grootschalige [prijzen van Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/single/)

## <a name="distributed-functions-architecture"></a>Functies voor gedistribueerde architectuur

In tegenstelling tot traditionele database-engines die beschikken over alle van de functies voor het beheer van gegevens in één locatie/proces gecentraliseerde (zelfs zodat aangeroepen gedistribueerde databases in de productieomgeving vandaag meerdere exemplaren van een monolithisch gegevensengine hebben), een database zeer grootschalige worden gescheiden de verwerking van query's-engine, waar de semantiek van verschillende gegevensengines afwijkt van de onderdelen die langdurige opslag en duurzaamheid voor de gegevens bieden. Op deze manier kunnen de opslagcapaciteit kan worden soepel uitgebreid zo ver nodig (eerste doel is 100 TB). Alleen-lezen replica's delen dezelfde opslagonderdelen, zodat geen kopiëren van gegevens is vereist voor het instellen van een nieuwe leesbare replica. 

Het volgende diagram illustreert de verschillende soorten knooppunten in een grote database:

![architectuur](./media/sql-database-hyperscale/hyperscale-architecture.png)

Een grote database bevat de volgende soorten knooppunten:

### <a name="compute-node"></a>Rekenknooppunt

De compute-knooppunt is waar de relationele engine zich bevinden, zodat alle de elementen van de taal, verwerking van query's, enzovoort, optreden. Alle interactie van gebruikers met een grote database gebeurt via deze compute-knooppunten. COMPUTE-knooppunten op basis van SSD-caches (met de naam RBPEX - Tolerante Buffergroepuitbreiding in het voorgaande diagram) hebben om te beperken het nummer van het netwerk retouren zijn vereist voor het ophalen van een pagina met gegevens. Er is een primaire rekenknooppunt waar de werkbelastingen voor lezen / schrijven en transacties worden verwerkt. Er zijn een of meer secundaire compute-knooppunten die fungeren als hot stand-by-knooppunten voor failover-doeleinden, evenals fungeren als alleen-lezen rekenknooppunten voor het offloading-workloads (als deze functionaliteit is vereist) te lezen.

### <a name="page-server-node"></a>Pagina server-knooppunt

Pagina-servers zijn systemen voor een scale-out opslag-engine.  Elke pagina-server is verantwoordelijk voor een subset van de pagina's in de database.  Nominaal, elke pagina serverbesturingselementen tussen 128 GB en 1 TB aan gegevens. Er zijn geen gegevens worden gedeeld op meer dan één pagina server (buiten de replica's die worden bijgehouden voor redundantie en beschikbaarheid). De taak van een pagina-server is voor het bieden van database-pagina's uit naar de rekenknooppunten op aanvraag, en het behouden van de pagina's bijgewerkt als transacties gegevens bijwerken. Pagina-servers worden up-to-date gehouden door af te spelen records in logboek registreren vanuit de log-service. Pagina servers onderhouden ook caches op basis van SSD voor betere prestaties. Langdurige opslag van gegevenspagina's wordt opgeslagen in Azure Storage voor extra betrouwbaarheid.

### <a name="log-service-node"></a>Logboek-knooppunt

Het knooppunt van de service log records in logboek registreren van het primaire rekenknooppunt accepteert, ze zich blijft voordoen in een duurzame cache en verzendt de records in logboek registreren voor de rest van de compute-knooppunten (zodat ze hun caches bijwerken kunnen) en de relevante pagina (s), zodat de gegevens bijgewerkt t worden kunnen Hier volgen enkele. Op deze manier worden alle gegevenswijzigingen van de primaire rekenknooppunt doorgegeven via de logboekservice tot alle secundaire compute-knooppunten en pagina-servers. Ten slotte worden het log-record (s) doorgegeven aan opslag in Azure Storage, dit een opslagplaats voor onbeperkte opslag is op lange termijn. Dit mechanisme Hiermee verwijdert u de noodzaak van regelmatig moet worden afgekapt. De log-service heeft ook een lokale cache om toegang te versnellen.

### <a name="azure-storage-node"></a>Azure storage-knooppunt

Het Azure storage-knooppunt is de uiteindelijke bestemming van gegevens van pagina-servers. Deze opslag wordt gebruikt voor back-updoeleinden ook als die voor replicatie tussen Azure-regio's. Back-ups bestaan uit momentopnamen van bestanden. Herstellen bewerking zijn snel uit deze momentopnamen en gegevens kunnen worden hersteld naar een bepaald tijdstip.

## <a name="backup-and-restore"></a>Back-ups en herstellen

Back-ups base momentopname van de bestanden en ze zijn daarom nagenoeg onmiddellijke. Opslag- en rekencapaciteit gescheiden inschakelen in de back-up-/ herstelbewerking pushen naar de storage-laag te verminderen van de werkbelasting van de verwerking van het primaire knooppunt. Als gevolg hiervan is de back-up van een grote database niet van invloed op de prestaties van het primaire knooppunt. Op deze manier herstelacties moeten worden uitgevoerd door de momentopname van het bestand kopiëren en als zodanig zijn niet een grootte van gegevens. De herstelbewerking is voor herstelbewerkingen binnen hetzelfde opslagaccount, snel.

## <a name="scale-and-performance-advantages"></a>Schaal en prestaties voordelen

Met de mogelijkheid om snel automatisch instellen/uitschakelen als u meer alleen-lezen-compute-knooppunten, de architectuur kan aanzienlijke grootschalige mogelijkheden voor lezen en kan ook het primaire knooppunt voor het uitvoeren van meer schrijfaanvragen vrij. Ook kunnen de compute-knooppunten worden geschaald omhoog/omlaag snel vanwege de gedeelde opslag-architectuur van de grootschalige-architectuur.

## <a name="create-a-hyperscale-database"></a>Maak een grootschalige-database

Een grote database kan worden gemaakt met de [Azure-portal](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current), [Powershell](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabase) of [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create). Zeer grootschalige databases zijn alleen beschikbaar is met de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).

De volgende T-SQL-opdracht maakt een grootschalige-database. Moet u de editie en de service-doel in de `CREATE DATABASE` instructie. Raadpleeg de [resourcelimieten](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases#hyperscale-service-tier) voor een lijst van geldige service doelstellingen.

```sql
-- Create a HyperScale Database
CREATE DATABASE [HyperScaleDB1] (EDITION = 'HyperScale', SERVICE_OBJECTIVE = 'HS_Gen5_4');
GO
```
Hiermee maakt u een database zeer grootschalige op Gen5 hardware met 4 kernen.

## <a name="migrate-an-existing-azure-sql-database-to-the-hyperscale-service-tier"></a>Een bestaande Azure SQL-Database migreren naar de servicelaag grootschalige

U kunt uw bestaande Azure SQL-databases verplaatsen naar grootschalige met behulp van de [Azure-portal](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current), [Powershell](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase) of [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update). Op dit moment is dit een eenzijdige migratie. U kunt databases van grootschalige niet verplaatsen naar een andere servicelaag. U wordt aangeraden een kopie van uw productiedatabases en migreren naar grootschalige voor het testen van concepten (Proefverzamelingen).

De volgende T-SQL-opdracht is een database verplaatst naar de grootschalige service tier. Moet u de editie en de service-doel in de `ALTER DATABASE` instructie.

```sql
-- Alter a database to make it a HyperScale Database
ALTER DATABASE [DB2] MODIFY (EDITION = 'HyperScale', SERVICE_OBJECTIVE = 'HS_Gen5_4');
GO
```

## <a name="connect-to-a-read-scale-replica-of-a-hyperscale-database"></a>Verbinding maken met een leesschaal replica van een grootschalige-database

In grote databases, de `ApplicationIntent` argument in de verbindingsreeks die is opgegeven door de client bepaalt of de verbinding wordt doorgestuurd naar de replica schrijven of naar een secundaire replica alleen-lezen. Als de `ApplicationIntent` ingesteld op `READONLY` en de database heeft geen een secundaire replica, verbinding worden doorgestuurd naar de primaire replica en de standaard ingesteld op `ReadWrite` gedrag.

```cmd
-- Connection string with application intent
Server=tcp:<myserver>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```
## <a name="disaster-recovery-for-hyperscale-databases"></a>Herstel na noodgevallen voor grote Databases
### <a name="restoring-a-hyperscale-database-to-a-different-geography"></a>Herstellen van een grootschalige-database naar een andere Geografie bevinden
Als u een grootschalige Azure SQL Database-database herstellen naar een andere regio dan degene die dit momenteel wordt gehost wilt in, als onderdeel van een herstel na noodgevallen bewerking of inzoomen, verplaatsing of een andere reden is de primaire methode voor te doen van een geo-herstel van de database.  Dit omvat het precies dezelfde stappen als wat u kunt een andere AZURE SQL-database herstellen naar een andere regio:
1. Als u nog geen een geschikte server bevat, moet u een SQL-Database-server maken in de doelregio.  Deze server moet eigendom zijn van hetzelfde abonnement als de oorspronkelijke (bron)-server.
2. Volg de instructies in de [geo-herstel](https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups#geo-restore) onderwerp van de pagina op het herstellen van Azure SQL-Databases van automatische back-ups.

> [!NOTE]
> Omdat de bron- en zich in verschillende regio's, delen niet de database opslag voor momentopnamen met de brondatabase zoals in niet-geo-herstellen extreem snel kunnen worden uitgevoerd.  In het geval van een geo-herstel van een grote database is dit een bewerking met de grootte van gegevens, zelfs als het doel in de gekoppelde regio van de opslag met geo-replicatie.  Dit betekent dat dat doen van een geo-herstel duurt lang in verhouding met de grootte van de database die wordt hersteld.  Als het doel in de gekoppelde regio, wordt de kopie zich in een datacenter, is aanzienlijk sneller dan een interlokale kopie via het internet, maar deze nog steeds kopieert u alle bits.

## <a name=regions></a>Beschikbare regio 's

De Azure SQL Database grootschalige-laag is momenteel beschikbaar in de volgende regio's:

- Australië - oost
- Australië - zuidoost
- Brazilië - zuid
- Canada - midden
- US - centraal
- China - oost 2
- China - noord 2
- Azië - oost
- US - oost
- East Us 2
- Frankrijk - centraal
- Japan - oost
- Japan - west
- Korea - centraal
- Korea - zuid
- US - noord-centraal
- Europa - noord
- Zuid-Afrika (noord)
- US - zuid-centraal
- Azië - zuidoost
- Verenigd Koninkrijk Zuid
- Verenigd Koninkrijk West
- Europa -west
- US - west
- US - west 2

Als u maken van zeer grootschalige database in een regio die niet wordt vermeld wilt als ondersteund, kunt u een onboarding-aanvraag via Azure portal verzenden. We werken om uit te breiden de lijst met ondersteunde regio's wordt dus controleer weer voor de meest recente regiolijst met.

Om aan te vragen de mogelijkheid om te maken van zeer grootschalige databases in de regio's die niet wordt vermeld:

1. Navigeer naar [Azure Help en ondersteuning-Blade](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)

2. Klik op [ **nieuwe ondersteuningsaanvraag**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

    ![Azure Help en ondersteuning-Blade](media/sql-database-service-tier-hyperscale/whitelist-request-screen-1.png)

3. Voor **probleemtype**, selecteer **limieten voor Service en -abonnement (quota)**

4. Kies het abonnement dat u gebruikt voor het maken van de database (s)

5. Voor **Quotumtype**, selecteer **SQL-database**

6. Klik op **Next: Oplossingen**

1. Klik op **vindt u informatie**

    ![Details van probleem](media/sql-database-service-tier-hyperscale/whitelist-request-screen-2.png)

8. Kies **SQL-Database quotumtype**: **Andere quotumaanvraag**

9. Vul in de volgende sjabloon:

    ![Quotumgegevens](media/sql-database-service-tier-hyperscale/whitelist-request-screen-3.png)

    Geef de volgende informatie in de sjabloon.

    > Aanvraag voor het maken van Azure grootschalige SQL-Database in een nieuwe regio<br/> Regio: [Vul in de opgevraagde regio]  <br/>
    > SKU/totaal rekenkernen, inclusief leesbare replica 's <br/>
    > Aantal TB geschatte 
    >

10. Kies **Ernst C**

11. Kies de juiste contactmethode en vul gegevens in.

12. Klik op **opslaan** en **gaan**

## <a name="known-limitations"></a>Bekende beperkingen
Dit zijn de huidige beperkingen aan de servicelaag grootschalige vanaf algemene beschikbaarheid.  We werken actief als u wilt verwijderen als veel van deze beperkingen mogelijk.

| Probleem | Description |
| :---- | :--------- |
| Het deelvenster back-ups beheren voor een logische server wordt niet weergegeven voor het zeer grootschalige databases van SQL server worden gefilterd  | Zeer grootschalige heeft een afzonderlijke methode voor het beheren van back-ups en zo de lange termijn wordt bewaard en punt in tijd back-retentie-instellingen zijn niet van toepassing / ongeldig worden gemaakt. Zeer grootschalige databases verschijnen dus niet in het deelvenster back-up beheren. |
| Terugzetten naar eerder tijdstip | Zodra een database wordt gemigreerd naar de servicelaag van grootschalige, wordt herstel naar een punt-in-time vóór de migratie wordt niet ondersteund.|
| Herstellen van niet - flexibele DB Hypserscale en vice versa | U kunt een grootschalige-database niet terugzetten in een niet-flexibele-database, noch kunt u een niet-flexibele-database herstellen in een grootschalige-database.|
| Als een databasebestand tijdens de migratie vanwege een actieve werkbelasting toeneemt en de 1 TB per bestand grens snijdt, mislukt de migratie | Oplossingen: <br> -Migreren van de database indien mogelijk, als er geen update-workload uitgevoerd.<br> -Probeer opnieuw de migratie, het slaagt, zolang de grens van 1 TB niet tijdens de migratie is overschreden.|
| Beheerd exemplaar | Azure SQL Database Managed Instance is momenteel niet ondersteund met grootschalige databases. |
| Elastische pools |  Elastische Pools worden momenteel niet ondersteund met SQL Database grootschalige.|
| Migratie naar grootschalige is momenteel een enkelvoudige bewerking | Zodra een database wordt gemigreerd naar grootschalige, worden niet het rechtstreeks aan de servicelaag van een niet-flexibele gemigreerd. Op dit moment is de enige manier om een database migreren van grootschalige naar niet-flexibele te exporteren/importeren met behulp van een BACPAC-bestand.|
| Migratie van databases met objecten in het geheugen | Objecten in het geheugen moeten worden verwijderd en opnieuw als niet-In-Memory-objecten worden gemaakt voordat u een database migreren naar de grootschalige service tier.|
| Bijhouden van gegevens | Niet mogelijk het gebruik van bijhouden van gegevens met zeer grootschalige databases. |
| Geo-replicatie  | U configureren niet nog geo-replicatie voor Azure SQL Database grootschalige.  U kunt geo-herstellen (herstellen van de database in een andere Geografie bevinden, voor andere doeleinden of DR) uitvoeren |
| TDE/Azure Sleutelkluis-integratie | Transparante versleuteling van de Database met behulp van Azure Key Vault (vaak aangeduid als Bring-Your-Own-Key of byok genoemd) wordt nog niet ondersteund voor Azure SQL Database zeer grootschalige, maar TDE met de Service beheerde sleutels wordt volledig ondersteund. |
|Intelligente databasefuncties | 1. Index, Drop Index Advisor modellen zijn niet is getraind voor grote databases te maken. <br/>2. Schemaprobleem, DbParameterization - onlangs toegevoegd adviseurs worden niet ondersteund voor grootschalige Database.|



## <a name="next-steps"></a>Volgende stappen

- Zie voor veelgestelde vragen op grote schaal, [Veelgestelde vragen over zeer grootschalige](sql-database-service-tier-hyperscale-faq.md).
- Zie voor meer informatie over Servicelagen [Servicelagen](sql-database-service-tiers.md)
- Zie [overzicht van resource beperkt op een logische server](sql-database-resource-limits-logical-server.md) voor informatie over de beperkingen op het niveau van de server en -abonnement.
- Zie voor het aanschaffen van model-limieten voor één database [Azure SQL Database vCore gebaseerde model limieten voor één database aanschaffen](sql-database-vcore-resource-limits-single-databases.md).
- Voor een functies en van de vergelijkingslijst, Zie [algemene SQL-functies](sql-database-features.md).
