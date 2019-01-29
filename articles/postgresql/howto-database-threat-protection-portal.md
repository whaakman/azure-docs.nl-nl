---
title: Advanced Threat Protection - Azure Database for PostgreSQL
description: Beveiliging tegen bedreigingen detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de database.
author: bolzmj
ms.author: mbolz
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: bbb48708d0e5a7cd07a3971a6966f40696107562
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55095092"
---
# <a name="advanced-threat-protection-for-azure-database-for-postgresql"></a>Advanced Threat Protection voor Azure Database for PostgreSQL

Advanced Threat Protection for Azure Database for PostgreSQL detecteert vreemde activiteiten die duiden op ongebruikelijke en mogelijk schadelijke pogingen om toegang te verkrijgen tot databases of om deze aan te vallen.

Advanced Threat Protection maakt deel uit van de aanbieding met geavanceerde beveiliging van gegevens, dit een geïntegreerde-pakket voor geavanceerde beveiligingsmogelijkheden is. Advanced Threat Protection kan worden geopend en beheerd de [Azure-portal](https://portal.azure.com) en is momenteel in preview.

> [!NOTE]
> De functie Advanced Threat Protection is **niet** beschikbaar in de volgende Azure government en onafhankelijke cloud-regio's: VS (overheid)-Texas, VS (overheid)-Arizona, VS (overheid)-Iowa, VS (overheid)-Virginia, US DoD-Oost, VS DoD-centraal, Duitsland-centraal, Noord Duitsland, China-Oost, China-Oost 2. Ga naar [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/) voor algemene beschikbaarheid.
>

> [!NOTE]
> Deze functie is beschikbaar in alle regio's van Azure waarop Azure Database for PostgreSQL voor algemeen gebruik en geoptimaliseerd voor geheugen-servers wordt geïmplementeerd.

## <a name="set-up-threat-detection"></a>Detectie van bedreigingen instellen
1. Starten van de Azure portal op [ https://portal.azure.com ](https://portal.azure.com).
2. Navigeer naar de configuratiepagina van de Azure Database for PostgreSQL-server die u wilt beveiligen. Selecteer in de beveiligingsinstellingen **Advanced Threat Protection (Preview)**.
3. Op de **Advanced Threat Protection (Preview)** configuratiepagina:

   - Advanced Threat Protection inschakelen op de server.
   - In **Advanced Threat Protection-instellingen**, in de **waarschuwingen verzenden naar** tekst geeft u de lijst met e-mailberichten ontvangen beveiligingswaarschuwingen tijdens de detectie van afwijkende activiteiten.
  
   ![Detectie van bedreigingen instellen](./media/howto-database-threat-protection-portal/set-up-threat-protection.png)

## <a name="explore-anomalous-database-activities"></a>Afwijkende databaseactiviteiten verkennen

U ontvangt een e-mailmelding bij detectie van afwijkende activiteiten. Het e-mailbericht bevat informatie over de verdachte beveiligingsgebeurtenis, inclusief de aard van de afwijkende activiteiten, databasenaam, servernaam, de naam van toepassing en de tijd van de gebeurtenis. Bovendien het e-mailbericht bevat informatie over mogelijke oorzaken en aanbevolen acties om te onderzoeken en oplossen van de mogelijke bedreiging met de database.
    
1. Klik op de **recente waarschuwingen weergeven** koppeling in de e-mail om te starten van de Azure-portal en weergeven van de Azure Security Center pagina met waarschuwingen, waarmee u een overzicht van actieve bedreigingen die zijn gedetecteerd op de SQL-database.
    
    ![Rapport van de afwijkende activiteit](./media/howto-database-threat-protection-portal/anomalous-activity-report.png)

    Weergave actieve bedreigingen:

    ![Actieve bedreigingen](./media/howto-database-threat-protection-portal/active-threats.png)

2. Klik op een specifieke waarschuwing om aanvullende gegevens en acties voor deze bedreiging te onderzoeken en oplossen van problemen met toekomstige bedreigingen.
    
    ![Specifieke waarschuwing](./media/howto-database-threat-protection-portal/specific-alert.png)

## <a name="explore-threat-detection-alerts"></a>Meldingen voor geconstateerde bedreigingen verkennen

Advanced Threat Protection integreert de waarschuwingen met [Azure Security Center](https://azure.microsoft.com/services/security-center/). 

Klik op **beveiligingswaarschuwingen** onder **THREAT PROTECTION** naar de Azure Security Center start waarschuwingen pagina en krijg een overzicht van actieve SQL-bedreigingen gedetecteerd op de database.

  ![Threat protection asc](./media/howto-database-threat-protection-portal/threat-detection-alert-asc.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Zie voor meer informatie over prijzen voor de [Azure Database for PostgreSQL-prijzen-pagina](https://azure.microsoft.com/pricing/details/postgresql/)  
