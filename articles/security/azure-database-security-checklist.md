---
title: Controlelijst voor de beveiliging van de Azure-database | Microsoft Docs
description: In dit artikel biedt een reeks controlelijst voor beveiliging van de Azure-database.
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: 37523506369ece144183dba76feb8b23b6c1acb2
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-database-security-checklist"></a>Controlelijst voor de beveiliging van de Azure-database

Om beveiliging te verbeteren, Azure-Database bevat een aantal ingebouwde beveiligingsmechanismen die u kunt gebruiken om te beperken en beheren van toegang.

Deze omvatten:

-   Een firewall die u kunt maken [firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) connectiviteit beperken door IP-adres
-   Niveau van de Server firewall toegankelijk is vanaf de Azure-portal
-   Firewallregels voor databaseniveau toegankelijk is vanaf SSMS
-   Beveiligde verbinding met de database met de beveiligde verbindingsreeksen
-   Toegangsbeheer gebruiken
-   Gegevensversleuteling
-   Controle van SQL-Database
-   Detectie van dreigingen SQL-Database

## <a name="introduction"></a>Inleiding
Cloud computing nieuwe beveiliging paradigma's die niet bekend te veel gebruikers van de toepassing, databasebeheerders en programmeurs zijn vereist. Als gevolg hiervan zijn sommige organisaties terughoudend voor het implementeren van een cloudinfrastructuur voor gegevensbeheer vanwege merkbare beveiligingsrisico's. Veel van dit probleem kan echter worden ondervangen via een beter begrip van de beveiligingsfuncties die is ingebouwd in Microsoft Azure en Microsoft Azure SQL Database.

## <a name="checklist"></a>Controlelijst
Het is raadzaam dat u leest de [Azure Security Best Practices](https://docs.microsoft.com/azure/security/azure-database-security-best-practices) artikel voordat deze controlelijst controleren. U kunt zich Maak optimaal gebruik van deze controlelijst nadat u de best practices begrijpt. Vervolgens kunt u deze controlelijst om ervoor te zorgen dat u de belangrijke problemen in Azure databasebeveiliging hebt verholpen.


|Controlelijst categorie| Beschrijving|
| ------------ | -------- |
|**Gegevens beveiligen**||
| <br> Codering in beweging/doorvoer| <ul><li>[Transport Layer Security](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol), om gegevens te coderen wanneer gegevens worden verplaatst naar de netwerken.</li><li>Database vereist een beveiligde communicatie van clients op basis van de [TDS (Tabular Data Stream)](https://msdn.microsoft.com/en-in/library/dd357628.aspx) protocol via TLS (Transport Layer Security).</li></ul> |
|<br>Versleuteling 'at rest'| <ul><li>[Met transparante gegevensversleuteling](http://go.microsoft.com/fwlink/?LinkId=526242)wanneer inactieve gegevens fysiek worden opgeslagen in een digitale vorm.</li></ul>|
|**Toegang beheren**||  
|<br> Toegang tot de database | <ul><li>[Verificatie](https://docs.microsoft.com/azure/sql-database/sql-database-control-access) (Azure Active Directory-verificatie) AD-verificatie wordt gebruikgemaakt van identiteiten die worden beheerd door Azure Active Directory.</li><li>[Autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-control-access) Verleen gebruikers de minimaal benodigde bevoegdheden nodig zijn.</li></ul> |
|<br>Toegang tot toepassingen| <ul><li>[Rij level Security](https://msdn.microsoft.com/library/dn765131) (met behulp van beveiligingsbeleid, op hetzelfde moment rijniveau toegang beperken op basis van een gebruiker identiteit, rol of uitvoering context).</li><li>[Dynamische-Gegevensmaskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) (met behulp van machtiging & beleid beperkt blootstelling van gevoelige gegevens door het maskeren voor onbevoegde gebruikers)</li></ul>|
|**Proactieve controle**||  
| <br>Bijhouden & detecteren| <ul><li>[Controle](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) houdt databasegebeurtenissen bij en schrijft deze naar een controlelogboek / activiteit log in uw [Azure Storage-account](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</li><li>Gebruik van track Azure Database health [Azure Monitor activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</li><li>[Bedreigingendetectie](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) worden afwijkende databaseactiviteiten die wijzen op beveiligingsdreigingen voor de database gedetecteerd. </li></ul> |
|<br>Azure Security Center| <ul><li>[Bewaking van de gegevens](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-databases) Azure Security Center gebruiken als een oplossing voor een gecentraliseerde veiligheidscontrole voor SQL en andere Azure-services.</li></ul>|       

## <a name="conclusion"></a>Conclusie
Azure-Database is een databaseplatform robuuste met een volledige reeks beveiligingsfuncties die voldoen aan veel organisatie- en regelgeving nalevingsvereisten. U kunt eenvoudig gegevens beveiligen door de fysieke toegang tot uw gegevens beheren en het gebruik van een groot aantal opties voor beveiliging van gegevens op het file-, kolom- of met transparante gegevensversleuteling, -Celcodering of beveiliging op rijniveau. Altijd kan versleutelde ook bewerkingen op de versleutelde gegevens, het proces van toepassingsupdates vereenvoudigen. Op zijn beurt biedt toegang tot de controlelogboeken van de activiteit van de SQL-Database u de informatie die u nodig hebt, zodat u weet hoe en wanneer gegevens worden geopend.

## <a name="next-steps"></a>Volgende stappen
U kunt de beveiliging van uw database tegen kwaadwillende gebruikers of onbevoegde toegang met een paar eenvoudige stappen kunt verbeteren. In deze zelfstudie leert u naar:

- Instellen van [firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor de server en of de database.
- Uw gegevens beschermen met [versleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-encryption).
- Schakel [SQL Database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing).

