---
title: Algemene verbindingsfouten oplossen Azure SQL-database
description: Stappen voor het identificeren en oplossen van veelvoorkomende verbindingsfouten voor Azure SQL Database.
services: sql-database
author: dalechen
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: daleche
ms.openlocfilehash: 65892518cf041cbef87657b8c43cfb0b2ac333cb
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45981898"
---
# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Verbindingsproblemen met Azure SQL Database oplossen
Wanneer de verbinding met Azure SQL Database is mislukt, ontvangt u [foutberichten](sql-database-develop-error-messages.md). In dit artikel is een gecentraliseerde onderwerp waarmee u problemen met Azure SQL Database. Dit introduceert [de algemene oorzaken](#cause) van verbindingsproblemen, raadt [een hulpprogramma voor probleemoplossing](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) die identiteit het probleem, en bevat de stappen voor probleemoplossing om op te lossen [tijdelijke fouten](#troubleshoot-transient-errors) en [permanente of niet-tijdelijke fouten](#troubleshoot-persistent-errors). 

Als u de verbindingsproblemen ondervindt, kunt u de stappen voor problemen oplossen die worden beschreven in dit artikel.
[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Oorzaak
Problemen met de verbinding kunnen worden veroorzaakt door een van de volgende:

* Aanbevolen procedures en richtlijnen voor het ontwerpen van toepassing tijdens het ontwerpproces voor de toepassing is mislukt.  Zie [overzicht van de ontwikkeling van de SQL Database](sql-database-develop-overview.md) aan de slag.
* Herconfiguratie van Azure SQL-Database
* Firewallinstellingen
* Verbindingstime-out
* Onjuiste aanmeldingsgegevens
* Maximumlimiet bereikt in een Azure SQL Database-resources

Verbindingsproblemen met Azure SQL Database kunnen over het algemeen als volgt worden ingedeeld:

* [Tijdelijke fouten (tijdelijke of onregelmatige)](#troubleshoot-transient-errors)
* [Permanente of niet-tijdelijke fouten (fouten regelmatig terugkerende)](#troubleshoot-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Probeer de probleemoplosser voor problemen met de netwerkverbinding van de Azure SQL Database
Als u een specifieke verbinding-fout optreedt, probeert u [dit hulpprogramma](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), die wordt kunt u snel identiteit en het probleem kunt oplossen.

## <a name="troubleshoot-transient-errors"></a>Tijdelijke fouten oplossen

Wanneer een toepassing verbinding met een Azure SQL-database maakt, ontvangt u de volgende strekking weergegeven:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [!NOTE]
> Dit foutbericht wordt gewoonlijk tijdelijk (tijdelijke).
> 
> 

Deze fout treedt op wanneer de Azure-database wordt verplaatst (of opnieuw geconfigureerd) en uw toepassing verliest de verbinding met de SQL-database. SQL database-herconfiguratie gebeurtenissen optreden vanwege een geplande gebeurtenis is (bijvoorbeeld een software-upgrade) of een niet-geplande gebeurtenis (bijvoorbeeld een proces loopt vast of load balancing). De meeste herconfiguratie van gebeurtenissen zijn algemeen analyserapporten en maximaal in minder dan 60 seconden moeten worden uitgevoerd. Echter kunnen deze gebeurtenissen van tijd tot tijd langer duren om te voltooien, zoals wanneer een grote transactie zorgt ervoor het herstel van een langlopende dat.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Stappen voor het oplossen van problemen met de tijdelijke netwerkverbinding

1. Controleer de [servicedashboard van Microsoft Azure](https://azure.microsoft.com/status) bekende storingen die zijn opgetreden tijdens de periode gedurende welke de fouten zijn gemeld door de toepassing.
2. Toepassingen die verbinding met een cloudservice maken zoals Azure SQL Database moet periodieke herconfiguratie gebeurtenissen verwachten en implementeren van logica voor het afhandelen van deze fouten in plaats van deze als toepassingsfouten aan gebruikers zichtbaar te maken voor opnieuw proberen. Controleer de [tijdelijke fouten](sql-database-connectivity-issues.md) sectie en de aanbevolen procedures en richtlijnen voor het ontwerpen op [overzicht van de ontwikkeling van de SQL Database](sql-database-develop-overview.md) voor meer informatie en algemene strategieën voor opnieuw proberen. Ga vervolgens naar codevoorbeelden [Verbindingsbibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md) voor meer informatie.
3. Als een database de resourcelimieten nadert, kan het lijken te zijn van een tijdelijk verbindingsprobleem. Zie [resourcelimieten](sql-database-resource-limits.md).
4. Als u problemen met de netwerkverbinding gaan, of als de duur die uw toepassing de fout optreedt groter is dan 60 seconden, of als er meerdere exemplaren van de fout in een bepaalde dag een ondersteuningsaanvraag indienen voor Azure-bestand door te selecteren **ondersteuning krijgen voor**op de [ondersteuning voor Azure](https://azure.microsoft.com/support/options) site.

## <a name="troubleshoot-persistent-errors"></a>Permanente fouten oplossen
Als de toepassing niet permanent verbinding maken met Azure SQL Database, duidt dit meestal op een probleem met een van de volgende:

* Firewall-configuratie. De Azure SQL database of client-side-firewall wordt geblokkeerd door verbindingen met Azure SQL Database.
* Herconfiguratie van netwerken op de client: bijvoorbeeld een nieuw IP-adres of een proxyserver.
* Gebruikersfout: bijvoorbeeld een typefout gemaakt verbindingsparameters, zoals de naam van de server in de verbindingsreeks.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Stappen voor het oplossen van problemen met de permanente netwerkverbinding
1. Instellen van [firewall-regels](sql-database-configure-firewall-settings.md) waarmee de client-IP-adres. Voor tijdelijke voor testdoeleinden, stelt u een firewallregel 0.0.0.0 gebruikt als het eerste IP-adresbereik en 255.255.255.255 als het laatste IP-adresbereik. Hiermee opent u de server aan alle IP-adressen. Als dit het probleem met de netwerkverbinding is opgelost, verwijdert u deze regel en een firewallregel voor een op de juiste wijze beperkt IP-adres of adresbereik maken. 
2. Zorg ervoor dat poort 1433 geopend voor uitgaande verbindingen is op alle firewalls tussen de client en het Internet. Beoordeling [configureren van de Windows Firewall op SQL Server-toegang toestaan](https://msdn.microsoft.com/library/cc646023.aspx) en [hybride identiteit vereiste poorten en protocollen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports) voor aanvullende verwijzingen met betrekking tot extra poorten die u nodig hebt om te openen voor Azure Active Directory-verificatie.
3. Controleer of de verbindingsreeks en andere instellingen. Zie de sectie Connection String in het [connectiviteit problemen onderwerp](sql-database-connectivity-issues.md#connections-to-sql-database).
4. Controleer de servicestatus in het dashboard. Als u denkt er is een regionale onderbreking dat, Zie [herstellen na een storing](sql-database-disaster-recovery.md) voor stappen om te herstellen naar een nieuwe regio.

## <a name="next-steps"></a>Volgende stappen
* [Zoeken in de documentatie op Microsoft Azure](http://azure.microsoft.com/search/documentation/)
* [De meest recente updates voor de service Azure SQL Database bekijken](http://azure.microsoft.com/updates/?service=sql-database)

## <a name="additional-resources"></a>Aanvullende resources
* [Overzicht van de ontwikkeling van de SQL Database](sql-database-develop-overview.md)
* [Algemene richtlijnen voor tijdelijke afhandeling van fouten](../best-practices-retry-general.md)
* [Verbindingsbibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md)

