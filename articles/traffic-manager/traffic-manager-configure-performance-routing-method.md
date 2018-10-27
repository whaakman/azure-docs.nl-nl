---
title: Prestaties traffic routeringsmethode configureren met behulp van Azure Traffic Manager | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u Traffic Manager configureren om verkeer te routeren naar het eindpunt met de laagste latentie
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
ms.openlocfilehash: c378043a9e10a0aed5344ac3182af6163d217c7b
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140465"
---
# <a name="configure-the-performance-traffic-routing-method"></a>De methode voor verkeersroutering voor prestaties configureren

De methode voor verkeersroutering voor prestaties kunt u instellen dat verkeer naar het eindpunt met de laagste latentie vanaf het netwerk van de client. Het datacenter met de laagste latentie is meestal het dichtst in de geografische afstand. Deze methode voor verkeersroutering kan rekening gehouden met realtime wijzigingen in de netwerkconfiguratie of laden.

##  <a name="to-configure-performance-routing-method"></a>Routeringsmethode voor prestaties configureren

1. Meld u vanuit een browser aan bij [Azure Portal](http://portal.azure.com). Als u nog geen account hebt, kunt u zich registreren voor een [gratis proefversie van één maand](https://azure.microsoft.com/free/). 
2. Zoek in de zoekbalk van de portal, de **Traffic Manager-profielen** en klik vervolgens op de naam van het profiel dat u wilt de routeringsmethode voor configureren.
3. In de **Traffic Manager-profiel** blade controleren of de cloudservices en de websites die u wilt opnemen in uw configuratie weergegeven worden.
4. In de **instellingen** sectie, klikt u op **configuratie**, en klik in de **configuratie** blade voltooid zijn als volgt te werk:
    1. Voor **traffic routing-methode-instellingen**, voor **routeringsmethode** Selecteer **prestaties**.
    2. Stel de **monitor eindpuntinstellingen** identiek voor alle elk eindpunt binnen dit profiel als volgt te werk:
        1. Selecteer de juiste **Protocol**, en geef de **poort** getal. 
        2. Voor **pad** typt u een slash */*. Voor het controleren van eindpunten, moet u een pad en bestandsnaam opgeven. Een schuine streep naar voren '/' is een geldige vermelding voor het relatieve pad en geeft aan dat het bestand is in de hoofdmap (standaard).
        3. Aan de bovenkant van de pagina, klikt u op **opslaan**.
5.  De wijzigingen in uw configuratie als volgt testen:
    1.  In de zoekbalk van de portal, zoek de naam van het Traffic Manager-profiel en klik op het Traffic Manager-profiel in de resultaten die de weergegeven.
    2.  In de **Traffic Manager** blade profiel, klikt u op **overzicht**.
    3.  De **Traffic Manager-profiel** blade wordt weergegeven voor de DNS-naam van uw zojuist gemaakte Traffic Manager-profiel. Dit kan worden gebruikt door clients (bijvoorbeeld door te navigeren naar het via een webbrowser) ophalen gerouteerd naar het juiste eindpunt als bepaald door het routeringstype. In dit geval alle aanvragen worden doorgestuurd naar het eindpunt met de laagste latentie vanaf het netwerk van de client.
6. Zodra uw Traffic Manager-profiel werkt, bewerkt u de DNS-record op de gezaghebbende DNS-server de naam van uw bedrijf domein verwijzen naar de naam van het Traffic Manager-domein.

![Prestaties van de methode voor verkeersroutering met Traffic Manager configureren][1]

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [gewogen verkeersrouteringsmethode](traffic-manager-configure-weighted-routing-method.md).
- Meer informatie over [de routeringsmethode met prioriteit](traffic-manager-configure-priority-routing-method.md).
- Meer informatie over [geografische verkeersrouteringsmethode](traffic-manager-configure-geographic-routing-method.md).
- Meer informatie over het [Traffic Manager-instellingen testen](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png