---
title: Overzicht van meerdere tenants terug eindigt met Azure Application Gateway
description: Op deze pagina wordt de ondersteuning voor back-ends met meerdere tenants in Application Gateway beschreven.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 8/1/2018
ms.author: victorh
ms.openlocfilehash: 8e5f48e42a2a677622dae0b733f9d5af484d1c51
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53605800"
---
# <a name="application-gateway-support-for-multi-tenant-back-ends"></a>Application Gateway-ondersteuning voor back-ends met meerdere tenants

Azure Application Gateway biedt ondersteuning voor virtuele-machineschaalsets, netwerkinterfaces, openbare en persoonlijke IP- of FQDN-naam (Fully Qualified Domain Name) als onderdeel van de back-end-pools. Application Gateway wijzigt standaard niet de binnenkomende HTTP-host-header van de client en stuurt de header ongewijzigd terug naar de back-end. Er zijn veel services zoals [Azure App Service](../app-service/overview.md) die van nature werken met meerdere tenants en vertrouwen op een specifieke host-header of SNI-extensie op te lossen naar het juiste eindpunt. Application Gateway ondersteunt nu de mogelijkheid voor gebruikers wilt overschrijven van de binnenkomende HTTP-host-header op basis van de back-end-HTTP-instellingen. Dankzij deze mogelijkheid is er ondersteuning voor Azure-web-apps en API-beheer met back-ends met meerdere tenants. Deze mogelijkheid is beschikbaar voor zowel de standaard- als de WAF-SKU. Back-end-ondersteuning voor meerdere tenants werkt ook met SSL-beëindiging en end-to-end SSL-scenario's.

> [!NOTE]
> Certificaatinstallatie verificatie is niet vereist voor betrouwbare Azure-services zoals Azure Web Apps.

![web-app-scenario](./media/application-gateway-web-app-overview/scenario.png)

De mogelijkheid om op te geven van een host onderdrukking is gedefinieerd in de HTTP-instellingen en kan worden toegepast op alle back-end-adrespool tijdens het maken van de regel. Back-ends met meerdere tenants bieden ondersteuning voor twee manieren om host-headers en SNI-extensies te overschrijven.

1. De mogelijkheid om de hostnaam in te stellen op een vaste waarde in de HTTP-instellingen. Deze functionaliteit zorgt ervoor dat de host-header wordt overschreven met deze waarde voor al het verkeer naar de back-end-pool waar de HTTP-instellingen worden toegepast. Als u end-to-end SSL gebruikt, wordt de overschreven hostnaam gebruikt in de SNI-extensie. Op deze manier kunt scenario's waarin een back-end-pool-farm wordt verwacht dat een host-header die verschilt van de binnenkomende host-header van de klant.

2. De mogelijkheid om de hostnaam afgeleid van de IP- of FQDN-naam van de leden van de back-end-pool. HTTP-instellingen bieden ook de mogelijkheid om op te halen de hostnaam van een lid van de back-end-pool FQDN als geconfigureerd met de optie voor de hostnaam afgeleid van het lid van een afzonderlijke back-end-pool. Als u end-to-end SSL gebruikt, wordt de hostnaam afgeleid van de FQDN en gebruikt in de SNI-extensie. Op deze manier kunt scenario's waarin een back-end-pool kan twee of meer multitenant PaaS-services zoals Azure web-apps hebben en host-header op elk lid van de aanvraag bevat de hostnaam die is afgeleid van de FQDN.

> [!NOTE]
> In beide bovenstaande gevallen hebben de instellingen alleen invloed op het liveverkeersgedrag en niet op het gedrag van de statustest. Aangepaste tests bieden de mogelijkheid al om een host-header op te geven in de testconfiguratie. Bij aangepaste tests kunt u nu ook het host-headergedrag afleiden van de op dat moment geconfigureerde HTTP-instellingen. Deze configuratie kan worden opgegeven met behulp van de parameter `PickHostNameFromBackendHttpSettings` in de testconfiguratie. Als u wilt dat de end-to-end configuratie werkt, moeten zowel de test als de HTTP-instellingen worden bewerkt op basis van de juiste configuratie.

Met deze functie kunnen klanten opties opgeven in de HTTP-instellingen en kunnen ze aangepaste tests uitvoeren voor de gewenste configuratie. Deze instelling wordt vervolgens gekoppeld aan een listener en een back-end-adrespool met behulp van een regel.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het instellen van een toepassingsgateway met een web-app als lid van een back-end-pool door te bezoeken: [App Service WebApps configureren met Application Gateway](application-gateway-web-app-powershell.md)
