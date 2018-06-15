---
title: Advanced Threat Protection - Azure SQL Database | Microsoft Docs
description: Meer informatie over de functionaliteit voor het detecteren en classificeren van gevoelige gegevens, het beheren van uw database beveiligingsproblemen en afwijkende activiteiten die kunnen duiden op een bedreiging voor uw Azure SQL database te detecteren.
services: sql-database
author: ronitr
manager: craigg
ms.service: sql-database
ms.topic: conceptual
ms.date: 5/17/2018
ms.author: ronitr
ms.reviewer: carlrab
ms.openlocfilehash: da21a0b66d86b4cc3e2dc59bdd972d4e24d7e5ec
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34305447"
---
# <a name="advanced-threat-protection-for-azure-sql-database"></a>Advanced Threat Protection voor Azure SQL-Database

SQL geavanceerde Threat Protection is een uniform pakket voor geavanceerde mogelijkheden voor SQL-beveiliging. Het bevat functionaliteit voor het detecteren en classificeren van gevoelige gegevens, het beheren van uw database beveiligingsproblemen en afwijkende activiteiten die kunnen duiden op een bedreiging voor uw database te detecteren. Biedt een locatie één Ga naar voor inschakelen en beheren van deze mogelijkheden. 

## <a name="overview"></a>Overzicht

SQL geavanceerde Threat Protection (ATP) biedt een set van geavanceerde SQL-beveiligingsmogelijkheden, met inbegrip van gegevens te zoeken en classificatie, controle op beveiligingslekken en detectie van dreigingen. 

- [Detectie van de gegevens & classificatie](sql-database-data-discovery-and-classification.md) (momenteel in preview) biedt ingebouwde mogelijkheden in Azure SQL Database voor het detecteren, classificeren, labelen en beveiligen van de gevoelige gegevens in uw databases. Worden kan gebruikt voor het leveren van zichtbaarheid van de status van de classificatie van uw database en de toegang tot gevoelige gegevens binnen de database en buiten de randen bijhouden.
- [Controle op beveiligingslekken](sql-vulnerability-assessment.md) is een eenvoudig te configureren van de service die u kunt detecteren, volgen en te herstellen database beveiligingslekken. Deze biedt inzicht in uw beveiligingsstatus en omvat bruikbare stappen voor het oplossen van beveiligingsproblemen en uw database fortifications te verbeteren.
- [Bedreigingendetectie](sql-database-threat-detection.md) detecteert afwijkende activiteiten die aangeeft ongebruikelijke en potentieel schadelijke probeert te openen of misbruik van uw database. Continu bewaakt uw database voor verdachte activiteiten en biedt onmiddellijke beveiligingswaarschuwingen op mogelijke beveiligingsproblemen, SQL-injectieaanvallen en database afwijkende toegangspatronen. Dagelijks geconstateerde waarschuwingen Geef details op van de verdachte activiteit en aanbevolen actie voor het onderzoeken en risico.

Schakel SQL ATP eenmaal zodat al deze functies opgenomen. Met één klik kunt u ATP inschakelen op de gehele database-server wordt toegepast op alle databases op de server. 

Prijzen voor ATP wordt uitgelijnd met Azure Security Center standaardcategorie $15/knooppunt/maand, waarbij elke beveiligde SQL Database-server als een knooppunt wordt geteld. De eerste 60 dagen na activering worden beschouwd als een gratis proefperiode en worden niet in rekening gebracht. Zie voor meer informatie de [pagina met prijzen van Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/).


## <a name="getting-started-with-atp"></a>Aan de slag met ATP 
De volgende stappen kunnen u met ATP. 

## <a name="1-enable-atp"></a>1. ATP inschakelen

ATP inschakelen door te navigeren naar **Advanced Threat Protection** onder de **beveiliging** in de kop van uw Azure SQL Database-deelvenster. Om in te schakelen ATP voor alle databases op de server, klikt u op **inschakelen Advanced Threat Protection op de server**.

