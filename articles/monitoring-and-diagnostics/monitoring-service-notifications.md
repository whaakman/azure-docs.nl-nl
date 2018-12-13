---
title: Wat zijn meldingen voor Azure service health?
description: Servicestatusmeldingen kunnen u berichten van health-service gepubliceerd door Microsoft Azure weergeven.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 4/12/2017
ms.author: dukek
ms.component: activitylog
ms.openlocfilehash: fbe5e6a34332326d5169497a016a03ef629dd00c
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269019"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Servicestatusmeldingen bekijken met behulp van Azure portal

Servicestatusmeldingen zijn gepubliceerd door Azure, en bevatten informatie over de resources in uw abonnement. Deze meldingen zijn een subklasse van de activiteit van gebeurtenissen, en kunnen u ook vinden in het activiteitenlogboek. Servicestatusmeldingen zijn informatieve en bruikbare, afhankelijk van de klasse.

Er zijn verschillende soorten servicestatusmeldingen:  

- **Actie vereist:** Azure mogelijk ziet u iets vreemds gebeurt op uw account en samen met u lost dit probleem. Azure stuurt u een melding, ofwel met gedetailleerde informatie over de acties die u moet ondernemen of hoe u contact op met Azure engineering of ondersteuning.  
- **Geboden na herstel:** Een gebeurtenis heeft plaatsgevonden en technici hebt bevestigd dat u zich nog steeds effecten voordoet. Azure engineering moet samen met u rechtstreeks naar het herstellen van uw services naar volledige status.  
- **Incident:** Een gebeurtenis die van invloed is op service is momenteel die betrekking hebben op een of meer van de resources in uw abonnement.  
- **Onderhoud:** Een geplande onderhoudsactiviteiten die mogelijk invloed op een of meer van de resources in uw abonnement.  
- **Informatie over:** Mogelijke optimalisaties die kunnen helpen bij het verbeteren van uw resource gebruiken. 
- **Beveiliging:** Dringende beveiligingsgerelateerde informatie met betrekking tot uw toepassingen die worden uitgevoerd op Azure.

Elke melding van de health service bevat informatie over de omvang en impact op uw resources. Details omvatten:

Naam van eigenschap | Description
-------- | -----------
kanalen | Een van de volgende waarden: **Beheerder** of **bewerking**.
correlationId | Meestal een GUID in de indeling van de verbindingsreeks. Gebeurtenissen die deel uitmaken van dezelfde actie meestal delen de dezelfde correlationId.
eventDataId | De unieke id van een gebeurtenis.
eventName | De titel van een gebeurtenis.
niveau | Het niveau van een gebeurtenis
resourceProviderName | De naam van de resourceprovider voor de betrokken resource.
ResourceType| Het type resource van de betrokken resource.
subStatus | Meestal de HTTP-statuscode van de bijbehorende REST aanroepen, maar kan ook andere tekenreeksen met een beschrijving van een substatus bevatten. Bijvoorbeeld: OK (HTTP-statuscode: 200), die zijn gemaakt (HTTP-statuscode: 201), geaccepteerd (HTTP-statuscode: 202), geen inhoud (HTTP-statuscode: 204), onjuiste aanvraag (HTTP-statuscode: 400), niet gevonden (HTTP-statuscode: 404), conflict (HTTP-statuscode: 409), een interne serverfout (HTTP-statuscode: 500), Service is niet beschikbaar (HTTP-statuscode: 503), en de time-out van Gateway (HTTP-statuscode: 504).
eventTimestamp | Tijdstip waarop de gebeurtenis is gegenereerd door de Azure-service verwerken van de aanvraag overeenkomt met de gebeurtenis.
submissionTimestamp | Tijdstip waarop de gebeurtenis is beschikbaar voor het uitvoeren van query's geworden.
subscriptionId | De Azure-abonnement waarin deze gebeurtenis is vastgelegd.
status | De tekenreeks met een beschrijving van de status van de bewerking. Sommige algemene waarden zijn: **Aan de slag**, **Bezig**, **geslaagd**, **is mislukt**, **Active**, en **opgelost**.
operationName | De naam van de bewerking.
category | Deze eigenschap is altijd **ServiceHealth**.
resourceId | De Resource-ID van de betrokken resource.
Properties.title | De gelokaliseerde naam voor deze communicatie. Engels is de standaardinstelling.
Properties.Communication | Gelokaliseerde informatie van de communicatie met de HTML-opmaak. Engels is de standaardinstelling.
Properties.incidentType | Een van de volgende waarden: **Handeling vereist**, **informatie**, **Incident**, **onderhoud**, of **Security**.
Properties.trackingId | Het incident waarmee deze gebeurtenis gekoppeld is. Gebruik deze optie als u correlaties zichtbaar maken tussen de gebeurtenissen die betrekking hebben op een incident.
Properties.impactedServices | Een met escape-teken JSON-blob die wordt beschreven van de services en regio's die worden beïnvloed door het incident. De eigenschap bevat een lijst met services, die allemaal een **ServiceName**, en een lijst van betrokken regio's, die allemaal een **RegionName**.
Properties.defaultLanguageTitle | De communicatie in het Engels.
Properties.defaultLanguageContent | De communicatie in het Engels als HTML-indeling of tekst zonder opmaak.
Properties.Stage | De mogelijke waarden voor **Incident**, en **Security** zijn **actief is,** **opgelost** of **RCA**. Voor **handeling vereist** of **informatie** is de enige waarde **actief.** Voor **onderhoud** ze zijn: **Actieve**, **geplande**, **InProgress**, **geannuleerd**, **opnieuw gepland**, **opgelost**, of **volledige**.
Properties.communicationId | De communicatie die aan deze gebeurtenis gekoppeld is.

