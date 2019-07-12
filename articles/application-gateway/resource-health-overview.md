---
title: Overzicht van Azure Application Gateway Resource Health
description: In dit artikel wordt een overzicht van de resource health-functie voor Azure Application Gateway
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 7/9/2019
ms.author: victorh
ms.openlocfilehash: db29551a8150b70e797d45fe659482470c8aca2a
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659500"
---
# <a name="azure-application-gateway-resource-health-overview"></a>Overzicht van Azure Application Gateway Resource Health

[Azure Resource Health](../service-health/resource-health-overview.md) helpt u bij het diagnosticeren en ondersteuning krijgen wanneer een probleem met de Azure-service is van invloed op uw resources. U informeren te over de huidige en eerdere status van uw resources. En het biedt technische ondersteuning om te helpen u bij het beperken van problemen.

Voor Application Gateway, is Resource Health afhankelijk van signalen verzonden door de gateway om te beoordelen of deze in orde of niet is. Als de gateway niet in orde is, analyseert Resource Health aanvullende informatie om de oorzaak van het probleem vast te stellen. Het identificeert ook acties die Microsoft duurt of wat u kunt doen om het probleem te verhelpen.

Voor meer informatie over hoe status wordt beoordeeld, bekijk de volledige lijst met resourcetypen en statuscontroles in [Azure Resource Health](../service-health/resource-health-checks-resource-types.md#microsoftnetworkapplicationgateways).


De integriteitsstatus voor Application Gateway wordt weergegeven als een van de volgende statussen:

## <a name="available"></a>Beschikbaar

Een **beschikbaar** status betekent dat de service alle gebeurtenissen die invloed hebben op de status van de resource is niet gevonden. U ziet de **recent opgeloste problemen met** melding in gevallen waar de gateway is hersteld van niet-geplande uitvaltijd tijdens de afgelopen 24 uur.

![Beschikbare integriteitsstatus](media/resource-health-overview/available-full.png)

## <a name="unavailable"></a>Niet beschikbaar

Een **niet beschikbaar** status betekent dat de service heeft een lopende of een gebeurtenis voor niet-platform die van invloed is op de status van de gateway.

### <a name="platform-events"></a>Platform-gebeurtenissen

Platform-gebeurtenissen worden geactiveerd door meerdere onderdelen van de Azure-infrastructuur. Ze bevatten zowel geplande acties (bijvoorbeeld: gepland onderhoud) en onverwachte incidenten (bijvoorbeeld een niet-geplande host opnieuw opstarten).

Resource Health biedt extra informatie over de gebeurtenis en het herstelproces. Ook kunt u contact op met ondersteuning, zelfs als u geen een actieve Microsoft ondersteuning voor overeenkomst.

![De status niet beschikbaar](media/resource-health-overview/unavailable.png)

## <a name="unknown"></a>Onbekend

De **onbekende** de integriteitsstatus van de Resource Health geeft informatie over de gateway voor meer dan 10 minuten nog niet ontvangen. Deze status is niet een definitieve indicatie van de status van de gateway. Maar het is een belangrijk gegevenspunt in het proces voor het oplossen van problemen.

Als de gateway wordt uitgevoerd zoals verwacht, verandert de status in **beschikbaar** na een paar minuten.

Als u problemen ondervindt het **onbekende** integriteitsstatus kan wijzen dat een gebeurtenis in het platform is die betrekking hebben op de gateway.

![Onbekende status](media/resource-health-overview/unknown.png)

## <a name="degraded"></a>Verminderd

De **gedegradeerd** de integriteitsstatus van de geeft aan dat uw gateway is gedetecteerd verlies in de prestaties, maar dit nog steeds beschikbaar voor gebruik is.

![Degrated status](media/resource-health-overview/degraded.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het oplossen van Application Gateway Web Application Firewall (WAF), [oplossen Web Application Firewall (WAF) voor Azure Application Gateway](web-application-firewall-troubleshoot.md).