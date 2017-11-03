---
title: Configureer prestaties verkeersrouteringsmethode met behulp van Azure Traffic Manager | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u Traffic Manager configureren om verkeer te leiden naar het eindpunt met de laagste latentie
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
ms.openlocfilehash: 014aa646459cd64fca7c697419324caa3edaeeea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-the-performance-traffic-routing-method"></a>De verkeersrouteringsmethode voor prestaties configureren

De verkeersrouteringsmethode prestaties kunt u verkeer omleiden naar het eindpunt met de laagste latentie vanaf het netwerk van de client. Het datacenter met de laagste latentie is doorgaans het dichtst in geografische afstand. Deze methode voor het doorsturen van verkeer kan geen account voor realtime-wijzigingen in de netwerkconfiguratie of laden.

##  <a name="to-configure-performance-routing-method"></a>Routeringsmethode voor prestaties configureren

1. Meld u vanuit een browser aan bij [Azure Portal](http://portal.azure.com). Als u nog geen account hebt, kunt u zich registreren voor een [gratis proefversie van één maand](https://azure.microsoft.com/free/). 
2. Zoek in de portal zoekbalk, op de **Traffic Manager-profielen** en klik vervolgens op de naam van het profiel dat u wilt de routeringsmethode voor configureren.
3. In de **Traffic Manager-profiel** blade, Controleer of de cloudservices en de websites die u wilt opnemen in uw configuratie aanwezig zijn.
4. In de **instellingen** sectie, klikt u op **configuratie**, en in de **configuratie** blade voltooid zijn als volgt:
    1. Voor **verkeer van de instellingen voor verkeersroutering methode**, voor **routeringsmethode** Selecteer **prestaties**.
    2. Stel de **monitor eindpuntinstellingen** identiek voor alle elk eindpunt binnen dit profiel als volgt:
        1. Selecteer de relevante **Protocol**, en geef de **poort** getal. 
        2. Voor **pad** Typ een slash  */* . U moet een pad en bestandsnaam opgeven voor het bewaken van eindpunten. Een schuine streep naar voren '/' is een geldige invoer voor het relatieve pad en impliceert dat het bestand zich in de hoofdmap (standaard).
        3. Klik boven aan de pagina op **opslaan**.
5.  De wijzigingen in uw configuratie als volgt testen:
    1.  In de portal zoekbalk, zoek de naam van het Traffic Manager-profiel en klik op het Traffic Manager-profiel in de resultaten die de weergegeven.
    2.  In de **Traffic Manager** blade profiel, klikt u op **overzicht**.
    3.  De **Traffic Manager-profiel** blade bevat de DNS-naam van de zojuist gemaakte Traffic Manager-profiel. Dit kan worden gebruikt door clients (bijvoorbeeld door te navigeren naar het met een webbrowser) te verkrijgen gerouteerd naar het juiste eindpunt als bepaald door het type routering. In dit geval alle aanvragen worden doorgestuurd naar het eindpunt met de laagste latentie vanaf het netwerk van de client.
6. Zodra uw Traffic Manager-profiel werkt, bewerkt u de DNS-record op de gezaghebbende DNS-server de naam van uw bedrijf domein verwijzen naar de naam van het Traffic Manager-domein.

![Prestaties verkeersrouteringsmethode met Traffic Manager configureren][1]

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [verkeersrouteringsmethode gewogen](traffic-manager-configure-weighted-routing-method.md).
- Meer informatie over [routeringsmethode voor prioriteit](traffic-manager-configure-priority-routing-method.md).
- Meer informatie over [routeringsmethode voor geografische](traffic-manager-configure-geographic-routing-method.md).
- Meer informatie over hoe [Traffic Manager-instellingen testen](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png