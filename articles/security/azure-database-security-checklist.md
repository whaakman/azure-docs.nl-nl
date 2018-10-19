---
title: Controlelijst voor de beveiliging van Azure-database | Microsoft Docs
description: In dit artikel biedt een set met controlelijst voor beveiliging van de Azure database.
services: security
documentationcenter: na
author: unifycloud
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: f6e8ed1725b7ba2a0cba8523ae03c0306337aba0
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49402504"
---
# <a name="azure-database-security-checklist"></a>Controlelijst voor de beveiliging van Azure-database

Om beveiliging te verbeteren, Azure-Database bevat een aantal ingebouwde beveiligingsmaatregelen die u kunt gebruiken om te beperken en beheren van toegang.

Deze omvatten:

-   Een firewall die u kunt maken [firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) connectiviteit beperken door IP-adres.
-   De firewall op serverniveau toegankelijk is vanaf de Azure-portal
-   De firewall op databaseniveau regels toegankelijk via SSMS
-   Beveiligde verbinding met uw database met behulp van veilige verbindingsreeksen
-   Access management gebruiken
-   Gegevensversleuteling
-   SQL Database Auditing
-   Detectie van bedreigingen voor SQL-Database

## <a name="introduction"></a>Inleiding
Cloud computing vereist nieuwe beveiliging paradigma's die niet veel gebruikers van de toepassing, databasebeheerders en programmeurs bekend zijn. Als gevolg hiervan zijn sommige organisaties willen implementeren van een cloud-infrastructuur voor gegevensbeheer vanwege waargenomen beveiligingsrisico's. Veel van dit probleem kan echter worden verholpen door een beter begrip van de beveiligingsfuncties die is ingebouwd in Microsoft Azure en Microsoft Azure SQL Database.

## <a name="checklist"></a>Controlelijst
Het is raadzaam dat u leest de [Best Practices voor beveiliging op Azure-Database](https://docs.microsoft.com/azure/security/azure-database-security-best-practices) artikel voordat u deze controlelijst bekijken. Kunt u zich kunt u optimaal te profiteren van deze controlelijst wanneer u bekend bent met de aanbevolen procedures. U kunt vervolgens deze controlelijst om ervoor te zorgen dat u de belangrijke problemen in Azure database-beveiliging hebt behandeld.


|Controlelijst voor categorie| Beschrijving|
| ------------ | -------- |
|**Gegevens beveiligen**||
| <br> Versleuteling in beweging/doorvoer| <ul><li>[Transport Layer Security](https://docs.microsoft.com/windows-server/security/tls/transport-layer-security-protocol), voor gegevensversleuteling bij het verplaatsen van gegevens naar de netwerken.</li><li>Database is vereist voor veilige communicatie van clients op basis van de [TDS (Tabular Data Stream)](https://msdn.microsoft.com/library/dd357628.aspx) protocol via TLS (Transport Layer Security).</li></ul> |
|<br>Versleuteling 'at rest'| <ul><li>[De Transparent Data Encryption](http://go.microsoft.com/fwlink/?LinkId=526242)als inactieve gegevens fysiek worden opgeslagen in een digitale vorm.</li></ul>|
|**Toegang beheren**||  
|<br> Toegang tot de database | <ul><li>[Verificatie](https://docs.microsoft.com/azure/sql-database/sql-database-control-access) (Azure Active Directory-verificatie) AD-verificatie wordt gebruikt voor identiteiten die worden beheerd door Azure Active Directory.</li><li>[Autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-control-access) gebruikers de minimaal benodigde bevoegdheden verlenen.</li></ul> |
|<br>Toegang tot toepassingen| <ul><li>[Beveiliging op rij](https://msdn.microsoft.com/library/dn765131) (met behulp van beveiligingsbeleid op hetzelfde moment beperken van de beveiliging op rijniveau toegang op basis van de identiteit, rol of uitvoering context van een gebruiker).</li><li>[Dynamic Data Masking](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) (met behulp van machtigingen en -beleid, blootstelling van gevoelige gegevens door deze te maskeren voor niet-gemachtigde gebruikers)</li></ul>|
|**Proactieve controle**||  
| <br>Volgen & detecteren| <ul><li>[Controle](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) databasegebeurtenissen en schrijft deze naar een auditlogboek / activiteitenlogboek uw [Azure Storage-account](https://docs.microsoft.com/azure/storage/storage-create-storage-account).</li><li>Spoor Azure Database health gebruiken [activiteitenlogboeken van Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</li><li>[Detectie van bedreigingen](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de database. </li></ul> |
|<br>Azure Security Center| <ul><li>[Bewaking van de gegevens](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-databases) Azure Security Center gebruiken als een gecentraliseerde beveiliging voor SQL en andere Azure-services voor controle.</li></ul>|       

## <a name="conclusion"></a>Conclusie
Azure Database is een krachtige databaseplatform, met een volledige reeks beveiligingsfuncties die voldoen aan veel organisatie en wettelijke nalevingsvereisten. U kunt eenvoudig gegevens beschermen door de fysieke toegang tot uw gegevens beheren en het gebruik van een verscheidenheid aan opties voor beveiliging van gegevens op het bestand-, kolom- of beveiliging op rijniveau met Transparent Data Encryption, versleuteling op celniveau of beveiliging op rijniveau. Altijd kan versleutelde ook bewerkingen op de versleutelde gegevens, het proces van updates van toepassingen vereenvoudigen. Op zijn beurt biedt toegang tot de logboeken van de activiteit van SQL Database auditing u de informatie die u nodig hebt, zodat u weet hoe en wanneer gegevens worden geopend.

## <a name="next-steps"></a>Volgende stappen
U hoeft slechts een paar eenvoudige stappen uit te voeren om de beveiliging van uw database tegen kwaadwillende gebruikers of onbevoegde toegang te verbeteren. In deze zelfstudie leert u het volgende:

- Instellen van [firewall-regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor uw server en/of database.
- Uw gegevens beschermen met [versleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-encryption).
- Schakel [SQL Database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing).

