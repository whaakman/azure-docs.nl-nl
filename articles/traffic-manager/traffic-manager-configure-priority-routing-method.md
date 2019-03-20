---
title: Prioriteit verkeersrouteringsmethode configureren met behulp van Azure Traffic Manager | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u de prioriteit verkeersrouteringsmethode configureren in Traffic Manager
services: traffic-manager
documentationcenter: ''
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: 66c5bd9390d6fe0f26af66e18aed22c07a7da3e4
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58003104"
---
# <a name="configure-priority-traffic-routing-method-in-traffic-manager"></a>Routeringsmethode voor verkeer prioriteit configureren in Traffic Manager

Ongeacht de websitemodus bieden al failoverfunctionaliteit voor websites in een datacenter (ook wel bekend als een regio) Azure Websites. Traffic Manager kunt failover voor websites in verschillende datacenters.

Een algemeen patroon voor failover van de service is het verzenden van verkeer naar een primaire service en bieden een set identieke back-upservice voor failover. De volgende stappen wordt uitgelegd hoe u deze met prioriteit failover met Azure cloudservices en websites:

## <a name="to-configure-the-priority-traffic-routing-method"></a>De verkeersrouteringsmethode prioriteit configureren

1. Meld u vanuit een browser aan bij [Azure Portal](https://portal.azure.com). Als u nog geen account hebt, kunt u zich registreren voor een [gratis proefversie van één maand](https://azure.microsoft.com/free/). 
2. Zoek in de zoekbalk van de portal, de **Traffic Manager-profielen** en klik vervolgens op de naam van het profiel dat u wilt de routeringsmethode voor configureren.
3. In de **Traffic Manager-profiel** blade controleren of de cloudservices en de websites die u wilt opnemen in uw configuratie weergegeven worden.
4. In de **instellingen** sectie, klikt u op **configuratie**, en klik in de **configuratie** blade voltooid zijn als volgt te werk:
    1. Voor **traffic routing-methode-instellingen**, Controleer of de verkeersrouteringsmethode **prioriteit**. Als dit niet het geval is, klikt u op **prioriteit** in de vervolgkeuzelijst.
    2. Stel de **monitor eindpuntinstellingen** identiek voor alle elk eindpunt binnen dit profiel als volgt te werk:
        1. Selecteer de juiste **Protocol**, en geef de **poort** getal. 
        2. Voor **pad** typt u een slash */*. Voor het controleren van eindpunten, moet u een pad en bestandsnaam opgeven. Een schuine streep naar voren '/' is een geldige vermelding voor het relatieve pad en geeft aan dat het bestand is in de hoofdmap (standaard).
        3. Aan de bovenkant van de pagina, klikt u op **opslaan**.
5. In de **instellingen** sectie, klikt u op **eindpunten**.
6. In de **eindpunten** blade, Controleer de volgorde van prioriteit voor uw eindpunten. Wanneer u selecteert de **prioriteit** routeringsmethode voor verkeer, de volgorde van de geselecteerde eindpunten belangrijk is. Controleer of de volgorde van prioriteit van eindpunten.  Het primaire eindpunt is op de voorgrond. Controleer op de volgorde die wordt weergegeven. alle aanvragen worden doorgestuurd naar het eerste eindpunt en als Traffic Manager wordt gedetecteerd, worden niet in orde, wordt het verkeer automatisch failover-schakeling naar het eindpunt van de volgende. 
7. De volgorde van de eindpunt-prioriteit wijzigen, klikt u op het eindpunt, en klik in de **eindpunt** blade die wordt weergegeven, klikt u op **bewerken** en wijzig de **prioriteit** waarde indien nodig. 
8. Klik op **opslaan** opslaan de eindpuntinstellingen wijzigen.
9. Nadat u uw wijzigingen in de configuratie hebt voltooid, klikt u op **opslaan** aan de onderkant van de pagina.
10. De wijzigingen in uw configuratie als volgt testen:
    1.  In de zoekbalk van de portal, zoek de naam van het Traffic Manager-profiel en klik op het Traffic Manager-profiel in de resultaten die de weergegeven.
    2.  In de **Traffic Manager** blade profiel, klikt u op **overzicht**.
    3.  De **Traffic Manager-profiel** blade wordt weergegeven voor de DNS-naam van uw zojuist gemaakte Traffic Manager-profiel. Dit kan worden gebruikt door clients (bijvoorbeeld door te navigeren naar het via een webbrowser) ophalen gerouteerd naar het juiste eindpunt als bepaald door het routeringstype. In dit geval alle aanvragen worden doorgestuurd naar het eerste eindpunt en als Traffic Manager wordt gedetecteerd, worden niet in orde, wordt het verkeer automatisch failover-schakeling naar het eindpunt van de volgende.
11. Zodra uw Traffic Manager-profiel werkt, bewerkt u de DNS-record op de gezaghebbende DNS-server de naam van uw bedrijf domein verwijzen naar de naam van het Traffic Manager-domein.

![Prioriteit routeringsmethode voor verkeer met Traffic Manager configureren][1]

## <a name="next-steps"></a>Volgende stappen


- Meer informatie over [gewogen verkeersrouteringsmethode](traffic-manager-configure-weighted-routing-method.md).
- Meer informatie over [routeringsmethode voor prestaties](traffic-manager-configure-performance-routing-method.md).
- Meer informatie over [geografische verkeersrouteringsmethode](traffic-manager-configure-geographic-routing-method.md).
- Meer informatie over het [Traffic Manager-instellingen testen](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-priority-routing-method/traffic-manager-priority-routing-method.png