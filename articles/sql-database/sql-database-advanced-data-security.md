---
title: Geavanceerde beveiliging van gegevens - Azure SQL Database | Microsoft Docs
description: Meer informatie over de functionaliteit voor het detecteren en classificeren van gevoelige gegevens, beheren van uw databaseproblemen en opsporen van afwijkende activiteiten die op een bedreiging voor uw Azure SQL database wijzen kunnen.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 02/04/2019
ms.openlocfilehash: 388a1a2ff70f4d00e66511811bffdb76806ed713
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735681"
---
# <a name="advanced-data-security-for-azure-sql-database"></a>Geavanceerde beveiliging voor Azure SQL Database

Geavanceerde beveiliging is een geïntegreerde-pakket voor geavanceerde mogelijkheden voor de beveiliging van SQL. Het bevat functionaliteit voor het detecteren en classificeren van gevoelige gegevens, zichtbaar te maken en beperkende potentiële databaseproblemen, en het opsporen van afwijkende activiteiten die op een bedreiging voor uw database wijzen kunnen. Het is tevens een centraal punt voor het inschakelen en beheren van deze mogelijkheden.

## <a name="overview"></a>Overzicht

Geavanceerde beveiliging (AD) biedt een geavanceerde SQL-beveiligingsmogelijkheden, met inbegrip van gegevensdetectie en classificatie, evaluatie van beveiligingsproblemen en detectie van bedreigingen.

- [Gegevensdetectie en classificatie](sql-database-data-discovery-and-classification.md) (momenteel in preview) biedt mogelijkheden die zijn ingebouwd in Azure SQL Database voor het detecteren, classificeren, labelen en beveiligen van de gevoelige gegevens in uw databases. Het kan worden gebruikt voor het zichtbaar maken van de classificatiestatus van gegevens in uw database, en het traceren van de toegang tot gevoelige gegevens binnen en buiten de database.
- [Evaluatie van beveiligingsproblemen](sql-vulnerability-assessment.md) is een eenvoudig te configureren van de service die u kunt detecteren, volgen en u helpen potentiële databaseproblemen oplossen. Deze service biedt u inzicht in de status van de beveiliging en bruikbare stappen om beveiligingsproblemen op te lossen en de beveiliging van uw database te verbeteren.
- [Detectie van bedreigingen](sql-database-threat-detection-overview.md) detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van uw database aangeeft. Hiermee wordt uw database continu gecontroleerd op verdachte activiteiten en wordt u onmiddellijk gewaarschuwd bij mogelijke beveiligingsproblemen, SQL-injectieaanvallen en afwijkende databasetoegangspatronen. Meldingen voor geconstateerde bedreigingen Geef details op van de verdachte activiteit en geven aanbevelingen voor het onderzoeken en tegenhouden.

Schakel SQL ADVERTENTIES eenmaal zodat al deze functies opgenomen. Met één klik kunt ADVERTENTIES inschakelen voor alle databases op uw SQL Database-server of beheerd exemplaar. In- of beheren van instellingen voor ADVERTENTIES is vereist tot behoort de [SQL security manager](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) rol, beheerdersrol voor SQL database of SQL server-beheerdersrol. 

