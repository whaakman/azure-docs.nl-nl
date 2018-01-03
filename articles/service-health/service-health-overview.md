---
title: Overzicht van Azure servicestatus | Microsoft Docs
description: "Persoonlijke informatie over hoe uw apps van Azure worden beïnvloed door de huidige en toekomstige Azure serviceproblemen en onderhoud."
services: Resource health
documentationcenter: 
author: rboucher
manager: 
editor: 
ms.assetid: 
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/07/2017
ms.author: robb
ms.openlocfilehash: c463479b7eaee5a0548c8891dd3a20ef070dd39b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="azure-service-health"></a>Azure Service Health
Status van de Azure-Service biedt tijdige en persoonlijke informatie wanneer problemen in de Azure-services invloed zijn op uw services.  Hiermee kunt u ook voorbereiden op toekomstige gepland onderhoud.

## <a name="service-health-events"></a>Gebeurtenissen van de Health service
De Health-service houdt drie soorten health-gebeurtenissen die mogelijk van invloed op uw resources:
1. **Problemen met service** -problemen in de Azure-services die u nu direct beïnvloeden. 
2. **Gepland onderhoud** -aanstaande onderhoud die de beschikbaarheid van uw services in de toekomst kan beïnvloeden.  
3. **Health aanbevelingen** -wijzigingen in de Azure-services die uw aandacht vereisen. Voorbeelden zijn wanneer het Azure-functies zijn afgeschaft of als u een gebruiksquotum overschrijdt.

    ![Service Health-gebeurtenissen](./media/service-health-overview/azure-service-health-overview-7.png)

## <a name="get-started-with-service-health"></a>Aan de slag met de Health-Service
Start uw dashboard voor servicestatus, selecteer de tegel status van de Service op uw portal-dashboard. Als u de tegel eerder hebt verwijderd, of u aangepaste dashboard, zoekt u naar de Service Health-service in 'Meer services' (rechtsonder op uw dashboard).
![Aan de slag met de Health-Service](./media/service-health-overview/azure-service-health-overview-1.png)

## <a name="see-current-issues-which-impact-your-services"></a>Zie actuele problemen die van invloed zijn uw services
De **Service problemen** weergave bevat actieve problemen in Azure-services die van invloed zijn op de uw resources. U kunt begrijpen wanneer het probleem is begonnen, en welke services en regio's worden beïnvloed. U kunt ook de meest recente update om te begrijpen wat Azure doet het probleem op te lossen lezen. 
![Serviceprobleem beheren](./media/service-health-overview/azure-service-health-overview-2.png)

Kies de **potentiële impact** tabblad voor een overzicht van de specifieke lijst met resources die u bezit en die kunnen worden beïnvloed door het probleem. U kunt een CSV-lijst van deze resources te delen met uw team kunt downloaden.
![Serviceprobleem - Impact beheren](./media/service-health-overview/azure-service-health-overview-4.png)

## <a name="get-links-and-downloadable-explanations"></a>Koppelingen en downloadbare uitleg ophalen 
U kunt een koppeling voor het probleem moet worden gebruikt in uw probleem beheersysteem ophalen. U kunt downloaden PDF- en soms CSV-bestanden te delen met mensen die u geen toegang tot de Azure-portal hebt.   
![Beheren van serviceprobleem - probleembeheer](./media/service-health-overview/azure-service-health-overview-3.png)

## <a name="get-support-from-microsoft"></a>Ondersteuning krijgen van Microsoft
Neem contact op met ondersteuning als de bron in een verkeerde status blijft zelfs nadat het probleem opgelost is.  Gebruik de koppelingen ondersteuning aan de rechterkant van de pagina.  

## <a name="pin-a-personalized-health-map-to-your-dashboard"></a>Een persoonlijke health kaart aan uw dashboard vastmaken
Servicestatus om weer te geven van uw bedrijfskritieke abonnementen, regio's en brontypen filteren. Het filter hebt opgeslagen en een persoonlijke health world kaart aan uw portal-dashboard vastmaken. 
![Filter gepersonaliseerde health kaart](./media/service-health-overview/azure-service-health-overview-6a.png)
![vastmaken een gepersonaliseerde health-kaart](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Servicestatus waarschuwingen configureren
Status van de Azure-Service kan worden geïntegreerd met Azure-bewaking om u te waarschuwen via e-mailberichten, tekstberichten en webhook meldingen wanneer uw bedrijfskritieke resources ondervinden gevolgen. Instellen van een activiteit logboek-waarschuwing voor de gebeurtenis status van de Service. Deze waarschuwing doorsturen naar de juiste mensen in uw organisatie met behulp van groepen in te grijpen. Zie voor meer informatie [waarschuwingen configureren voor de Health-Service](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md)

 