### <a name="details-on-service-health-level-information"></a>Meer informatie over service health informatie op het niveau
  <ul>
    <li><b>Actie vereist</b> (properties.incidentType == handeling vereist) <dl>
            <dt>Informatief</dt>
            <dd>Beheerdersactie vereist om te voorkomen dat invloed op bestaande services</dd>
        </dl>
    </li>
    <li><b>Onderhoud</b> (properties.incidentType onderhoud ==) <dl>
            <dt>Waarschuwing</dt>
            <dd>noodgevallen onderhoud<dd>
            <dt>Informatief</dt>
            <dd>Standard gepland onderhoud</dd>
        </dl>
    </li>
    <li><b>Informatie</b> (properties.incidentType == informatie) <dl>
            <dt>Informatief</dt>
            <dd>De beheerder mogelijk moet om de gevolgen voor bestaande services</dd>
        </dl>
    </li>
    <li><b>Beveiliging</b> (properties.incidentType == beveiliging) <dl>
            <dt>Fout</dt>
            <dd>Wijdverbreid problemen met toegang tot meerdere services in meerdere regio's van invloed zijn op een breed scala aan klanten.</dd>
            <dt>Waarschuwing</dt>
            <dd>Problemen met toegang tot specifieke services en/of specifieke regio's van invloed zijn op een subset van klanten.</dd>
            <dt>Informatief</dt>
            <dd>Problemen die invloed hebben op bewerkingen en/of latentie, niet van invloed op beschikbaarheid van de service.</dd>
        </dl>
    </li>
    <li><b>Problemen met service</b> (properties.incidentType Incident ==) <dl>
            <dt>Fout</dt>
            <dd>Wijdverbreid problemen met toegang tot meerdere services in meerdere regio's van invloed zijn op een breed scala aan klanten.</dd>
            <dt>Waarschuwing</dt>
            <dd>Problemen met toegang tot specifieke services en/of specifieke regio's van invloed zijn op een subset van klanten.</dd>
            <dt>Informatief</dt>
            <dd>Problemen die invloed hebben op bewerkingen en/of latentie, niet van invloed op beschikbaarheid van de service.</dd>
        </dl>
    </li>
  </ul>

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Uw servicestatusmeldingen bekijken in Azure portal
1.  In de [Azure-portal](https://portal.azure.com), selecteer **Monitor**.

    ![Schermafbeelding van de Azure portal in het menu met Monitor geselecteerd](./media/monitoring-service-notifications/home-monitor.png)

    Azure Monitor combineert alle controle-instellingen en gegevens in één geconsolideerde weergave. Als eerste wordt de sectie **Activiteitenlogboek** geopend.

3.  Selecteer **waarschuwingen**.

    ![Schermafbeelding van de Monitor Activity-log, met waarschuwingen die zijn geselecteerd](./media/monitoring-service-notifications/service-health-summary.png)
4. Selecteer **+ waarschuwing voor activiteitenlogboek toevoegen**, en een waarschuwing instellen om te controleren of u ontvangt een melding voor toekomstige servicemeldingen. Zie voor meer informatie, [waarschuwingen voor activiteitenlogboek maken voor servicemeldingen](../azure-monitor/platform/alerts-activity-log-service-notifications.md).

## <a name="next-steps"></a>Volgende stappen
Ontvangen [meldingen wanneer er een melding van de health service](../azure-monitor/platform/alerts-activity-log-service-notifications.md) wordt geplaatst.  
Meer informatie over [waarschuwingen voor activiteitenlogboeken](../azure-monitor/platform/activity-log-alerts.md).
