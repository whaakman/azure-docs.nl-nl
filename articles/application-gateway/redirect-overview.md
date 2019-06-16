---
title: Overzicht van omleiden voor Azure Application Gateway
description: Meer informatie over de mogelijkheden voor omleiding in Azure Application Gateway
services: application-gateway
documentationcenter: na
author: amsriva
manager: jpconnock
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/19/2018
ms.author: amsriva
ms.openlocfilehash: 8e88e0e11b3ccab7cc2c68b2617df2d588680780
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60715794"
---
# <a name="application-gateway-redirect-overview"></a>Overzicht van Application Gateway redirect

Application gateway kunt u verkeer omleiden.  De service heeft een algemeen omleidingsmechanisme om verkeer dat is ontvangen door de ene listener om te leiden naar een andere listener of naar een externe site. Dit vereenvoudigt de configuratie van toepassing, optimaliseert het gebruik van bronnen en biedt ondersteuning voor nieuwe omleiding-scenario's, met inbegrip van omleiding van globale en op basis van een pad.

Een veelvoorkomend scenario omleiding voor veel webtoepassingen wordt voor de ondersteuning van automatische HTTP naar HTTPS-omleiding om te controleren of dat alle communicatie tussen de toepassing en de gebruikers daarvan vindt plaats via een gecodeerde pad. Klanten hebben in het verleden technieken zoals het maken van een toegewezen back endpool met als enig doel is om te leiden van HTTP naar HTTPS ontvangen aanvragen worden gebruikt. Met de ondersteuning van omleiding in Application Gateway, kunt u dit doen door een nieuwe omleidings-configuratie toe te voegen aan een regel voor het doorsturen en een ander listener met HTTPS-protocol als de doel-listener op te geven.

De volgende typen omleiding worden ondersteund:

- 301: permanente omleiding
- 302 gevonden
- 303 Zie overige
- 307 Temporary Redirect

Ondersteuning voor Application Gateway-omleiding biedt de volgende mogelijkheden:

-  **Globale omleiding**

   Omleidingen van één listener voor een ander listener op de gateway. Hierdoor is HTTP-naar-HTTPS-omleiding op een site mogelijk.
- **Op pad gebaseerde omleiding**

   Dit type omleiding kunt HTTP naar HTTPS-omleiding alleen op het gebied van een specifieke site, bijvoorbeeld een winkelwagentje winkelwagen gebied aangeduid met/winkelwagen / *.
- **Omleiden naar de externe site**

![redirect](./media/redirect-overview/redirect.png)

Met deze wijziging moeten klanten maken van een nieuwe omleiden configuration-object, dat aangeeft van de doel-listener of een externe site waarvoor omleiding is vereist. De configuratie-element ondersteunt ook opties voor het inschakelen van de URI-pad en de query-tekenreeks toe te voegen aan de omgeleide URL. U kunt ook het type van omleiding. Eenmaal gemaakt, wordt deze omleiding-configuratie is gekoppeld aan de bron-listener via een nieuwe regel. Wanneer u een basisregel, wordt de configuratie van de omleidings-is gekoppeld aan de listener van een bron en is een algemene omleiding. Als u een pad gebaseerde regel gebruikt, wordt de configuratie van de omleidings-is gedefinieerd in de URL-path-map. Zodat deze alleen van toepassing op het padgebied van het specifieke van een site.

### <a name="next-steps"></a>Volgende stappen

[URL-omleiding in een toepassingsgateway configureren](tutorial-url-redirect-powershell.md)
