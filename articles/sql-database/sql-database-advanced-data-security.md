---
title: Geavanceerde beveiliging van gegevens - Azure SQL Database | Microsoft Docs
description: Meer informatie over de functionaliteit voor het detecteren en classificeren van gevoelige gegevens, beheren van uw databaseproblemen en opsporen van afwijkende activiteiten die op een bedreiging voor uw Azure SQL database wijzen kunnen.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: conceptual
author: monhaber
ms.author: monhaber
ms.reviewer: vanto
manager: craigg
ms.date: 03/31/2019
ms.openlocfilehash: a078ac38cef5b395a19481188c474c7f908160d5
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58791606"
---
# <a name="advanced-data-security-for-azure-sql-database"></a>Geavanceerde beveiliging voor Azure SQL Database

Geavanceerde beveiliging is een geïntegreerde-pakket voor geavanceerde mogelijkheden voor de beveiliging van SQL. Het bevat functionaliteit voor het detecteren en classificeren van gevoelige gegevens, zichtbaar te maken en beperkende potentiële databaseproblemen, en het opsporen van afwijkende activiteiten die op een bedreiging voor uw database wijzen kunnen. Het is tevens een centraal punt voor het inschakelen en beheren van deze mogelijkheden.

## <a name="overview"></a>Overzicht

Geavanceerde beveiliging (AD) biedt een geavanceerde SQL-beveiligingsmogelijkheden, met inbegrip van gegevensdetectie en classificatie, evaluatie van beveiligingsproblemen en Advanced Threat Protection.

- [Gegevensdetectie en classificatie](sql-database-data-discovery-and-classification.md) (momenteel in preview) biedt mogelijkheden die zijn ingebouwd in Azure SQL Database voor het detecteren, classificeren, labelen en beveiligen van de gevoelige gegevens in uw databases. Het kan worden gebruikt voor het zichtbaar maken van de classificatiestatus van gegevens in uw database, en het traceren van de toegang tot gevoelige gegevens binnen en buiten de database.
- [Evaluatie van beveiligingsproblemen](sql-vulnerability-assessment.md) is een eenvoudig te configureren van de service die u kunt detecteren, volgen en u helpen potentiële databaseproblemen oplossen. Deze service biedt u inzicht in de status van de beveiliging en bruikbare stappen om beveiligingsproblemen op te lossen en de beveiliging van uw database te verbeteren.
- [Advanced Threat Protection](sql-database-threat-detection-overview.md) detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van uw database aangeeft. Hiermee wordt uw database continu gecontroleerd op verdachte activiteiten en wordt u onmiddellijk gewaarschuwd bij mogelijke beveiligingsproblemen, SQL-injectieaanvallen en afwijkende databasetoegangspatronen. Advanced Threat Protection waarschuwingen Geef details op van de verdachte activiteit en geven aanbevelingen voor het onderzoeken en tegenhouden.

Schakel SQL ADVERTENTIES eenmaal zodat al deze functies opgenomen. Met één klik kunt ADVERTENTIES inschakelen voor alle databases op uw SQL Database-server of beheerd exemplaar. In- of beheren van instellingen voor ADVERTENTIES is vereist tot behoort de [SQL security manager](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) rol, beheerdersrol voor SQL database of SQL server-beheerdersrol. 