ADVERTENTIES prijzen worden uitgelijnd met Azure Security Center standard-laag, waarbij elke beveiligde SQL Database-server of het beheerde exemplaar wordt geteld als één knooppunt. Nieuw beveiligde resources in aanmerking komen voor een gratis proefversie van Security Center standard-laag. Zie voor meer informatie de [pagina met prijzen van Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-ads"></a>Aan de slag met ADVERTENTIES

De volgende stappen uit om u aan de slag met Active Directory.

## <a name="1-enable-ads"></a>1. ADVERTENTIES inschakelen

Inschakelen van ADVERTENTIES door te navigeren naar **gegevensbeveiliging geavanceerde** onder de **Security** kop voor uw SQL Database-server of het beheerde exemplaar. Om in te schakelen ADVERTENTIES voor alle databases op de database-server of het beheerde exemplaar, klikt u op **geavanceerde gegevensbeveiliging inschakelen op de server**.

![ADVERTENTIES inschakelen](./media/sql-advanced-protection/enable_atp.png) 

> [!NOTE]
> De kosten van ADVERTENTIES wordt uitgelijnd met Azure Security Center standard-laag-prijzen per knooppunt, waarbij een knooppunt de volledige SQL-Database-server of het beheerde exemplaar is. U wordt dus slechts één keer betaalt voor het beveiligen van alle databases op de database-server of het beheerde exemplaar met ADVERTENTIES. U kunt uitproberen ADVERTENTIES in eerste instantie met een gratis proefversie.

## <a name="2-configure-vulnerability-assessment"></a>2. Evaluatie van beveiligingsproblemen configureren

Als u wilt gaan met behulp van de evaluatie van beveiligingsproblemen, die u wilt configureren van een opslagaccount waarin de scanresultaten worden opgeslagen. Om dit te doen, klikt u op de kaart van de evaluatie van beveiligingsproblemen.

![Evaluatie van beveiligingsproblemen configureren](./media/sql-advanced-protection/configure_va.png) 

Selecteer of maak een opslagaccount voor het opslaan van scanresultaten. U kunt ook periodieke terugkerende scans het configureren van evaluatie van beveiligingsproblemen om uit te voeren automatisch scannen op één keer per week inschakelen. Een samenvatting van de scan resultaat wordt verzonden naar het e-mailadressen die u opgeeft.

![Instellingen voor evaluatie van beveiligingsproblemen](./media/sql-advanced-protection/va_settings.png) 

## <a name="3-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>3. Start gegevens classificeren en onderzoeken van waarschuwingen van threat beveiligingsproblemen bijhouden

Klik op de **gegevensdetectie en classificatie** kaart om te zien aanbevolen gevoelige kolommen te classificeren en om uw gegevens met permanente gevoeligheidslabels te classificeren. Klik op de **evaluatie van beveiligingsproblemen** kaart weergeven en beheren door een beveiligingslek scans en de rapporten en voor het bijhouden van uw waarmee beveiliging kan worden voldaan. Als u beveiligingswaarschuwingen zijn ontvangen, klikt u op de **detectie van bedreigingen** kaart om weer te geven details van de waarschuwingen en voor een geconsolideerde rapport over alle waarschuwingen in uw Azure-abonnement via de pagina met Azure Security Center security waarschuwingen.

## <a name="4-manage-ads-settings-on-your-sql-database-server-or-managed-instance"></a>4. Beheren van instellingen voor ADVERTENTIES op uw SQL Database-server of beheerd exemplaar

Als u wilt weergeven en beheren van instellingen voor ADVERTENTIES, gaat u naar **gegevensbeveiliging geavanceerde** onder de **Security** kop voor uw SQL Database-server of het beheerde exemplaar. Op deze pagina kunt u inschakelen of uitschakelen van ADVERTENTIES en wijzigen threat detection-instellingen voor uw hele SQL Database-server of het beheerde exemplaar.

![Serverinstellingen](./media/sql-advanced-protection/server_settings.png) 

## <a name="5-manage-ads-settings-for-a-sql-database"></a>5. Instellingen voor ADVERTENTIES voor SQL database beheren

Als u wilt onderdrukken ADS-instellingen voor een bepaalde database, controleert u de **geavanceerde gegevensbeveiliging inschakelen op databaseniveau** selectievakje. Gebruik deze optie alleen als u een bepaalde vereiste voor het ontvangen van afzonderlijke threat detectiewaarschuwingen voor de individuele database, in plaats van of naast de waarschuwingen ontvangen voor alle databases op de database-server of beheerd exemplaar. 

Nadat u het selectievakje is ingeschakeld, klikt u op **Threat Detection-instellingen voor deze database** en configureer vervolgens de relevante instellingen voor deze database.

![Database- en bedreigingsbeveiliging-detectie-instellingen](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Geavanceerde beveiligingsinstellingen voor uw database-server of het beheerde exemplaar kunnen ook worden bereikt vanaf het deelvenster van de database ADVERTENTIES. Klik op **instellingen** in het hoofdvenster van ADVERTENTIES en klik vervolgens op **weergave Geavanceerd gegevensbeveiliging serverinstellingen**. 

![Database-instellingen](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Volgende stappen 

- Meer informatie over [gegevensdetectie en classificatie](sql-database-data-discovery-and-classification.md) 
- Meer informatie over [evaluatie van beveiligingsproblemen](sql-vulnerability-assessment.md) 
- Meer informatie over [detectie van bedreigingen](sql-database-threat-detection.md)
- Meer informatie over [Azure security center](https://docs.microsoft.com/azure/security-center/security-center-intro)
