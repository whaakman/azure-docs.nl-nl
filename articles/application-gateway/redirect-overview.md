---
title: Overzicht omleiden voor Azure Application Gateway
description: Meer informatie over de mogelijkheden voor omleiding in Azure Application Gateway
services: application-gateway
documentationcenter: na
author: amsriva
manager: timlt
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/19/2018
ms.author: amsriva
ms.openlocfilehash: f503998668f35ec5bc17db74ee74c4a26465ab04
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="application-gateway-redirect-overview"></a>Overzicht van Application Gateway-omleiding

Een veelvoorkomend scenario voor veel webtoepassingen is de ondersteuning van automatische HTTP naar HTTPS-omleiding zodat alle communicatie tussen de toepassing en haar gebruikers vindt plaats via een versleutelde pad. In het verleden hebt klanten technieken zoals het maken van een toegewezen back-endpool met als enige doel is om te leiden aanvragen van HTTP naar HTTPS ontvangen gebruikt.

Application Gateway ondersteunt nu de mogelijkheid om te leiden van verkeer op de gateway. Dit vereenvoudigt de configuratie van toepassing, optimaliseert het brongebruik en nieuwe scenario's voor omleiding omleiding van globale en op basis van een pad inclusief ondersteunt. Ondersteuning voor toepassingen Gateway omleiding is niet beperkt tot HTTP-omleiding van HTTPS alleen >. Er is een mechanisme met algemene omleiding, waardoor de omleiding van verkeer ontvangen bij een listener voor een ander listener voor een toepassingsgateway. Omleiding van de externe site wordt ook ondersteund.

Ondersteuning voor toepassingen Gateway-omleiding biedt de volgende mogelijkheden:

-  **Globale omleiding**

   Omleidingen vanaf één listener voor een ander listener op de gateway. Hierdoor kunnen HTTP naar HTTPS-omleiding op een site.
- **Op basis van een pad omleiding**

   Dit type omleiding kunt HTTP naar HTTPS-omleiding alleen op een specifieke site gebied, zoals een winkelwagen winkelwagen gebied aangeduid met/mandje / *.
- **Omleiden naar de externe site**

![omleiden](./media/redirect-overview/redirect.png)

Met deze wijziging moeten klanten maken van een nieuw omleiding configuration-object waarmee de doel-listener of een externe site die omleiding gewenst is. Het configuratie-element ondersteunt ook opties voor het inschakelen van de URI-pad en query-tekenreeks toegevoegd aan de omgeleide URL. U kunt ook kiezen of omleiding een tijdelijke (HTTP-statuscode 302) of een permanente omleiding (HTTP-statuscode 301 is). Zodra gemaakt, wordt deze configuratie omleiding is gekoppeld aan de bron-listener via een nieuwe regel. Wanneer u een eenvoudige regel gebruikt, wordt de configuratie van de omleiding is gekoppeld aan een bron-listener en is een globale omleiding. Wanneer een regel op basis van het pad wordt gebruikt, wordt de omleidings-configuratie op de kaart URL-pad gedefinieerd. Zodat deze alleen van toepassing op het gebied specifiek pad van een site.

### <a name="next-steps"></a>Volgende stappen

[URL-omleiding op een toepassingsgateway configureren](tutorial-url-redirect-powershell.md)
