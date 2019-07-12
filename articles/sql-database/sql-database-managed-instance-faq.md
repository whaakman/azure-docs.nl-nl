---
title: SQL Database managed instance Veelgestelde vragen over | Microsoft Docs
description: SQL-Database beheerd exemplaar Veelgestelde vragen (FAQ)
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 07/08/2019
ms.openlocfilehash: c3a070eb7e1435055b47b39985cf8cb0b182a514
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798063"
---
# <a name="sql-database-managed-instance-frequently-asked-questions-faq"></a>SQL-Database beheerd exemplaar Veelgestelde vragen (FAQ)

In dit artikel bevat een groot aantal van de meest voorkomende vragen over [SQL-Database beheerd exemplaar](sql-database-managed-instance.md).

## <a name="where-can-i-find-a-list-of-features-that-are-supported-on-managed-instance"></a>Waar vind ik een lijst met functies die worden ondersteund op een beheerd exemplaar?

Zie voor een lijst van ondersteunde functies in beheerd exemplaar, [Azure SQL Database versus SQL Server](sql-database-features.md).

Zie voor verschillen in syntaxis en het gedrag tussen Azure SQL Database managed instance en on-premises SQL Server, [T-SQL-verschillen van SQL Server](sql-database-managed-instance-transact-sql-information.md).


## <a name="where-can-i-find-technical-characteristics-and-resource-limits-for-managed-instance"></a>Waar vind ik technische kenmerken en resourcelimieten voor het beheerde exemplaar?

