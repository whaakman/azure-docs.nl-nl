---
title: IP-adressen in Azure App Service | Microsoft Docs
description: Hierin wordt beschreven hoe binnenkomende en uitgaande IP adressen worden gebruikt in App Service en het zoeken naar informatie over deze voor uw app.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: cephalin
ms.openlocfilehash: 906a5d511615c57b6ff807ac240a838c63917e66
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
ms.locfileid: "31789943"
---
# <a name="inbound-and-outbound-ip-addresses-in-azure-app-service"></a>Binnenkomende en uitgaande IP-adressen in Azure App Service

[Azure App Service](app-service-web-overview.md) is een multitenant-service, met uitzondering van [App Service-omgevingen](environment/intro.md). Apps die zich niet in een App Service-omgeving (niet in de [geïsoleerde laag](https://azure.microsoft.com/pricing/details/app-service/)) netwerkinfrastructuur delen met andere apps. Als gevolg hiervan de IP-adressen voor binnenkomende en uitgaande van een app kunnen afwijken en zelfs in bepaalde situaties kunnen wijzigen. 

[App Service-omgevingen](environment/intro.md) toegewezen netwerkinfrastructuren gebruiken, zodat apps die worden uitgevoerd in een App Service-omgeving statische, toegewezen IP-adressen voor binnenkomende en uitgaande verbindingen.

## <a name="when-inbound-ip-changes"></a>Als inkomende IP gewijzigd

Elke app heeft ongeacht het aantal schaalvergroting instanties, één binnenkomende IP-adres. Het binnenkomende IP-adres kan veranderen wanneer u een van de volgende acties uitvoeren:

- Een app verwijderen en opnieuw maken in een andere resourcegroep.
- Hiermee verwijdert u de laatste app in een resourcegroep _en_ regio combinatie en maak deze opnieuw.
- Verwijderen van een bestaand SSL-binding, zoals tijdens certificaatvernieuwing (Zie [certificaten vernieuwen](app-service-web-tutorial-custom-ssl.md#renew-certificates)).

## <a name="get-static-inbound-ip"></a>Vaste binnenkomende IP-adres ophalen

Soms wilt u mogelijk een speciale, statische IP-adres voor uw app. Als u een statisch binnenkomende IP-adres, moet u voor het configureren van een [op basis van IP-SSL-binding](app-service-web-tutorial-custom-ssl.md#bind-your-ssl-certificate). U kunt u hoeft niet daadwerkelijk SSL-functionaliteit voor het beveiligen van uw app, zelfs een zelfondertekend certificaat voor deze binding te uploaden. In een IP-gebaseerde SSL-binding is het certificaat gebonden aan het IP-adres zelf, geval App Service voorziet in een statische IP-adressen zodat deze zich voordoen. 

## <a name="when-outbound-ips-change"></a>Wanneer de uitgaande IP-adressen wijzigen

Ongeacht het aantal exemplaren van de uitgebreide heeft elke app een bepaald aantal uitgaande IP-adressen op een bepaald moment. Alle uitgaande verbinding uit de App Service-app, zoals het een back-end-database maakt gebruik van een van de uitgaande IP-adressen als het IP-adres van de bron. U kan niet weet dat vooraf welk IP-adres een bepaalde app-exemplaar gebruikt de uitgaande om verbinding te maken, zodat uw back-end-service moet de firewall alle uitgaande IP-adressen van uw app openen.

De set van uitgaande IP-adressen voor uw appwijzigingen wanneer u uw app tussen de lagere lagen schalen (**Basic**, **standaard**, en **Premium**) en de  **Premium-V2** laag.

U vindt de set met alle mogelijke uitgaande IP-adressen uw app gebruiken kunt, ongeacht de prijscategorie, door te zoeken naar de `possibleOutboundIPAddresses` eigenschap. Zie [uitgaande IP-adressen vinden](#find-outbound-ips).

## <a name="find-outbound-ips"></a>Uitgaande IP-adressen vinden

Voor de uitgaande IP-adressen die momenteel wordt gebruikt door uw app in de Azure portal, klikt u **eigenschappen** in de linkernavigatiebalk van uw app. 

U kunt dezelfde informatie vinden door de volgende opdracht in de [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
```

Alle mogelijke vinden uitgaande IP-adressen voor uw app, ongeacht de prijscategorie, voer de volgende opdracht de [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```

## <a name="next-steps"></a>Volgende stappen

Informatie over het inkomende verkeer te beperken door de bron-IP-adressen.

> [!div class="nextstepaction"]
> [Statische IP-adresbeperkingen](app-service-ip-addresses.md)
