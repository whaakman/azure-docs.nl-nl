---
title: Verkeer beheren met Traffic Manager - Azure App Service
description: Dit artikel bevat overzichtsinformatie voor Azure Traffic Manager als deze zich tot Azure App Service verhoudt.
services: app-service\web
documentationcenter: ''
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: dabda633-e72f-4dd4-bf1c-6e945da456fd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 85405a96d141188203ddc88410c96654667fa83a
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53270107"
---
# <a name="controlling-azure-app-service-traffic-with-azure-traffic-manager"></a>Azure App Service-verkeer met Azure Traffic Manager beheren
> [!NOTE]
> Dit artikel bevat overzichtsinformatie voor Microsoft Azure Traffic Manager als deze zich tot Azure App Service verhoudt. Meer informatie over Azure Traffic Manager zelf kan worden gevonden door naar de koppelingen aan het einde van dit artikel te gaan.
> 
> 

## <a name="introduction"></a>Inleiding
U kunt Azure Traffic Manager gebruiken om te bepalen hoe aanvragen van web-clients worden gedistribueerd naar apps in Azure App Service. Wanneer App Service-eindpunten worden toegevoegd aan een Azure Traffic Manager-profiel, houdt Azure Traffic Manager de status van uw App Service-apps bij (actief, gestopt of verwijderd) zodat de service kan bepalen welke van deze eindpunten verkeer moeten ontvangen.

## <a name="routing-methods"></a>Routeringsmethoden
Met Azure Traffic Manager maakt gebruik van vier verschillende methoden voor het doorsturen. Deze methoden worden beschreven in de volgende lijst zoals ze betrekking op Azure App Service hebben.

* **[Prioriteit](../traffic-manager/traffic-manager-routing-methods.md#priority):** een primaire app gebruiken voor al het verkeer en back-ups opgeven als de primaire of de back-apps niet beschikbaar zijn.
* **[Gewogen](../traffic-manager/traffic-manager-routing-methods.md#weighted):** verkeer distribueert naar een set apps, gelijkmatig of op basis van gewicht, die u definieert.
* **[Prestaties](../traffic-manager/traffic-manager-routing-methods.md#performance):** wanneer u apps in verschillende geografische locaties hebt, gebruik van de app "dichtstbijzijnde" wat betreft de laagste netwerklatentie.
* **[Geografische](../traffic-manager/traffic-manager-routing-methods.md#geographic):** direct gebruikers aan specifieke apps op basis van op welke geografische locatie hun DNS-query is afkomstig uit. 

Zie voor meer informatie, [methoden voor het doorsturen van Traffic Manager](../traffic-manager/traffic-manager-routing-methods.md).

## <a name="app-service-and-traffic-manager-profiles"></a>App Service en Traffic Manager-profielen
Voor het configureren van het besturingselement van het verkeer van App Service-app, u maakt een profiel in Azure Traffic Manager dat maakt gebruik van een van de drie die eerder zijn beschreven methoden voor taakverdeling laden en voegt u de eindpunten (in dit geval wordt een App Service) waarvoor u wilt voor het beheren van verkeer naar de profiel. De status van uw app (die wordt uitgevoerd, gestopt of verwijderd) regelmatig wordt doorgegeven aan het profiel zodat Azure Traffic Manager dat verkeer dienovereenkomstig instellen kunt.

Als u Azure Traffic Manager voor Azure, houd rekening met de volgende punten:

* Voor app-implementaties binnen dezelfde regio biedt App Service al failover en round robin-functionaliteit ongeacht de app-modus.
* Voor implementaties in dezelfde regio die het gebruik van App Service in combinatie met een andere Azure-cloudservice, kunt u beide typen eindpunten om in te schakelen van hybride scenario's te combineren.
* U kunt slechts één App Service-eindpunt per regio opgeven in een profiel. Wanneer u een app als een eindpunt voor één regio selecteert, worden de resterende apps in deze regio niet beschikbaar voor selectie voor dit profiel.
* De App Service-eindpunten die u opgeeft in een Azure Traffic Manager-profiel wordt weergegeven onder de **domeinnamen** sectie op de pagina configureren voor de app in het profiel, maar kan niet worden geconfigureerd er.
* Nadat u een app aan een profiel toevoegen, de **Site-URL** op het Dashboard van de portal op de pagina van de app wordt het aangepaste domein-URL van de app weergegeven als u een hebt ingesteld. Anders wordt de Traffic Manager-profiel-URL (bijvoorbeeld `contoso.trafficmanager.net`). Zowel de directe domeinnaam van de app en de Traffic Manager-URL worden weergegeven op de configuratiepagina van de app onder de **domeinnamen** sectie.
* Uw aangepaste domeinnamen werken zoals verwacht, maar naast deze toe te voegen aan uw apps, moet u uw DNS-kaart om te verwijzen naar de Traffic Manager-URL ook configureren. Zie voor meer informatie over het instellen van een aangepast domein voor een App Service-app [een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](app-service-web-tutorial-custom-domain.md).
* U kunt alleen apps die in de standard- of premium-modus voor een Azure Traffic Manager-profiel zijn toevoegen.

## <a name="next-steps"></a>Volgende stappen
Zie voor een conceptuele en technisch overzicht van Azure Traffic Manager, [overzicht Traffic Manager](../traffic-manager/traffic-manager-overview.md).

Zie voor meer informatie over het gebruik van Traffic Manager met App Service, de blogberichten [met behulp van Azure Traffic Manager with Azure Web Sites](https://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) en [Azure Traffic Manager kan nu worden geïntegreerd met Azure websites](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).