ADVERTENTIES prijzen worden uitgelijnd met Azure Security Center standard-laag, waarbij elke beveiligde SQL Database-server of het beheerde exemplaar wordt geteld als één knooppunt. Nieuw beveiligde resources in aanmerking komen voor een gratis proefversie van Security Center standard-laag. Zie voor meer informatie de [pagina met prijzen van Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-ads"></a>Aan de slag met ADVERTENTIES

De volgende stappen uit om u aan de slag met Active Directory.

## <a name="1-enable-ads"></a>1. ADVERTENTIES inschakelen

Inschakelen van ADVERTENTIES door te navigeren naar **gegevensbeveiliging geavanceerde** onder de **Security** kop voor uw SQL Database-server of het beheerde exemplaar. Om in te schakelen ADVERTENTIES voor alle databases op de database-server of het beheerde exemplaar, klikt u op **geavanceerde gegevensbeveiliging inschakelen op de server**.

> [!NOTE]
> Een storage-account automatisch wordt gemaakt en geconfigureerd voor het opslaan van uw **evaluatie van beveiligingsproblemen** scanresultaten. Als u hebt AD al ingeschakeld voor een andere server in dezelfde resourcegroep en regio, wordt het bestaande storage-account gebruikt.

![ADVERTENTIES inschakelen](./media/sql-advanced-protection/enable_ads.png) 

> [!NOTE]
> De kosten van ADVERTENTIES wordt uitgelijnd met Azure Security Center standard-laag-prijzen per knooppunt, waarbij een knooppunt de volledige SQL-Database-server of het beheerde exemplaar is. U wordt dus slechts één keer betaalt voor het beveiligen van alle databases op de database-server of het beheerde exemplaar met ADVERTENTIES. U kunt uitproberen ADVERTENTIES in eerste instantie met een gratis proefversie.

## <a name="2-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>2. Start gegevens classificeren en onderzoeken van waarschuwingen van threat beveiligingsproblemen bijhouden

Klik op de **gegevensdetectie en classificatie** kaart om te zien aanbevolen gevoelige kolommen te classificeren en om uw gegevens met permanente gevoeligheidslabels te classificeren. Klik op de **evaluatie van beveiligingsproblemen** kaart weergeven en beheren door een beveiligingslek scans en de rapporten en voor het bijhouden van uw waarmee beveiliging kan worden voldaan. Als u beveiligingswaarschuwingen zijn ontvangen, klikt u op de **Advanced Threat Protection** kaart om weer te geven details van de waarschuwingen en voor een geconsolideerde rapport over alle waarschuwingen in uw Azure-abonnement via de pagina met Azure Security Center security waarschuwingen .

## <a name="3-manage-ads-settings-on-your-sql-database-server-or-managed-instance"></a>3. Beheren van instellingen voor ADVERTENTIES op uw SQL Database-server of beheerd exemplaar

Als u wilt weergeven en beheren van instellingen voor ADVERTENTIES, gaat u naar **gegevensbeveiliging geavanceerde** onder de **Security** kop voor uw SQL Database-server of het beheerde exemplaar. Op deze pagina kunt u inschakelen of uitschakelen van ADVERTENTIES en wijzigen door een beveiligingslek beoordeling en instellingen voor Advanced Threat Protection voor uw hele SQL Database-server of het beheerde exemplaar.

![Serverinstellingen](./media/sql-advanced-protection/server_settings.png) 

## <a name="4-manage-ads-settings-for-a-sql-database"></a>4. Instellingen voor ADVERTENTIES voor SQL database beheren

Als u wilt onderdrukken ADS-instellingen voor een bepaalde database, controleert u de **geavanceerde gegevensbeveiliging inschakelen op databaseniveau** selectievakje. Gebruik deze optie alleen als u een bepaalde vereiste voor het ontvangen van afzonderlijke Advanced Threat Protection-waarschuwingen of beveiligingslek met betrekking tot de resultaten van de beoordeling voor de individuele database, in plaats van of naast de waarschuwingen en de resultaten die zijn ontvangen voor alle databases op de database-server of het beheerde exemplaar.

Wanneer het selectievakje is ingeschakeld, kunt u vervolgens de relevante instellingen voor deze database.
 
![Instellingen voor database- en Advanced Threat Protection](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Geavanceerde beveiligingsinstellingen voor uw database-server of het beheerde exemplaar kunnen ook worden bereikt vanaf het deelvenster van de database ADVERTENTIES. Klik op **instellingen** in het hoofdvenster van ADVERTENTIES en klik vervolgens op **weergave Geavanceerd gegevensbeveiliging serverinstellingen**. 

![Database-instellingen](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Volgende stappen 

- Meer informatie over [gegevensdetectie en classificatie](sql-database-data-discovery-and-classification.md) 
- Meer informatie over [evaluatie van beveiligingsproblemen](sql-vulnerability-assessment.md) 
- Meer informatie over [Advanced Threat Protection](sql-database-threat-detection.md)
- Meer informatie over [Azure security center](https://docs.microsoft.com/azure/security-center/security-center-intro)
