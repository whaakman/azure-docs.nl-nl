---
title: Algemene verbindingsfouten oplossen Azure SQL-database
description: Stappen voor het identificeren en oplossen van veelvoorkomende fouten voor Azure SQL Database.
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: ac463d1c-aec8-443d-b66e-fa5eadcccfa8
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: daleche
ms.openlocfilehash: 1d756aa023ae143608acc988ddd0ae8acee1a113
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Verbindingsproblemen met Azure SQL Database oplossen
Wanneer de verbinding met Azure SQL Database is mislukt, krijgt u [foutberichten](sql-database-develop-error-messages.md). Dit artikel is een gecentraliseerde onderwerp helpt u problemen met Azure SQL Database. Dit introduceert [algemene oorzaken](#cause) van verbindingsproblemen, raadt [een hulpprogramma voor het oplossen van problemen](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) die helpt u bij de identiteit van het probleem en bevat de stappen voor probleemoplossing voor het oplossen van [tijdelijke fouten](#troubleshoot-transient-errors) en [permanente of tijdelijke fouten](#troubleshoot-persistent-errors). 

Als u de verbindingsproblemen ondervindt, kunt u de problemen met stappen die worden beschreven in dit artikel.
[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Oorzaak
Problemen met de verbinding kunnen worden veroorzaakt door het volgende:

* Best practices en ontwerprichtlijnen toepassen tijdens het ontwerpproces voor de toepassing is mislukt.  Zie [overzicht voor ontwikkelaars van SQL Database](sql-database-develop-overview.md) aan de slag.
* Herconfiguratie van Azure SQL Database
* Firewallinstellingen
* Verbindingstime-out
* Onjuiste aanmeldingsgegevens
* Maximum aantal is bereikt op sommige Azure SQL Database-resources

Verbindingsproblemen met Azure SQL Database kunnen in het algemeen als volgt worden ingedeeld:

* [Tijdelijke fouten (tijdelijke of onregelmatige)](#troubleshoot-transient-errors)
* [Permanente of tijdelijke fouten (fouten regelmatig terugkerende)](#troubleshoot-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Probeer de probleemoplosser voor voor Azure SQL Database-verbindingsproblemen
Als u een specifieke verbindingsfout ondervindt, kunt u [dit hulpprogramma](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), die wordt waarmee u snel identiteit en het probleem kunt oplossen.

## <a name="troubleshoot-transient-errors"></a>Tijdelijke fouten oplossen

Wanneer een toepassing verbinding met een Azure SQL database maakt, wordt het volgende foutbericht weergegeven:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [!NOTE]
> Dit foutbericht wordt gewoonlijk tijdelijk (tijdelijke).
> 
> 

Deze fout treedt op wanneer de Azure-database wordt verplaatst (of opnieuw geconfigureerd) en uw toepassing verliest de verbinding met de SQL-database. SQL database-herconfiguratie gebeurtenissen optreden vanwege een geplande gebeurtenis (bijvoorbeeld een software-upgrade) of een niet-geplande gebeurtenis (bijvoorbeeld een proces crash of taakverdeling). De meeste herconfiguratie van gebeurtenissen in het algemeen tijdelijke zijn en moeten worden voltooid in minder dan 60 seconden maximaal. Deze gebeurtenissen kunnen echter van tijd tot tijd duurt langer hebt voltooid, zoals wanneer een grote transactie zorgt een herstelbewerking langlopende dat.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Stappen voor het oplossen van problemen met de tijdelijke netwerkverbinding

1. Controleer de [servicedashboard van Microsoft Azure](https://azure.microsoft.com/status) voor eventuele bekende stroomonderbrekingen zijn die is opgetreden tijdens de periode gedurende welke de fouten zijn gemeld door de toepassing.
2. Toepassingen die verbinding met een cloudservice maken, zoals Azure SQL Database mag verwachten van periodieke herconfiguratie gebeurtenissen en implementeren Pogingslogica voor het afhandelen van deze fouten in plaats van deze poorten als toepassingsfouten aan gebruikers te zien. Controleer de [tijdelijke fouten](sql-database-connectivity-issues.md) sectie en de best practices en ontwerprichtlijnen op [overzicht voor ontwikkelaars van SQL Database](sql-database-develop-overview.md) voor meer informatie en algemene probeer strategieën. Ga vervolgens naar codevoorbeelden op [Verbindingsbibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md) voor details.
3. Als een database limieten voor de nadert, kan het lijken te zijn van een tijdelijk verbindingsprobleem. Zie [het oplossen van prestatieproblemen](sql-database-troubleshoot-performance.md).
4. Als connectiviteitsproblemen gaan of een aanvraag voor de ondersteuning van Azure als de duur die door uw toepassing de fout zich voordoet groter is dan 60 seconden of als meerdere exemplaren van de fout wordt weergegeven in een bepaalde dag-bestand door het selecteren van **ophalen ondersteunen** op de [ondersteuning van Azure](https://azure.microsoft.com/support/options) site.

## <a name="troubleshoot-persistent-errors"></a>Permanente fouten oplossen
Als de toepassing niet permanent verbinding maken met Azure SQL Database, dit geeft meestal aan dat een probleem met een van de volgende:

* Firewall-configuratie. De Azure SQL database of de client de firewall blokkeert verbindingen naar Azure SQL Database.
* Netwerk herconfiguratie aan de clientzijde: bijvoorbeeld een nieuw IP-adres of een proxyserver.
* Gebruikersfout: bijvoorbeeld verbindingsparameters, zoals de naam van de server in de verbindingsreeks een typefout gemaakt.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Stappen voor het oplossen van problemen met de permanente netwerkverbinding
1. Instellen van [firewall-regels](sql-database-configure-firewall-settings.md) waarmee de client-IP-adres. Voor tijdelijke voor testdoeleinden, stelt u een firewallregel 0.0.0.0 gebruikt als het eerste IP-adresbereik en 255.255.255.255 als het laatste IP-adresbereik. Hiermee opent u de server voor alle IP-adressen. Als dit het connectiviteitsprobleem is opgelost, verwijdert u deze regel en een firewallregel voor een op de juiste wijze beperkt IP-adres of -adresbereik maken. 
2. Zorg dat de poort 1433 geopend voor uitgaande verbindingen is op alle firewalls tussen de client en het Internet. Bekijk [configureren Windows Firewall op SQL Server-toegang toestaan](https://msdn.microsoft.com/library/cc646023.aspx) en [hybride identiteit nodig poorten en protocollen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports) voor aanvullende verwijzingen zijn gerelateerd aan extra poorten die u nodig hebt om te openen voor Azure Active Directory-verificatie.
3. Controleer of de verbindingsreeks en andere instellingen. Zie de sectie verbindingsreeks in de [connectiviteit problemen onderwerp](sql-database-connectivity-issues.md#connections-to-sql-database).
4. Controleer de servicestatus van de in het dashboard. Als u denkt dat er is een regionale uitval, Zie [herstellen van een storing](sql-database-disaster-recovery.md) voor stappen voor het herstellen naar een nieuw gebied.

## <a name="next-steps"></a>Volgende stappen
* [Prestatieproblemen Azure SQL Database oplossen](sql-database-troubleshoot-performance.md)
* [Zoeken in de documentatie op Microsoft Azure](http://azure.microsoft.com/search/documentation/)
* [De meest recente updates voor de Azure SQL Database-service weergeven](http://azure.microsoft.com/updates/?service=sql-database)

## <a name="additional-resources"></a>Aanvullende bronnen
* [Overzicht van SQL Database-ontwikkeling](sql-database-develop-overview.md)
* [Algemene richtlijnen voor tijdelijke afhandeling van fouten](../best-practices-retry-general.md)
* [Verbindingsbibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md)

