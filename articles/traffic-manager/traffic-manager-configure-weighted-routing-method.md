---
title: Routeringsmethode voor gewogen round robin-verkeer met behulp van Azure Traffic Manager configureren | Microsoft Docs
description: Dit artikel wordt uitgelegd hoe u taken te verdelen voor verkeer met een round robin-methode in Traffic Manager
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
ms.openlocfilehash: 7aa4c9120d44ff1b3e59a57090ea04e3f8021fc4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-weighted-traffic-routing-method-in-traffic-manager"></a>De gewogen verkeersrouteringsmethode in Traffic Manager configureren

Een patroon algemene traffic routing methode is het bieden van een reeks identieke eindpunten, waaronder cloudservices en websites, en verkeer worden verzonden naar elk round-robin toewijst. De volgende stappen geven een overzicht van het configureren van dit type verkeersrouteringsmethode.

> [!NOTE]
> Azure Websites bieden al round robin-functionaliteit voor websites binnen een datacenter (ook wel bekend als een regio) voor taakverdeling. Traffic Manager kunt u de routeringsmethode voor round robin-verkeer voor websites opgeven in verschillende datacenters.

## <a name="to-configure-the-weighted-traffic-routing-method"></a>De gewogen verkeersrouteringsmethode configureren

1. Meld u vanuit een browser aan bij [Azure Portal](http://portal.azure.com). Als u nog geen account hebt, kunt u zich registreren voor een [gratis proefversie van één maand](https://azure.microsoft.com/free/). 
2. Zoek in de portal zoekbalk, op de **Traffic Manager-profielen** en klik vervolgens op de naam van het profiel dat u wilt de routeringsmethode voor configureren.
3. In de **Traffic Manager-profiel** blade, Controleer of de cloudservices en de websites die u wilt opnemen in uw configuratie aanwezig zijn.
4. In de **instellingen** sectie, klikt u op **configuratie**, en in de **configuratie** blade voltooid zijn als volgt:
    1. Voor **verkeer van de instellingen voor verkeersroutering methode**, Controleer of de verkeersrouteringsmethode **gewogen**. Als dit niet het geval is, klikt u op **gewogen** uit de vervolgkeuzelijst.
    2. Stel de **monitor eindpuntinstellingen** identiek voor alle elk eindpunt binnen dit profiel als volgt:
        1. Selecteer de relevante **Protocol**, en geef de **poort** getal. 
        2. Voor **pad** Typ een slash  */* . U moet een pad en bestandsnaam opgeven voor het bewaken van eindpunten. Een schuine streep naar voren '/' is een geldige invoer voor het relatieve pad en impliceert dat het bestand zich in de hoofdmap (standaard).
        3. Klik boven aan de pagina op **opslaan**.
5. De wijzigingen in uw configuratie als volgt testen:
    1.  In de portal zoekbalk, zoek de naam van het Traffic Manager-profiel en klik op het Traffic Manager-profiel in de resultaten die de weergegeven.
    2.  In de **Traffic Manager** blade profiel, klikt u op **overzicht**.
    3.  De **Traffic Manager-profiel** blade bevat de DNS-naam van de zojuist gemaakte Traffic Manager-profiel. Dit kan worden gebruikt door clients (bijvoorbeeld door te navigeren naar het met een webbrowser) te verkrijgen gerouteerd naar het juiste eindpunt als bepaald door het type routering. In dit geval alle aanvragen worden doorgestuurd elk eindpunt round-robin toewijst.
6. Zodra uw Traffic Manager-profiel werkt, bewerkt u de DNS-record op de gezaghebbende DNS-server de naam van uw bedrijf domein verwijzen naar de naam van het Traffic Manager-domein.

![Gewogen verkeersrouteringsmethode met Traffic Manager configureren][1]

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [prioriteit van verkeer routeringsmethode](traffic-manager-configure-priority-routing-method.md).
- Meer informatie over [prestaties methode voor het doorsturen van verkeer](traffic-manager-configure-performance-routing-method.md).
- Meer informatie over [routeringsmethode voor geografische](traffic-manager-configure-geographic-routing-method.md).
- Meer informatie over hoe [Traffic Manager-instellingen testen](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
