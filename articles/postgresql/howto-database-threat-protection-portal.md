---
title: Advanced Threat Protection - Azure Database for PostgreSQL
description: Beveiliging tegen bedreigingen detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de database.
author: bolzmj
ms.author: mbolz
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/20/2018
ms.openlocfilehash: b7c1f873d47be0f2ec0125f769a47a399e492662
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53536148"
---
# <a name="advanced-threat-protection-for-azure-database-for-postgresql"></a>Advanced Threat Protection voor Azure Database for PostgreSQL

Advanced Threat Protection for Azure Database for PostgreSQL detecteert vreemde activiteiten die duiden op ongebruikelijke en mogelijk schadelijke pogingen om toegang te verkrijgen tot databases of om deze aan te vallen.

Advanced Threat Protection maakt deel uit van de aanbieding met geavanceerde beveiliging van gegevens, dit een geïntegreerde-pakket voor geavanceerde beveiligingsmogelijkheden is. Advanced Threat Protection kan worden geopend en beheerd de [Azure-portal](https://portal.azure.com) en is momenteel in preview.

> [!NOTE]
> De functie Advanced Threat Protection is **niet** beschikbaar in de volgende Azure government en onafhankelijke cloud-regio's: VS (overheid)-Texas, VS (overheid)-Arizona, VS (overheid)-Iowa, VS (overheid)-Virginia, US DoD-Oost, VS DoD-centraal, Duitsland-centraal, Noord Duitsland, China-Oost, China-Oost 2. Ga naar [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/) voor algemene beschikbaarheid.
>

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

  ![Threat protectoin asc](./media/howto-database-threat-protection-portal/threat-detection-alert-asc.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Zie voor meer informatie over prijzen voor de [Azure Database for PostgreSQL-prijzen-pagina](https://azure.microsoft.com/pricing/details/postgresql/)  
