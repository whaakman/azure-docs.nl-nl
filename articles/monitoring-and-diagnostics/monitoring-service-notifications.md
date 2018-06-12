---
title: Wat zijn Azure servicestatusmeldingen?
description: Servicestatusmeldingen zodat u kunt bekijken van berichten van health-service gepubliceerd door Microsoft Azure.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 4/12/2017
ms.author: dukek
ms.component: activitylog
ms.openlocfilehash: d6a87b17041c4ce6cf41da863354ef5a2a37141c
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264439"
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
eventName | De titel van een gebeurtenis.
niveau | Het niveau van een gebeurtenis
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
Properties.incidentType | Een van de volgende waarden: **ActionRequired**, **informatie**, **Incident**, **onderhoud**, of **beveiliging**.
Properties.trackingId | Het incident die aan deze gebeurtenis gekoppeld is. Gebruik deze optie als u de gebeurtenissen die betrekking hebben op een incident met elkaar correleren.
Properties.impactedServices | Een escape-teken JSON-blob die hierin worden de services en regio's die worden beïnvloed door het incident. De eigenschap bevat een lijst met services, elk met een **ServiceName**, en een lijst van de betrokken regio's, elk met een **RegionName**.
Properties.defaultLanguageTitle | De communicatie in het Engels.
Properties.defaultLanguageContent | De communicatie in het Engels als HTML-indeling of tekst zonder opmaak.
Properties.Stage | De mogelijke waarden voor **Incident**, en **beveiliging** zijn **actief,** **opgelost** of **RCA**. Voor **ActionRequired** of **informatie** is de enige waarde **actief.** Voor **onderhoud** ze: **Active**, **geplande**, **InProgress**, **geannuleerd**, **Opnieuw gepland**, **opgelost**, of **voltooid**.
Properties.communicationId | De communicatie die aan deze gebeurtenis gekoppeld is.

### <a name="details-on-service-health-level-information"></a>Meer informatie over health informatie over serviceniveau
  <ul>
    <li><b>Actie vereist</b> (properties.incidentType ActionRequired ==) <dl>
            <dt>Informatief</dt>
            <dd>Administrator-actie vereist om te voorkomen dat gevolgen voor bestaande services</dd>
        </dl>
    </li>
    <li><b>Onderhoud</b> (properties.incidentType onderhoud ==) <dl>
            <dt>Waarschuwing</dt>
            <dd>noodgevallen onderhoud<dd>
            <dt>Informatief</dt>
            <dd>standaard gepland onderhoud</dd>
        </dl>
    </li>
    <li><b>Informatie</b> (properties.incidentType == informatie) <dl>
            <dt>Informatief</dt>
            <dd>Beheerder mogelijk nodig zijn om te voorkomen dat gevolgen voor bestaande services</dd>
        </dl>
    </li>
    <li><b>Beveiliging</b> (properties.incidentType == beveiliging) <dl>
            <dt>Fout</dt>
            <dd>Wijdverbreid problemen bij toegang tot meerdere services over meerdere regio's van invloed zijn op een heel scala aan klanten.</dd>
            <dt>Waarschuwing</dt>
            <dd>Problemen bij toegang tot specifieke services en/of specifieke gebieden van invloed zijn op een subset van klanten.</dd>
            <dt>Informatief</dt>
            <dd>Problemen die invloed hebben op beheerbewerkingen en/of latentie, niet van invloed op de beschikbaarheid van de service.</dd>
        </dl>
    </li>
    <li><b>Problemen met service</b> (properties.incidentType Incident ==) <dl>
            <dt>Fout</dt>
            <dd>Wijdverbreid problemen bij toegang tot meerdere services over meerdere regio's van invloed zijn op een heel scala aan klanten.</dd>
            <dt>Waarschuwing</dt>
            <dd>Problemen bij toegang tot specifieke services en/of specifieke gebieden van invloed zijn op een subset van klanten.</dd>
            <dt>Informatief</dt>
            <dd>Problemen die invloed hebben op beheerbewerkingen en/of latentie, niet van invloed op de beschikbaarheid van de service.</dd>
        </dl>
    </li>
  </ul>

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
