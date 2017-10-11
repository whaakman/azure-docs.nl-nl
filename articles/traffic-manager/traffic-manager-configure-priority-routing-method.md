---
title: Prioriteit verkeersrouteringsmethode configureren met behulp van Azure Traffic Manager | Microsoft Docs
description: Dit artikel wordt uitgelegd hoe u configureert de prioriteit verkeersrouteringsmethode in Traffic Manager
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 6dca6de1-18f7-4962-bd98-6055771fab22
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: 0db83cde6facc89b8b8aa72e6419129ec868235c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-priority-traffic-routing-method-in-traffic-manager"></a>Prioriteit verkeersrouteringsmethode in Traffic Manager configureren

Ongeacht de websitemodus bieden Azure Websites al failover-functionaliteit voor websites binnen een datacenter (ook wel bekend als een regio). Traffic Manager biedt failover voor websites in verschillende datacenters.

Een algemene patroon voor de service failover is het verkeer naar een primaire service verzenden en leveren een verzameling van identieke back-services voor failover. De volgende stappen wordt uitgelegd hoe deze prioriteit failover configureren met Azure-cloudservices en websites:

## <a name="to-configure-the-priority-traffic-routing-method"></a>De verkeersrouteringsmethode prioriteit configureren

1. Meld u vanuit een browser aan bij [Azure Portal](http://portal.azure.com). Als u nog geen account hebt, kunt u zich registreren voor een [gratis proefversie van één maand](https://azure.microsoft.com/free/). 
2. Zoek in de portal zoekbalk, op de **Traffic Manager-profielen** en klik vervolgens op de naam van het profiel dat u wilt de routeringsmethode voor configureren.
3. In de **Traffic Manager-profiel** blade, Controleer of de cloudservices en de websites die u wilt opnemen in uw configuratie aanwezig zijn.
4. In de **instellingen** sectie, klikt u op **configuratie**, en in de **configuratie** blade voltooid zijn als volgt:
    1. Voor **verkeer van de instellingen voor verkeersroutering methode**, Controleer of de verkeersrouteringsmethode **prioriteit**. Als dit niet het geval is, klikt u op **prioriteit** uit de vervolgkeuzelijst.
    2. Stel de **monitor eindpuntinstellingen** identiek voor alle elk eindpunt binnen dit profiel als volgt:
        1. Selecteer de relevante **Protocol**, en geef de **poort** getal. 
        2. Voor **pad** Typ een slash  */* . U moet een pad en bestandsnaam opgeven voor het bewaken van eindpunten. Een schuine streep naar voren '/' is een geldige invoer voor het relatieve pad en impliceert dat het bestand zich in de hoofdmap (standaard).
        3. Klik boven aan de pagina op **opslaan**.
5. In de **instellingen** sectie, klikt u op **eindpunten**.
6. In de **eindpunten** blade bekijken van de volgorde van prioriteit voor uw eindpunten. Wanneer u selecteert de **prioriteit** verkeersrouteringsmethode, de volgorde van de geselecteerde eindpunten voor u belangrijk is. Controleer of de prioriteitsvolgorde van eindpunten.  Het primaire eindpunt is op de voorgrond. Controleer op de volgorde die wordt weergegeven. alle aanvragen worden doorgestuurd naar het eerste eindpunt en als Traffic Manager wordt gedetecteerd, worden niet in orde, het verkeer automatisch wordt overgenomen door het volgende eindpunt. 
7. De prioriteitsvolgorde eindpunt wijzigen, klikt u op het eindpunt en in de **eindpunt** blade die wordt weergegeven, klikt u op **bewerken** en wijzig de **prioriteit** waarde indien nodig. 
8. Klik op **opslaan** Sla de eindpuntinstellingen wijzigen.
9. Nadat u uw wijzigingen in de configuratie hebt voltooid, klikt u op **opslaan** aan de onderkant van de pagina.
10. De wijzigingen in uw configuratie als volgt testen:
    1.  In de portal zoekbalk, zoek de naam van het Traffic Manager-profiel en klik op het Traffic Manager-profiel in de resultaten die de weergegeven.
    2.  In de **Traffic Manager** blade profiel, klikt u op **overzicht**.
    3.  De **Traffic Manager-profiel** blade bevat de DNS-naam van de zojuist gemaakte Traffic Manager-profiel. Dit kan worden gebruikt door clients (bijvoorbeeld door te navigeren naar het met een webbrowser) te verkrijgen gerouteerd naar het juiste eindpunt als bepaald door het type routering. In dit geval alle aanvragen worden doorgestuurd naar het eerste eindpunt en als Traffic Manager wordt gedetecteerd, worden niet in orde, het verkeer automatisch wordt overgenomen door het volgende eindpunt.
11. Zodra uw Traffic Manager-profiel werkt, bewerkt u de DNS-record op de gezaghebbende DNS-server de naam van uw bedrijf domein verwijzen naar de naam van het Traffic Manager-domein.

![Prioriteit verkeersrouteringsmethode met Traffic Manager configureren][1]

## <a name="next-steps"></a>Volgende stappen


- Meer informatie over [verkeersrouteringsmethode gewogen](traffic-manager-configure-weighted-routing-method.md).
- Meer informatie over [routeringsmethode voor prestaties](traffic-manager-configure-performance-routing-method.md).
- Meer informatie over [routeringsmethode voor geografische](traffic-manager-configure-geographic-routing-method.md).
- Meer informatie over hoe [Traffic Manager-instellingen testen](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-priority-routing-method/traffic-manager-priority-routing-method.png