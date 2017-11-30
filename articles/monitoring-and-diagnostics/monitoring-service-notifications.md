---
title: Wat zijn Servicestatusmeldingen | Microsoft Docs
description: Servicestatusmeldingen kunnen u bekijken servicestatus berichten publiceren door Microsoft Azure.
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: efdd42d244710b27fc33154b708cfbe40312e3b0
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="service-health-notifications"></a>Servicestatusmeldingen
## <a name="overview"></a>Overzicht

Dit artikel laat zien hoe u servicestatusmeldingen met de Azure portal weergeeft.

Servicestatusmeldingen kunnen u berichten van health-service gepubliceerd door het team van Azure die invloed kan zijn op van de resources in uw abonnement weergeven. Deze meldingen zijn een subklasse zijn van de activiteit logboekgebeurtenissen en kunnen worden gevonden in het gebeurtenissenlogboek. Servicestatusmeldingen zijn informatief of actie worden uitgevoerd, afhankelijk van de klasse.

Er zijn vijf soorten servicestatusmeldingen:  

- **De actie is vereist:** van tijd tot tijd Azure is het mogelijk iets vreemds optreden op uw account. Azure wellicht samen met u lost dit probleem. Azure stuurt u een melding ofwel met gedetailleerde informatie over de acties die u moet uitvoeren of met meer informatie over het contact opnemen met Azure engineering of ondersteuning.  
- **Assisted Recovery:** een gebeurtenis heeft plaatsgevonden en engineers hebt bevestigd dat u nog steeds gevolgen ondervindt. Azure-engineering moet samenwerken met u rechtstreeks uw services naar volledige status te herstellen.  
- **Incident:** een service die van invloed op de gebeurtenis is momenteel die betrekking hebben op een of meer van de resources in uw abonnement.  
- **Onderhoud:** dit is een melding van een gepland onderhoud-activiteit die mogelijk van invloed op een of meer van de resources in uw abonnement.  
- **Gegevens:** van tijd tot tijd Azure u meldingen die u informeren over potentiële optimalisaties die kunnen helpen uw Resourcegebruik verbeteren kan verzenden.  
- **Beveiliging:** urgente beveiliging gerelateerde informatie met betrekking tot uw oplossing(en) uitgevoerd op Azure.

Elke melding van de health service bevat de details van het bereik en de gevolgen voor uw resources. Details opnemen:

De naam van eigenschap | Beschrijving
-------- | -----------
kanalen | Is een van de volgende waarden: 'Admin', 'Operation'
correlationId | Is meestal een GUID in de indeling van de tekenreeks. Gebeurtenissen met die deel uitmaken van dezelfde uber actie meestal delen de dezelfde correlationId.
eventDataId | Is de unieke id van een gebeurtenis
EventName | De titel van de gebeurtenis is
niveau | Niveau van de gebeurtenis. Een van de volgende waarden: 'Kritiek', 'Fout', 'Waarschuwing', 'Ter informatie' en 'Verbose'
resourceProviderName | Naam van de resourceprovider voor de betrokken resource
resourceType| Het type resource van de betrokken resource
subStatus | Meestal de HTTP-statuscode van de bijbehorende REST te bellen, maar kan ook andere tekenreeksen met een beschrijving van een substatus, zoals deze algemene waarden bevatten: OK (HTTP-statuscode: 200), gemaakt (HTTP-statuscode: 201), geaccepteerde (HTTP-statuscode: 202), geen inhoud (http-Status Code: 204), onjuiste aanvraag (HTTP-statuscode: 400), niet is gevonden (HTTP-statuscode: 404), Conflict (HTTP-statuscode: 409), interne serverfout (HTTP-statuscode: 500), Service niet beschikbaar (HTTP-statuscode: 503), time-out van Gateway (HTTP-statuscode: 504).
eventTimestamp | Tijdstempel wanneer de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag de gebeurtenis overeenkomt.
submissionTimestamp |   Tijdstempel wanneer de gebeurtenis beschikbaar voor het uitvoeren van query's zijn geworden.
subscriptionId | Het Azure-abonnement in waarmee deze gebeurtenis is vastgelegd
status | De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: In voortgang, geslaagd, mislukt, actief, opgelost gestart.
operationName | De naam van de bewerking.
category | 'ServiceHealth'
resourceId | Bron-ID van de betrokken resource.
Properties.title | De gelokaliseerde titel voor deze communicatie. Engels is de standaardtaal.
Properties.Communication | De gelokaliseerde details van de communicatie met de HTML-opmaak. Engels is de standaardinstelling.
Properties.incidentType | Mogelijke waarden: AssistedRecovery, ActionRequired, informatie, Incident-, onderhoud, beveiliging
Properties.trackingId | Identificeert het incident dat deze gebeurtenis is gekoppeld. Gebruik deze optie als u de gebeurtenissen die betrekking hebben op een incident met elkaar correleren.
Properties.impactedServices | Een escape-teken JSON-blob die hierin worden de services en regio's die worden beïnvloed door het incident. Een lijst met Services, die allemaal een servicenaam en een lijst met ImpactedRegions, die allemaal een RegionName.
Properties.defaultLanguageTitle | De communicatie in het Engels
Properties.defaultLanguageContent | De communicatie in het Engels als HTML-indeling of tekst zonder opmaak
Properties.Stage | Mogelijke waarden voor AssistedRecovery, ActionRequired, informatie, Incident-, beveiliging: actief, zijn opgelost. Ze zijn voor onderhoud: actief, gepland, InProgress, geannuleerd, Rescheduled, opgelost, voltooid
Properties.communicationId | De communicatie deze gebeurtenis is gekoppeld.


## <a name="viewing-your-service-health-notifications-in-the-azure-portal"></a>Uw servicestatusmeldingen weergeven in de Azure portal
1.  In de [portal](https://portal.azure.com), gaat u naar de **Monitor** service

    ![Bewaken](./media/monitoring-service-notifications/home-monitor.png)
2.  Klik op de **Monitor** optie om de ervaring van de Monitor te openen. Monitor voor Azure samenbrengt uw controle-instellingen en gegevens in één geconsolideerde weergave. Als eerste wordt de sectie **Activiteitenlogboek** geopend.

3.  Klik nu op **waarschuwingen** sectie

    ![Bewaken](./media/monitoring-service-notifications/service-health-summary.png)
4. Klik op de **+ activiteit logboek waarschuwing toevoegen** en een waarschuwing, zodat u wordt gewaarschuwd voor toekomstige servicemeldingen configureren. Voor meer informatie over het configureren van waarschuwingen op servicemeldingen [Ga naar de pagina activiteit logboek waarschuwingen en meldingen van de Service](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="next-steps"></a>Volgende stappen
Ontvangen [meldingen wanneer een melding van de health service](monitoring-activity-log-alerts-on-service-notifications.md) wordt gepost  
Meer informatie over [activiteit logboek waarschuwingen](monitoring-activity-log-alerts.md)
