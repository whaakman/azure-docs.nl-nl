---
title: Azure App Service-verkeer met Azure Traffic Manager beheren
description: Dit artikel bevat samenvattingsinformatie voor Azure Traffic Manager, omdat deze zich tot Azure App Service verhoudt.
services: app-service\web
documentationcenter: 
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
ms.openlocfilehash: efb732e9be62313eb199cb2cfbb1fa4d2cde0282
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="controlling-azure-app-service-traffic-with-azure-traffic-manager"></a>Azure App Service-verkeer met Azure Traffic Manager beheren
> [!NOTE]
> Dit artikel bevat samenvattingsinformatie voor Microsoft Azure Traffic Manager, omdat deze zich tot Azure App Service verhoudt. Via de koppelingen aan het einde van dit artikel vindt meer informatie over Azure Traffic Manager zelf.
> 
> 

## <a name="introduction"></a>Inleiding
U kunt Azure Traffic Manager gebruiken om te bepalen hoe aanvragen van de webserver en webclients worden gedistribueerd naar apps in Azure App Service. Wanneer de App Service-eindpunten worden toegevoegd aan een Azure Traffic Manager-profiel, Azure Traffic Manager houdt van de status van uw App Service-apps (actief, gestopt of verwijderd) zodat u deze kunt bepalen welke van deze eindpunten verkeer ontvangt.

## <a name="routing-methods"></a>methoden voor het doorsturen
Azure Traffic Manager maakt gebruik van vier verschillende methoden voor het doorsturen. Deze methoden worden beschreven in de volgende lijst zoals ze betrekking op Azure App Service hebben.

* **[Prioriteit](#priority):** een primaire app gebruiken voor al het verkeer en back-ups bieden voor het geval de primaire of back-apps niet beschikbaar zijn.
* **[Gewogen](#weighted):** verkeer verdelen over een set van apps, gelijkmatig of volgens het gewicht, die u definieert.
* **[Prestaties](#performance):** wanneer u apps in verschillende geografische locaties hebt, de 'dichtstbijzijnde' app in termen van de laagste netwerklatentie gebruiken.
* **[Geografische](#geographic):** direct gebruikers tot bepaalde apps op basis van welke geografische locatie hun DNS-query is afkomstig uit. 

Zie voor meer informatie [methoden voor het doorsturen van Traffic Manager](../traffic-manager/traffic-manager-routing-methods.md).

## <a name="app-service-and-traffic-manager-profiles"></a>App Service en Traffic Manager-profielen
Voor het configureren van het besturingselement van het verkeer van App Service-app u in Azure Traffic Manager dat wordt gebruikt een van de drie taakverdeling methoden die hierboven is beschreven laden van een profiel maken en voeg vervolgens de eindpunten (in dit geval App Service) waarvoor u wilt beheren van verkeer naar de profiel. De status van uw app (actief, gestopt of verwijderd) regelmatig wordt gecommuniceerd aan het profiel zodat Azure Traffic Manager kunt dienovereenkomstig verkeer omleiden.

Als u Azure Traffic Manager met Azure, houd er rekening mee houden de volgende punten:

* Voor app-implementaties binnen dezelfde regio biedt App Service al failover- en round-robin functionaliteit zonder rekening te houden appmodus.
* Voor implementaties in dezelfde regio die App Service in combinatie met een andere Azure-cloudservice gebruiken, kunt u beide typen eindpunten hybride scenario's te maken kunt combineren.
* U kunt slechts één App Service-eindpunt per regio opgeven in een profiel. Wanneer u een app als een eindpunt voor één regio selecteert, worden de resterende apps in deze regio niet beschikbaar voor selectie voor dit profiel.
* De App Service-eindpunten die u opgeeft in een Azure Traffic Manager-profiel wordt weergegeven onder de **domeinnamen** sectie op de pagina configureren voor de app in het profiel, maar kan niet worden geconfigureerd er.
* Nadat u een app aan een profiel toevoegen, de **Site-URL** op het Dashboard van de app portal-pagina wordt de URL van het aangepaste domein van de app weergegeven als u een hebt ingesteld. Anders wordt de Traffic Manager-profiel-URL (bijvoorbeeld `contoso.trafficmanager.net`). Zowel de directe domeinnaam van de app en de Traffic Manager-URL worden weergegeven op de pagina van de app configureren onder de **domeinnamen** sectie.
* Uw aangepaste domeinnamen werkt zoals verwacht, maar naast deze toevoegt aan uw apps, moet u ook de DNS-kaart om te verwijzen naar de Traffic Manager-URL configureren. Zie voor informatie over het instellen van een aangepast domein voor een App Service-app, [aangepaste DNS-naam van een bestaande toewijzen aan Azure Web Apps](app-service-web-tutorial-custom-domain.md).
* U kunt alleen apps die in de modus standard of premium aan een Azure Traffic Manager-profiel toevoegen.

## <a name="next-steps"></a>Volgende stappen
Zie voor een conceptuele en technisch overzicht van Azure Traffic Manager, [Traffic Manager-overzicht](../traffic-manager/traffic-manager-overview.md).

Zie voor meer informatie over het gebruik van Traffic Manager met App Service, de blogberichten [met behulp van Azure Traffic Manager met Azure websites](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) en [Azure Traffic Manager kan nu worden geïntegreerd met Azure websites](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).

