---
title: Overzicht van back-ends met meerdere tenants via Azure Application Gateway | Microsoft Docs
description: Op deze pagina wordt de ondersteuning voor back-ends met meerdere tenants in Application Gateway beschreven.
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: 
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.openlocfilehash: c29ff60a50e68c75b4e8f62713d6d1fffd2123d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="application-gateway-support-for-multi-tenant-back-ends"></a>Application Gateway-ondersteuning voor back-ends met meerdere tenants

Azure Application Gateway biedt ondersteuning voor virtuele-machineschaalsets, netwerkinterfaces, openbare en persoonlijke IP-adressen en volledig gekwalificeerde domeinnamen (FQDN) als onderdeel van de back-endpools. Application Gateway wijzigt standaard niet de binnenkomende HTTP-host-header van de client en stuurt de header ongewijzigd terug naar de back-end. Er zijn veel services zoals [Azure Web Apps](../app-service/app-service-web-overview.md) en [API Management](../api-management/api-management-key-concepts.md) die van nature werken met meerdere tenants en vertrouwen op een specifieke host-header of SNI-extensie om te kunnen omzetten naar de juiste eindpunten. Application Gateway biedt nu ondersteuning voor de mogelijkheid van gebruikers om de binnenkomende HTTP-host-header te overschrijven op basis van de back-end-HTTP-instellingen. Dankzij deze mogelijkheid is er ondersteuning voor Azure-web-apps en API-beheer met back-ends met meerdere tenants. Deze mogelijkheid is beschikbaar voor zowel de standaard- als de WAF-SKU. De ondersteuning voor back-ends met meerdere tenants omvat ook opties voor SSL-beëindiging en end-to-end SSL-scenario's.

![web-app-scenario](./media/application-gateway-web-app-overview/scenario.png)

De mogelijkheid om hosts te overschrijven kan worden gebruikt in de HTTP-instellingen en kan worden toegepast op alle back-endpools tijdens het maken van regels. Back-ends met meerdere tenants bieden ondersteuning voor twee manieren om host-headers en SNI-extensies te overschrijven.

1. De mogelijkheid om de hostnaam in te stellen op een vaste waarde in de HTTP-instellingen. Met deze mogelijkheid wordt de host-header overschreven met deze waarde voor al het verkeer naar de back-endpool waarin de HTTP-instellingen worden toegepast. Als u end-to-end SSL gebruikt, wordt de overschreven hostnaam gebruikt in de SNI-extensie. Dankzij deze mogelijkheid zijn er scenario's mogelijk waarin een back-endpoolfarm een host-header verwacht die verschilt van de binnenkomende host-header van de klant.

2. De mogelijkheid om de hostnaam af te leiden van het IP-adres of de FQDN van de leden van de back-endpool. HTTP-instellingen bieden ook de mogelijkheid om de hostnaam van de FQDN van een lid van een back-endpool te kiezen, indien dit is geconfigureerd bij de optie om hostnamen van afzonderlijke back-endpoolleden af te leiden. Als u end-to-end SSL gebruikt, wordt de hostnaam afgeleid van de FQDN en gebruikt in de SNI-extensie. Met deze mogelijkheid zijn er scenario's mogelijk waarin een back-endpool twee of meer PaaS-services met meerdere tenants heeft, zoals Azure-web-apps. De host-header van elk lid bevat de hostnaam die is afgeleid van de FQDN.

> [!NOTE]
> In beide bovenstaande gevallen hebben de instellingen alleen invloed op het liveverkeersgedrag en niet op het gedrag van de statustest. Aangepaste tests bieden de mogelijkheid al om een host-header op te geven in de testconfiguratie. Bij aangepaste tests kunt u nu ook het host-headergedrag afleiden van de op dat moment geconfigureerde HTTP-instellingen. Deze configuratie kan worden opgegeven met behulp van de parameter `PickHostNameFromback endAddress` in de testconfiguratie. Als u wilt dat de end-to-end configuratie werkt, moeten zowel de test als de HTTP-instellingen worden bewerkt op basis van de juiste configuratie.

Met deze functie kunnen klanten opties opgeven in de HTTP-instellingen en kunnen ze aangepaste tests uitvoeren voor de gewenste configuratie. De instellingen worden vervolgens middels een regel gekoppeld aan een listener en een back-endpool.

## <a name="next-steps"></a>Volgende stappen

Zie [App Service Web Apps configureren met Application Gateway](application-gateway-web-app-powershell.md) voor meer informatie over het instellen van een toepassingsgateway met een web-app als lid van een back-endpool