Zie voor kenmerken van de beschikbare hardware-generatie, [technische verschillen in hardwaregeneraties](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).
Zie voor beschikbare service-lagen en hun kenmerken [technische verschillen tussen service-lagen](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

## <a name="where-can-i-find-known-issues-and-bugs"></a>Waar vind ik fouten en bekende problemen?

Zie voor bugs en bekende problemen, [wijzigingen](sql-database-managed-instance-transact-sql-information.md#Changes) en [bekende problemen](sql-database-managed-instance-transact-sql-information.md#Issues).


## <a name="can-a-managed-instance-have-the-same-name-as-on-premises-sql-server"></a>Kan een beheerd exemplaar hebben dezelfde naam als de on-premises SQL Server?

Beheerd exemplaar moet een naam hebben die eindigt op *database.windows.net*. Een andere DNS-zone gebruiken in plaats van de standaard, bijvoorbeeld **mi een andere naam**. contoso.com: 
- CliConfig gebruiken voor het definiëren van een alias (het hulpprogramma is alleen een wrapper voor Register-instellingen, zodat dit kan ook worden gedaan met behulp van Groepsbeleid of script).
- Gebruik *CNAME* met *TrustServerCertificate = true* optie.


## <a name="how-can-i-move-database-from-managed-instance-back-to-sql-server-or-azure-sql-database"></a>Hoe kan ik database vanuit beheerde exemplaar terug verplaatsen naar SQL Server of Azure SQL Database?

U kunt [-database exporteren naar bacpac](sql-database-export.md) en vervolgens [het bacpac-bestand importeren]( sql-database-import.md). Deze aanpak wordt aanbevolen als uw database kleiner dan 100 GB is.

Transactionele replicatie kan worden gebruikt als alle tabellen in de database primaire sleutels hebben.

Systeemeigen `COPY_ONLY` back-ups die zijn overgenomen uit het beheerde exemplaar kunnen niet worden hersteld naar SQL Server omdat het beheerde exemplaar heeft een hogere ten opzichte van SQL Server-databaseversie.

## <a name="how-can-i-migrate-my-instance-database-to-a-single-azure-sql-database"></a>Hoe kan ik mijn exemplaar in de database migreren naar één Azure SQL Database?

Een optie is om te [exporteren van de database naar een bacpac](sql-database-export.md) en vervolgens [het bacpac-bestand importeren]( sql-database-import.md). 

Dit is de aanbevolen aanpak als uw database kleiner dan 100 GB is. Transactionele replicatie kan worden gebruikt als alle tabellen in de database primaire sleutels hebben.

## <a name="how-do-i-choose-between-gen-4-and-gen-5-hardware-generation-for-managed-instance"></a>Hoe ik kiezen tussen Gen 4 en Gen 5 hardware te genereren voor het beheerde exemplaar?

Dit is afhankelijk van uw werkbelasting als bepaalde hardware-generatie beter geschikt voor bepaalde soorten werkbelastingen dan de andere is. Terwijl het onderwerp van prestaties in plaats van een complex is om te vereenvoudigen, de volgende verschillen tussen de hardwaregeneraties die betrekking hebben op de prestaties van de werkbelastingen:
- Gen 4 biedt een betere ondersteuning voor compute zoals deze is gebaseerd op fysieke processors, versus Gen 5 die is gebaseerd op vCore-processors. Dit is mogelijk meer nuttig zijn voor het berekenen van intensieve workloads.
- Gen 5 ondersteunt versnelde netwerken leidt tot een betere i/o-bandbreedte naar de externe opslag. Dit kan zijn nuttig voor i/o-intensieve werkbelastingen op de Servicelagen voor algemeen gebruik. Gen 5 maakt gebruik van lokale schijven van snellere SSD in vergelijking met Gen 4. Dit kan zijn nuttig voor i/o-intensieve werkbelastingen op de kritieke zakelijke-Servicelagen.

Klanten wordt aangeraden voor het testen van de prestaties van de werkelijke workloads bedoeld voor productie voordat het live zetten om te bepalen welke hardware generatie beter geschikt in uw situatie.

## <a name="can-i-switch-my-managed-instance-hardware-generation-between-gen-4-and-gen-5-online"></a>Kan ik mijn beheerd exemplaar hardware genereren van tussen Gen 4 en Gen 5 online wisselen? 

Automatische online schakelen tussen hardwaregeneraties is mogelijk als beide hardwaregeneraties zijn beschikbaar in dezelfde regio waar uw beheerde exemplaar is ingericht. In dit geval hebt u een optie in de prijzen laag-sectie van de Azure-portal om over te schakelen tussen hardwaregeneraties.

Dit is een langdurige bewerking naar de nieuwe beheerde instantie worden ingericht op de back-end en de databases automatisch uitgevoerd overgedragen tussen de oude en nieuwe instantie. Dit proces worden naadloze voor klanten.

Als beide hardwaregeneraties worden niet ondersteund in dezelfde regio, wijzigt de generatie hardware is mogelijk maar moet handmatig worden gedaan. Hiervoor moet u voor het inrichten van een nieuw exemplaar in de regio waar de generatie van de gewenste hardware beschikbaar is, en handmatig een back-ups maken en herstellen van gegevens tussen de oude en nieuwe instantie.


## <a name="how-do-i-tune-performance-of-my-managed-instance"></a>Hoe ik in het afstemmen van prestaties van mijn beheerd exemplaar? 

Beheerd exemplaar voor algemeen gebruik gebruikt externe opslag vanwege die grootte van gegevens en logboekbestanden op de prestaties van belang is. Volg de instructies in dit blogbericht om af te stemmen laag voor algemeen gebruik serviceprestaties.

Voor i/o-intensieve workloads kunt u overwegen Gen 5-hardware, ten opzichte van Gen 4 voor berekenen van intensieve workloads. Voor meer informatie, Zie de sectie Veelgestelde vragen over het kiezen tussen hardwaregeneraties.

Als uw werkbelasting van veel kleine transacties bestaat, overweeg over te schakelen van het verbindingstype van proxy omleiden modus.

## <a name="what-is-the-maximum-storage-size-for-managed-instance"></a>Wat is de maximum opslagruimte voor het beheerde exemplaar? 

Maximale grootte voor het beheerde exemplaar is afhankelijk van de geselecteerde servicelaag (algemeen gebruik en bedrijfskritiek). Zie voor de beperkingen van de opslag van deze service-lagen, [Service tier kenmerk](sql-database-service-tiers-general-purpose-business-critical.md).

## <a name="is-the-backup-storage-deducted-from-my-managed-instance-storage"></a>Is de back-upopslag in mindering gebracht op mijn opslag beheerd exemplaar? 

Nee, back-upopslag niet worden afgetrokken van uw opslagruimte in beheerd exemplaar. De back-upopslag is onafhankelijk van de opslagruimte van het exemplaar en is niet in grootte beperkt. Back-upopslag wordt beperkt door de tijd voor het bewaren van de back-up van uw exemplaar van databases, configureerbare van 7 tot 35 dagen. Zie voor meer informatie, [geautomatiseerde back-ups](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
  
## <a name="how-can-i-set-inbound-nsg-rules-on-management-ports"></a>Hoe stel ik inkomende NSG-regels op beheerpoorten?

De functie ingebouwde firewall configureert Windows firewall op alle virtuele machines in het cluster waarmee inkomende verbindingen met IP-adresbereiken die alleen voor Microsoft management/implementatie machines en beveiligde beheerwerkstations effectief te voorkomen dat is gekoppeld indringers via de netwerklaag.

Hier ziet welke poorten worden gebruikt voor:

Poorten 9000 en 9003 worden gebruikt door de Service Fabric-infrastructuur. Primaire rol van service Fabric is de virtuele-cluster in orde te houden en doel staat wat betreft het aantal replica's onderdeel te houden.

Poorten 1438 en 1440 1452 worden gebruikt door de Agent-knooppunt. Knooppuntagent is een toepassing die wordt uitgevoerd binnen het cluster en wordt gebruikt door het besturingselement vlak voor het uitvoeren van opdrachten voor beheer.

Naast de ingebouwde firewall op de netwerklaag, wordt ook communicatie beschermd met certificaten.
  
Zie voor meer informatie en het controleren van de ingebouwde firewall [Azure SQL Database managed instance ingebouwde firewall](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).


## <a name="how-can-i-mitigate-networking-risks"></a>Hoe kan ik netwerken risico's verhelpen? 

Alle netwerken om risico te beperken, wordt u aangeraden een set van beveiligingsinstellingen en -besturingselementen toepassen:

- Schakel alle databases op transparante gegevensversleuteling (TDE).
- Schakel uit Common Language Runtime (CLR). Dit wordt on-premises ook aanbevolen.
- Gebruik Azure AD-accounts.
- Toegang SQL MI met DBA account met beperkte bevoegdheden.
- Toegang van jumpbox tot JiT voor sysadmin-account configureren.
- Schakel SQL-controle en integreren met waarschuwingssystemen mechanismen.
- Schakel detectie van bedreigingen van ATS-suite.


## <a name="where-can-i-find-use-cases-and-resulting-cost-savings-with-managed-instance"></a>Waar vind ik use cases en resulterende kosten te besparen met beheerd exemplaar?

Casestudy's beheerd exemplaar:

- [Komatsu](http://customers.microsoft.com/story/komatsu-australia-manufacturing-azure)
- [powerdetails](http://customers.microsoft.com/story/powerdetails-partner-professional-services-azure-sql-database-managed-instance)
- [Allscripts](http://customers.microsoft.com/story/allscripts-partner-professional-services-azure)   
Als u een beter begrip van de voordelen, kosten en risico's die zijn gekoppeld aan het implementeren van beheerd exemplaar voor Azure SQL Database, is er ook een Forresters casestudy over: [Totale economische Impact van MI](https://azure.microsoft.com/resources/forrester-tei-sql-database-managed-instance).


## <a name="can-i-do-dns-refresh"></a>Kan ik DNS vernieuwen? 
  
Op dit moment bieden we geen een functie voor het vernieuwen van de configuratie van de DNS-server voor het beheerde exemplaar.

DNS-configuratie wordt uiteindelijk vernieuwd:

- Wanneer de DHCP-lease is verlopen.
- Bij een platformupgrade.

Downgrade van het beheerde exemplaar in 4 vCore als tijdelijke oplossing, en daarna opnieuw een upgrade. Dit heeft een neveneffect van het vernieuwen van de DNS-configuratie.


## <a name="can-a-managed-instance-have-a-static-ip-address"></a>Kan een beheerd exemplaar van een statisch IP-adres hebben?

In zeldzame maar nodig situaties moet er een online migratie van een beheerd exemplaar naar een nieuw virtueel cluster uitvoeren. Indien nodig, is deze migratie vanwege wijzigingen in onze technologiestack gericht voor betere beveiliging en betrouwbaarheid van de service. Migreren naar een nieuwe virtuele cluster resultaten in het wijzigen van de IP-adres dat is toegewezen aan de naam van het beheerde exemplaar host. De service beheerd exemplaar biedt geen ondersteuning voor statische IP-adres niet claimen en behoudt zich het recht voor deze zonder kennisgeving worden gewijzigd als onderdeel van regelmatig onderhoudscycli.

Om deze reden wordt ten zeerste ontmoedigen vertrouwen op Onveranderbaarheid van het IP-adres, omdat dit leiden onnodige uitvaltijd tot kan.


## <a name="can-i-change-the-time-zone-for-an-existing-managed-instance"></a>Kan ik de tijdzone voor een bestaande beheerde exemplaar wijzigen?

Tijdzoneconfiguratie kan worden ingesteld als een beheerd exemplaar voor de eerste keer is ingericht. Wijzigen van de tijdzone van de bestaande beheerde instantie wordt niet ondersteund. Zie voor meer informatie, [tijdzone beperkingen](sql-database-managed-instance-timezone.md#limitations).

Oplossingen omvatten het maken van een nieuwe beheerd exemplaar met de juiste tijdzone en een voert een handmatige back-up en terugzetten, of wat wordt aanbevolen, voeren een [cross-instance point-in-time restore](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/).


## <a name="how-do-i-resolve-performance-issues-with-my-managed-instance"></a>Hoe los ik problemen met prestaties met mijn beheerd exemplaar

Voor een vergelijking van de prestaties tussen beheerd exemplaar en SQL Server, een goed uitgangspunt is [aanbevolen procedures voor prestaties vergelijking tussen Azure SQL managed instance en SQL Server](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/The-best-practices-for-performance-comparison-between-Azure-SQL/ba-p/683210).

Laden van gegevens zijn vaak langzamer zijn op een beheerd exemplaar dan in SQL Server vanwege een verplichte volledig-herstelmodel en [limieten](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics) op transactie logboek schrijven-doorvoer. Soms, kan dit worden gewerkt rond met geclusterde columnstore- of tabellen geoptimaliseerd voor geheugen of tijdelijke gegevens in tempdb in plaats van de gebruikersdatabase te laden.


## <a name="can-i-restore-my-encrypted-database-to-managed-instance"></a>Kan ik mijn versleutelde database terugzetten naar beheerd exemplaar?

Ja, moet u niet decoderen van de database als u wilt herstellen naar beheerd exemplaar. U hoeft te geven van een certificaat/sleutel die u als een sleutelbeveiliging versleuteling wordt gebruikt, in het bronsysteem met het beheerde exemplaar moeten kunnen gegevens lezen uit het versleutelde back-upbestand. Er zijn twee manieren om dit te doen:

- Certificaat sleutelbeveiliging uploaden naar het beheerde exemplaar. Dit is mogelijk alleen met behulp van PowerShell. Het voorbeeld van een script wordt het hele proces beschreven.
- Asymmetrische sleutelbeveiliging uploaden naar Azure Key Vault (AKV) en wijst het beheerd exemplaar. Deze aanpak is vergelijkbaar met bring-your-own-key (BYOK) TDE use-case die ook gebruikmaakt van Azure Sleutelkluis-integratie voor het opslaan van de versleutelingssleutel. Als u wilt dat alleen de sleutel die is geüpload naar Azure Sleutelkluis beschikbaar is voor beheerd exemplaar voor het herstellen van versleutelde databases zonder daadwerkelijk met behulp van de sleutel als een sleutelbeveiliging versleuteling, volg de instructies voor het instellen van BYOK TDE en geen, schakel het selectievakje in de geselecteerde sleutel maken de Standaard TDE-beveiliging.

Nadat u de versleuteling sleutelbeveiliging beschikbaar voor beheerd exemplaar maken, kunt u doorgaan met de standard-database terugzetten procedure.

## <a name="how-can-i-migrate-from-azure-sql-database-single-or-elastic-pool-to-managed-instance"></a>Hoe kan ik migreren uit Azure SQL Database enkele of elastische pool naar beheerd exemplaar? 

Beheerd exemplaar biedt de dezelfde prestaties per reken- en grootte als andere implementatie-opties van Azure SQL Database. Als u wilt samenvoegen van gegevens op één exemplaar of u alleen een functie die wordt ondersteund uitsluitend in het beheerde exemplaar hoeft, kunt u uw gegevens kunt migreren met behulp van de functionaliteit voor exporteren/importeren (BACPAC).
