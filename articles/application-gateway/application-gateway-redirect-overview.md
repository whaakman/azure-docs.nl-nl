---
title: Overzicht van omleiden voor Azure Application Gateway | Microsoft Docs
description: Meer informatie over de mogelijkheden voor omleiding in Azure Application Gateway
services: application-gateway
documentationcenter: na
author: amsriva
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/18/2017
ms.author: amsriva
ms.openlocfilehash: d05d509b67fd26c958e0e2fa2bbd877db26e6521
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54232206"
---
# <a name="application-gateway-redirect-overview"></a>Overzicht van Application Gateway redirect

Een veelvoorkomend scenario voor veel webtoepassingen is voor de ondersteuning van automatische HTTP naar HTTPS-omleiding om te controleren of dat alle communicatie tussen de toepassing en de gebruikers daarvan vindt plaats via een gecodeerde pad. Klanten hebben in het verleden technieken zoals het maken van een toegewezen back endpool met als enig doel is om te leiden van HTTP naar HTTPS ontvangen aanvragen worden gebruikt.  Application gateway ondersteunt nu omleiden van verkeer op de Application Gateway. Dit vereenvoudigt de configuratie van toepassing, optimaliseert het gebruik van bronnen en biedt ondersteuning voor nieuwe omleiding-scenario's, met inbegrip van omleiding van globale en op basis van een pad. Application Gateway-omleiding ondersteuning is niet beperkt tot HTTP -> alleen HTTPS-omleiding. Dit is een mechanisme algemene omleiding, waarmee voor omleiden van verkeer dat is ontvangen door een listener voor een ander listener voor een Application Gateway. Ook omleiding naar een externe site wordt ondersteund. Ondersteuning voor Application Gateway-omleiding biedt de volgende mogelijkheden:

1. Globale omleiding van één listener naar een ander listener voor de Gateway. Hierdoor is HTTP-naar-HTTPS-omleiding op een site mogelijk.
2. Padgebaseerde omleiding. Dit type omleiding kunt HTTP naar HTTPS-omleiding alleen op het gebied van een specifieke site, bijvoorbeeld een winkelwagentje winkelwagen gebied aangeduid met/winkelwagen / *.
3. Omleiden naar de externe site.

![omleiden](./media/application-gateway-redirect-overview/redirect.png)

Met deze wijziging moeten klanten zou maken van een nieuwe omleiden configuration-object, dat aangeeft van de doel-listener of een externe site waarvoor omleiding is vereist. De configuratie-element ondersteunt ook opties voor het inschakelen van de URI-pad en de query-tekenreeks toe te voegen aan de omgeleide URL. Klanten kunnen er ook voor kiezen of omleiding een tijdelijke (HTTP-statuscode 302) of een permanente omleiding (HTTP-statuscode 301 is). Als deze configuratie omleiden gemaakt is gekoppeld aan de bron-listener via een nieuwe regel. Wanneer u een basisregel, wordt de configuratie van de omleidings-is gekoppeld aan de listener van een bron en is een algemene omleiding. Wanneer een pad gebaseerde regel wordt gebruikt, wordt de configuratie van de omleiding is gedefinieerd in de URL-path-map en daarom alleen van toepassing op het padgebied van het specifieke van een site.

### <a name="next-steps"></a>Volgende stappen

[HTTP naar HTTPS-omleiding in een toepassingsgateway configureren](redirect-http-to-https-portal.md)