![ATP inschakelen](./media/sql-advanced-protection/enable_atp.png) 

> [!NOTE]
> De kosten van ATP is knooppunt-$15/maand, wanneer een knooppunt wordt de hele logische SQL-server. U dus betaalt slechts één keer voor het beveiligen van alle databases op de server met ATP. De eerste 60 dagen worden beschouwd als een gratis proefversie.

## <a name="2-configure-vulnerability-assessment"></a>2. Controle op beveiligingslekken configureren

Om te evalueren beveiligingsproblemen gebruiken, moet u een opslagaccount waarin scanresultaten worden opgeslagen configureren. Klik op de controle op beveiligingslekken kaart om dit te doen.

![VA configureren](./media/sql-advanced-protection/configure_va.png) 

Selecteer of maak een opslagaccount voor het opslaan van de scanresultaten. U kunt ook periodieke terugkerende scans voor het configureren van de controle op beveiligingslekken om uit te voeren van automatische scans eenmaal per week inschakelen. Een scan resultaat samenvatting wordt verzonden naar de e-adressen die u opgeeft.

![VA-instellingen](./media/sql-advanced-protection/va_settings.png) 

## <a name="3-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>3. Gegevens classificeren, beveiligingsproblemen bijhouden en onderzoeken van waarschuwingen van de threat starten

Klik op de **gegevens te zoeken en classificatie** kaart om te zien aanbevolen gevoelige kolommen te classificeren en om uw gegevens met permanente gevoeligheidslabels te classificeren. Klik op de **controle op beveiligingslekken** kaart scans op zwakke plekken en rapporten weergeven en beheren en bijhouden van de status van de beveiliging. Als beveiligingswaarschuwingen zijn ontvangen, klikt u op de **detectie van dreigingen** kaart om weer te geven om details van de waarschuwingen en ziet u een geconsolideerde rapport bij alle waarschuwingen in uw Azure-abonnement via de pagina waarschuwingen van Azure Security Center-beveiliging.

## <a name="4-manage-atp-settings-on-your-sql-server"></a>4. ATP-instellingen op de SQL server beheren

Als u wilt weergeven en beheren van Advanced Threat Protection-instellingen, gaat u naar **Advanced Threat Protection** onder de **beveiliging** in de kop van uw SQL server-venster. Op deze pagina kunt u inschakelen of uitschakelen van ATP en detectie van dreigingen instellingen wijzigen voor de hele SQL-server.

![Serverinstellingen](./media/sql-advanced-protection/server_settings.png) 

## <a name="5-manage-atp-settings-for-a-sql-database"></a>5. ATP instellingen beheren voor een SQL-database

Voor detectie van dreigingen ATP-instellingen voor een bepaalde database overschrijven, Controleer de **inschakelen Advanced Threat Protection op databaseniveau** selectievakje. Gebruik deze optie alleen als u een vereiste voor het ontvangen van afzonderlijke dagelijks geconstateerde waarschuwingen voor de afzonderlijke-database in plaats van of naast de waarschuwingen voor alle databases op de server ontvangen. 

Nadat u het selectievakje is ingeschakeld, klikt u op **detectie van dreigingen instellingen voor deze database** en configureer vervolgens de relevante instellingen voor deze database.

![Database- en threat detectie-instellingen](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Geavanceerde Threat Protection-instellingen voor uw server kunnen ook worden bereikt vanaf het deelvenster ATP-database. Klik op **instellingen** in het hoofdvenster ATP en klik vervolgens op **weergave Advanced Threat Protection-serverinstellingen**. 

![Database-instellingen](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Volgende stappen 

- Meer informatie over [gegevens detectie & classificatie](sql-database-data-discovery-and-classification.md) 
- Meer informatie over [controle op beveiligingslekken](sql-vulnerability-assessment.md) 
- Meer informatie over [detectie van dreigingen](sql-database-threat-detection.md)
- Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
