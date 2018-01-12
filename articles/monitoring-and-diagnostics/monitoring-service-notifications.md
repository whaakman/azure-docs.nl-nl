---
title: Wat zijn Azure servicestatusmeldingen? | Microsoft Docs
description: Servicestatusmeldingen zodat u kunt bekijken van berichten van health-service gepubliceerd door Microsoft Azure.
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
ms.openlocfilehash: 4a95e9882515e6a2861292829a44847e11f39063
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Servicestatusmeldingen weergeven met behulp van de Azure-portal

Servicestatusmeldingen door Azure worden gepubliceerd, en bevatten informatie over de resources in uw abonnement. Deze meldingen zijn een subklasse zijn van de activiteit logboekgebeurtenissen en kunnen worden gevonden in het gebeurtenissenlogboek. Servicestatusmeldingen zijn informatief of actie worden uitgevoerd, afhankelijk van de klasse.

Er zijn verschillende soorten servicestatusmeldingen:  

- **Actie vereist:** wellicht opgevallen dat Azure ongewone activiteit optreden op uw account en samen met u lost dit probleem. Azure stuurt u een melding, hetzij met gedetailleerde informatie over de acties die u moet uitvoeren of contact opnemen met de Azure-engineering of ondersteuning.  
- **Ondersteunde herstel:** een gebeurtenis heeft plaatsgevonden en engineers hebt bevestigd dat u nog steeds gevolgen ondervindt. Azure-engineering moet samenwerken met u rechtstreeks uw services naar volledige status te herstellen.  
- **Incident:** een gebeurtenis stuit die van invloed is op service momenteel invloed op een of meer van de resources in uw abonnement.  
- **Onderhoud:** een gepland onderhoud-activiteit die dit mogelijk invloed op een of meer van de resources in uw abonnement.  
- **Gegevens:** mogelijke optimalisaties die kunnen helpen verbeteren uw gebruik van bronnen. 
- **Beveiliging:** urgente veiligheidsgerelateerde informatie met betrekking tot uw oplossingen die worden uitgevoerd op Azure.

Elke melding van de health service bevat de details van het bereik en de gevolgen voor uw resources. Details opnemen:

Naam van eigenschap | Beschrijving
-------- | -----------
kanalen | Een van de volgende waarden: **Admin** of **bewerking**.
correlationId | Meestal een GUID in de indeling van de tekenreeks. Gebeurtenissen die deel uitmaken van dezelfde actie meestal delen de dezelfde correlationId.
eventDataId | De unieke id van een gebeurtenis.
EventName | De titel van een gebeurtenis.
niveau | Het niveau van een gebeurtenis. Een van de volgende waarden: **Kritiek**, **fout**, **waarschuwing** of **informatief**.
resourceProviderName | De naam van de resourceprovider voor de betrokken resource.
resourceType| Het type resource van de betrokken resource.
subStatus | Meestal de HTTP-statuscode van de bijbehorende REST aanroepen, maar kan ook andere tekenreeksen met een beschrijving van een substatus bevatten. Bijvoorbeeld: OK (HTTP-statuscode: 200), gemaakt (HTTP-statuscode: 201), geaccepteerde (HTTP-statuscode: 202), geen inhoud (HTTP-statuscode: 204), ongeldige aanvraag (HTTP-statuscode: 400), niet vinden (HTTP-statuscode: 404), Conflict (HTTP-statuscode: 409), interne Server Fout (HTTP-statuscode: 500), Service niet beschikbaar (HTTP-statuscode: 503), en de time-out van Gateway (HTTP-statuscode: 504).
eventTimestamp | Tijdstempel wanneer de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag overeenkomt met de gebeurtenis.
submissionTimestamp | Tijdstempel wanneer de gebeurtenis beschikbaar voor het uitvoeren van query's zijn geworden.
subscriptionId | De Azure-abonnement in waarmee deze gebeurtenis is vastgelegd.
status | De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: **gestart**, **In uitvoering**, **geslaagd**, **mislukt**, **Active**, en **Opgelost**.
operationName | De naam van de bewerking.
category | Deze eigenschap is altijd **ServiceHealth**.
resourceId | De Resource-ID van de betrokken resource.
Properties.title | De gelokaliseerde titel voor deze communicatie. Engels is de standaardinstelling.
Properties.Communication | De gelokaliseerde details van de communicatie met de HTML-opmaak. Engels is de standaardinstelling.
Properties.incidentType | Een van de volgende waarden: **AssistedRecovery**, **ActionRequired**, **informatie**, **Incident**,  **Onderhoud**, of **beveiliging**.
Properties.trackingId | Het incident die aan deze gebeurtenis gekoppeld is. Gebruik deze optie als u de gebeurtenissen die betrekking hebben op een incident met elkaar correleren.
Properties.impactedServices | Een escape-teken JSON-blob die hierin worden de services en regio's die worden beïnvloed door het incident. De eigenschap bevat een lijst met services, elk met een **ServiceName**, en een lijst van de betrokken regio's, elk met een **RegionName**.
Properties.defaultLanguageTitle | De communicatie in het Engels.
Properties.defaultLanguageContent | De communicatie in het Engels als HTML-indeling of tekst zonder opmaak.
Properties.Stage | De mogelijke waarden voor **AssistedRecovery**, **ActionRequired**, **informatie**, **Incident**, en **beveiliging**  zijn **Active** of **opgelost**. Voor **onderhoud**, ze zijn: **Active**, **geplande**, **InProgress**, **geannuleerd**, **Opnieuw gepland**, **opgelost**, of **voltooid**.
Properties.communicationId | De communicatie die aan deze gebeurtenis gekoppeld is.


## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Uw servicestatusmeldingen weergeven in de Azure portal
1.  In de [Azure-portal](https://portal.azure.com), selecteer **Monitor**.

    ![Schermopname van Azure-portal-menu, met Monitor geselecteerd](./media/monitoring-service-notifications/home-monitor.png)

    Monitor voor Azure samenbrengt uw controle-instellingen en gegevens in één geconsolideerde weergave. Als eerste wordt de sectie **Activiteitenlogboek** geopend.

3.  Selecteer **waarschuwingen**.

    ![Schermafbeelding van de activiteit van de Monitor-logboek met waarschuwingen die zijn geselecteerd](./media/monitoring-service-notifications/service-health-summary.png)
4. Selecteer **+ toevoegen activiteit logboek waarschuwing**, en een waarschuwing instellen om te controleren of u wordt gewaarschuwd voor toekomstige servicemeldingen. Zie voor meer informatie [activiteit logboek waarschuwingen maken op servicemeldingen](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="next-steps"></a>Volgende stappen
Ontvangen [meldingen wanneer een melding van de health service](monitoring-activity-log-alerts-on-service-notifications.md) is geplaatst.  
Meer informatie over [activiteit logboek waarschuwingen](monitoring-activity-log-alerts.md).
