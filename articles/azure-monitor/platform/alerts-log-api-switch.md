---
title: Overschakelen van de verouderde Log Analytics waarschuwingen API in de nieuwe API voor Azure-waarschuwingen
description: Overzicht van buiten gebruik stellen van verouderde savedSearch op basis van Log Analytics-waarschuwing API en proces als u wilt overschakelen van regels voor waarschuwingen naar nieuwe ScheduledQueryRules API met details van algemene problemen van klanten-adressering.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/24/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 8e123705443c44373734a81936459a56bbb259cc
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55105982"
---
# <a name="switch-api-preference-for-log-alerts"></a>Voorkeur voor switch API voor Logboekwaarschuwingen

> [!NOTE]
> Inhoud vermeld **niet** van toepassing op gebruikers van Azure GovCloud en alleen gebruikers van de openbare cloud van Azure.  

Tot voor kort beheerd u regels voor waarschuwingen in de Microsoft Operations Management Suite-portal. De nieuwe ervaring voor waarschuwingen is geïntegreerd met verschillende services in Microsoft Azure, inclusief Log Analytics en we gevraagd [uw regels voor waarschuwingen van OMS-portal uitbreiden naar Azure](alerts-extend.md). Maar om ervoor te zorgen minimale verstoring voor klanten, het proces heeft geen invloed op de programma-interface voor het verbruik - [Log Analytics-waarschuwing API](api-alerts.md) op basis van SavedSearch.

Maar nu u een echte Azure programmatische alternatief aankondigen voor Log Analytics waarschuwingen gebruikers [Azure Monitor - ScheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), dit is ook bij reflectieve in uw [Azure billing - voor logboekwaarschuwingen](alerts-unified-log.md#pricing-and-billing-of-log-alerts). Zie voor meer informatie over het beheren van uw waarschuwingen met behulp van de API, [waarschuwingen beheren met behulp van Azure-Resourcesjabloon](alerts-log.md#managing-log-alerts-using-azure-resource-template) en [waarschuwingen beheren met behulp van PowerShell, CLI of API](alerts-log.md#managing-log-alerts-using-powershell-cli-or-api).

## <a name="benefits-of-switching-to-new-azure-api"></a>Voordelen van het overschakelen naar de nieuwe Azure-API

Er zijn enkele voordelen van het maken en beheren van waarschuwingen via [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) via [verouderde Log Analytics-waarschuwing API](api-alerts.md); er is een lijst enkele van de volgende belangrijkste oplossingen:

- Mogelijkheid om te [cross-werkruimte zoeken in logboeken](../log-query/cross-workspace-query.md) in regels voor waarschuwingen en het bereik van externe bronnen, zoals Log Analytics-werkruimten of zelfs Application Insights-apps
- Met behulp van meerdere velden die aan de groep in de query, [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) gebruiker kunt opgeven welk veld moet een statistische functie op in Azure portal
- Waarschuwingsregels maken in één keer als één resource zonder de noodzaak om te maken van drie niveaus van resources als [verouderde Log Analytics-waarschuwing API](api-alerts.md)
- Enkele programma-interface voor alle varianten van query's gebaseerde logboekwaarschuwingen in Azure - nieuwe [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kan worden gebruikt voor het beheren van regels voor Log Analytics, evenals de Application Insights
- Alle waarschuwingen functionaliteit voor het nieuwe logboekbestanden en toekomstige ontwikkeling is beschikbaar via de nieuwe [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)

## <a name="process-of-switching-from-legacy-log-alerts-api"></a>Proces van het overschakelen van verouderde Log-API voor waarschuwingen

Het proces van servermodi waarschuwingsregels van [verouderde Log Analytics-waarschuwing API](api-alerts.md) heeft geen betrekking op de meldingsdefinitie, query of configuratie in geen enkele manier wijzigen. Gebruikers zijn gratis voor [verouderde Log Analytics-waarschuwing API](api-alerts.md) of de nieuwe [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Waarschuwingsregels die zijn gemaakt door een van beide API, zal *worden beheerd door de dezelfde API alleen* - en als u vanuit de Azure-portal. Standaard Azure Monitor worden echter ook doorgaan met [verouderde Log Analytics-waarschuwing API](api-alerts.md) voor het maken van een nieuwe waarschuwingsregel vanuit Azure portal.

De gevolgen van de switch van voorkeur aan scheduledQueryRules API worden gecompileerd hieronder:

- Alle interacties gedaan voor het beheren van waarschuwingen via programmatische interfaces moet nu worden uitgevoerd met [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) in plaats daarvan. Voor meer informatie ziet, [voorbeeld gebruiken via Azure Resource-sjabloon](alerts-log.md#managing-log-alerts-using-azure-resource-template) en [voorbeeld gebruiken via Azure CLI en PowerShell](alerts-log.md#managing-log-alerts-using-powershell-cli-or-api)
- Een nieuwe waarschuwingsregel hebt gemaakt in Azure portal worden gemaakt met [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) alleen en gebruikers gebruiken de [aanvullende functionaliteit van de nieuwe API](#Benefits-of-switching-to-new-Azure-API) via Azure portal ook

Elke klant die wil vrijwillig overschakelen naar de nieuwe [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) en blokkeren van gebruik van de [verouderde Log Analytics-waarschuwing API](api-alerts.md); kunt dit doen door het uitvoeren van een PUT-aanroep op de onderstaande API om over te schakelen van alle waarschuwing regels die zijn gekoppeld aan de specifieke Log Analytics-werkruimte.

```
PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Met de aanvraag hoofdtekst die de onderstaande JSON.

```json
{
    "scheduledQueryRulesEnabled" : true
}
```

Als switch van alle regels voor waarschuwingen in Log Analytics-werkruimte gebruik van nieuwe [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) is geslaagd, het volgende antwoord wordt geleverd.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```

Gebruikers kunnen ook de huidige status van uw Log Analytics-werkruimte controleren en zien of deze is of is niet ingesteld voor het gebruik van [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) alleen. Als u wilt controleren, kunnen gebruikers een GET-aanroep uitvoeren op de onderstaande API.

```
GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Als de opgegeven Log Analytics-werkruimte is overgeschakeld naar het gebruik van [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) alleen; vervolgens het antwoord JSON worden zoals hieronder vermeld.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```
Anders, als de opgegeven Log Analytics-werkruimte nog niet is overgeschakeld voor het gebruik van [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) alleen; vervolgens het antwoord JSON worden zoals hieronder vermeld.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : false
}
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [Azure Monitor - waarschuwingen](alerts-unified-log.md).
- Meer informatie over het maken van [waarschuwingen voor activiteitenlogboeken in Azure-waarschuwingen](alerts-log.md).
- Meer informatie over de [ervaren Azure-waarschuwingen](../../azure-monitor/platform/alerts-overview.md).
