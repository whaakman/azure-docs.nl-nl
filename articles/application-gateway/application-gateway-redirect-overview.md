---
title: Overzicht omleiden voor Azure Application Gateway | Microsoft Docs
description: Meer informatie over de mogelijkheden voor omleiding in Azure Application Gateway
services: application-gateway
documentationcenter: na
author: amsriva
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/18/2017
ms.author: amsriva
ms.openlocfilehash: ea9ae8373ff67bf9557b06bbc8a4b0d82a03e2d0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="application-gateway-redirect-overview"></a>Overzicht van Application Gateway-omleiding

Een veelvoorkomend scenario voor veel webtoepassingen is de ondersteuning van automatische HTTP naar HTTPS-omleiding zodat alle communicatie tussen de toepassing en haar gebruikers vindt plaats via een versleutelde pad. In het verleden hebt klanten technieken zoals het maken van een toegewezen back-endpool met als enige doel is om te leiden aanvragen van HTTP naar HTTPS ontvangen gebruikt.  Application gateway ondersteunt nu de mogelijkheid verkeer in de toepassingsgateway omleiden. Dit vereenvoudigt de configuratie van toepassing, optimaliseert het brongebruik en nieuwe scenario's voor omleiding omleiding van globale en op basis van een pad inclusief ondersteunt. Ondersteuning voor toepassingen Gateway omleiding is niet beperkt tot HTTP-omleiding van HTTPS alleen >. Dit is een algemene omleiding mechanisme, waardoor de omleiding van verkeer dat op één listener voor een ander listener voor Application Gateway wordt ontvangen. Het ondersteunt ook een omleiding naar een externe site ook. Ondersteuning voor toepassingen Gateway-omleiding biedt de volgende mogelijkheden:

1. Globale omleiding van één listener naar een ander listener op de Gateway. Hierdoor kunnen HTTP naar HTTPS-omleiding op een site.
2. Omleiding op basis van het pad. Dit type omleiding kunt HTTP naar HTTPS-omleiding alleen op een specifieke site gebied, zoals een winkelwagen winkelwagen gebied aangeduid met/mandje / *.
3. Omleiden naar de externe site.

![omleiden](./media/application-gateway-redirect-overview/redirect.png)

Met deze wijziging heeft klanten zou moeten maken van een nieuw omleiding configuration-object waarmee de doel-listener of een externe site die omleiding gewenst is. Het configuratie-element ondersteunt ook opties voor het inschakelen van de URI-pad en query-tekenreeks toegevoegd aan de omgeleide URL. Klanten kunnen ook voor kiezen of omleiding een tijdelijke (HTTP-statuscode 302) of een permanente omleiding (HTTP-statuscode 301 is). Wanneer deze configuratie omleiding gemaakt is gekoppeld aan de bron-listener via een nieuwe regel. Wanneer u een eenvoudige regel gebruikt, wordt de configuratie van de omleiding is gekoppeld aan een bron-listener en is een globale omleiding. Wanneer een regel op basis van het pad wordt gebruikt, wordt de omleidings-configuratie wordt gedefinieerd op de kaart URL-pad en daarom alleen van toepassing op het gebied specifiek pad van een site.

### <a name="next-steps"></a>Volgende stappen

[URL-omleiding op een toepassingsgateway configureren](application-gateway-configure-redirect-powershell.md)
