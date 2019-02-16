---
title: Status van de service-overzicht | Microsoft Docs
description: Persoonlijke informatie over hoe uw Azure-apps worden beïnvloed door problemen met de huidige en toekomstige Azure-service en onderhoud.
services: service-health
author: stephbaron
ms.author: stbaron
documentationcenter: service-health
ms.service: service-health
ms.topic: article
ms.workload: Supportability
ms.date: 03/27/2018
ms.openlocfilehash: 465e8751d02692648234a7a90b84b68f41522cb2
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56328262"
---
# <a name="service-health"></a>Service Health
Status van de service biedt u een aanpasbaar dashboard houdt de status van uw Azure-services in de regio's waar u ze gebruiken. In dit dashboard, kunt u actieve gebeurtenissen, zoals problemen van de serviceonderbreking gepland onderhoud of relevante statusadvies bijhouden. Wanneer gebeurtenissen niet-actief worden, krijgen ze in de geschiedenis van uw geplaatst voor maximaal 90 dagen. Ten slotte kunt u de Service Health-dashboard maken en beheren van waarschuwingen van de health service die u proactief te waarschuwen wanneer er problemen met de service van invloed zijn op u.

## <a name="service-health-events"></a>Service Health-gebeurtenissen
Status van de service houdt drie typen van de health-gebeurtenissen die mogelijk van invloed op uw resources:
1. **Problemen met service** -problemen in de Azure-services die u nu direct invloed hebben op. 
2. **Gepland onderhoud** -toekomstig onderhoud die de beschikbaarheid van uw services in de toekomst kan beïnvloeden.  
3. **Statusadvies** -wijzigingen in de Azure-services die uw aandacht nodig hebben. Voorbeelden zijn wanneer de Azure-functies zijn afgeschaft of als u een quotum voor gebruik.

> [!NOTE]
> Als u Service Health-gebeurtenissen, moeten gebruikers de rol van lezer voor een abonnement hebben.

## <a name="get-started-with-service-health"></a>Aan de slag met Service Health
Selecteer de tegel status van de Service op uw portaldashboard voor het starten van uw Service Health-dashboard. Als u de tegel eerder hebt verwijderd of u aangepaste dashboard, zoekt u Service Health-service in 'Meer services' (rechtsonder op uw dashboard).

![Aan de slag met Service Health](./media/service-health-overview/azure-service-health-overview-1.png)

## <a name="see-current-issues-which-impact-your-services"></a>Zie de huidige problemen die invloed hebben op uw services
De **Service problemen** weergave toont de lopende problemen in Azure-services die van invloed zijn op de resources. U kunt begrijpen wanneer het probleem is begonnen en op welke services en regio's worden beïnvloed. U kunt ook de meest recente update voor meer informatie over wat Azure doet het probleem op te lossen lezen. 

![Serviceprobleem beheren](./media/service-health-overview/azure-service-health-overview-2.png)

Kies de **mogelijke invloed op** tabblad om te bekijken van de specifieke lijst met resources van jou die kunnen worden beïnvloed door het probleem. U kunt een CSV-lijst van deze resources te delen met uw team kunt downloaden.

![Serviceprobleem - Impact beheren](./media/service-health-overview/azure-service-health-overview-4.png)

## <a name="get-links-and-downloadable-explanations"></a>Get-koppelingen en downloadbare uitleg 
U kunt een koppeling voor het probleem te gebruiken in uw probleembeheersysteem ophalen. U kunt downloaden PDF- en soms CSV-bestanden te delen met mensen die geen toegang tot de Azure-portal hebt.   

![Beheren van serviceprobleem - probleembeheer](./media/service-health-overview/azure-service-health-overview-3.png)

## <a name="get-support-from-microsoft"></a>Ondersteuning krijgen van Microsoft
Neem contact op met ondersteuning als uw resource in een slechte status blijft, zelfs nadat het probleem opgelost is.  Gebruik de koppelingen aan de rechterkant van de pagina.  

## <a name="pin-a-personalized-health-map-to-your-dashboard"></a>Een aangepaste status toewijzen aan uw dashboard vastmaken
Service Health om weer te geven uw bedrijfskritische abonnementen, regio's en resourcetypen filteren. Het filter hebt opgeslagen en een persoonlijke health wereldkaart aan uw portal-dashboard vastmaken. 

![Filter de gezondheid van aangepaste kaart](./media/service-health-overview/azure-service-health-overview-6a.png)

![Pincode een persoonlijke health-kaart](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Service health waarschuwingen configureren
Status van de service kan worden geïntegreerd met Azure Monitor om u te waarschuwen via e-mails, SMS-berichten en webhook-meldingen wanneer er gevolgen zijn voor uw bedrijfskritische resources. Instellen van een waarschuwing voor activiteitenlogboek voor de betreffende service health-gebeurtenis. Deze waarschuwing gerouteerd naar de juiste mensen in uw organisatie met behulp van Actiegroepen. Zie voor meer informatie, [waarschuwingen configureren voor servicestatus](../azure-monitor/platform/alerts-activity-log-service-notifications.md)

# <a name="next-steps"></a>Volgende stappen
Stel waarschuwingen in zodat u een melding van statusproblemen ontvangt. Zie voor meer informatie, [waarschuwingen configureren voor Service Health](../azure-monitor/platform/alerts-activity-log-service-notifications.md). 
