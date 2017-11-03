---
title: Prestatie-overwegingen voor Azure Traffic Manager | Microsoft Docs
description: Op de prestaties van Traffic Manager en het testen van de prestaties van uw website bij gebruik van Traffic Manager begrijpen
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 3ba5dfa1-2922-43f1-9a23-d06969c4a516
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: f686685138625a53971f1fc5fc754fd22c9d67b2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="performance-considerations-for-traffic-manager"></a>Prestatieoverwegingen voor Traffic Manager

Deze pagina wordt uitgelegd Prestatieoverwegingen met Traffic Manager. Houd rekening met het volgende scenario:

Hebt u exemplaren van uw website in de WestUS en Oost-Aziatische gebieden. Een of meer exemplaren van is de statuscontrole voor de traffic manager-test mislukt. Toepassing verkeer wordt omgeleid naar de orde regio. Deze failover wordt verwacht, maar prestaties op basis van de latentie van het verkeer naar een verafgelegen regio nu op reis problematisch kan zijn.

## <a name="performance-considerations-for-traffic-manager"></a>Prestatieoverwegingen voor Traffic Manager

De enige prestatie-invloed die Traffic Manager op uw website hebben kan is de eerste DNS-zoekopdracht. Een DNS-aanvraag voor de naam van uw Traffic Manager-profiel wordt verwerkt door de Microsoft DNS-hoofdserver die als host fungeert voor de zone trafficmanager.net. Traffic Manager gevuld en regelmatig bijgewerkt met de Microsoft DNS-hoofdmap servers op basis van het Traffic Manager-beleid en de resultaten van de test. Dus zelfs tijdens de eerste DNS-zoekactie, worden er geen DNS-query's aan Traffic Manager verzonden.

Traffic Manager bestaat uit verschillende onderdelen: DNS-naam servers, een API-service, de storage-laag en een eindpunt bewaking van de service. Als een serviceonderdeel Traffic Manager is mislukt, heeft dit geen effect op de DNS-naam die is gekoppeld aan uw Traffic Manager-profiel. De records in de Microsoft DNS-servers blijven ongewijzigd. Eindpuntcontrole en bijwerken van de DNS-echter niet gebeuren. Traffic Manager is daarom niet bijwerken van DNS om te verwijzen naar uw failoversite wanneer de primaire site uitvalt.

DNS-naamomzetting is snel en resultaten in de cache zijn opgeslagen. De snelheid van de eerste DNS-zoekopdracht is afhankelijk van de DNS-servers die de client voor naamomzetting gebruikt. Een client kan normaal gesproken een DNS-zoekopdracht binnen ~ 50 ms voltooid. De resultaten van de zoekactie voor de duur van de DNS-Time-to-live (TTL) in cache zijn opgeslagen. De standaard TTL voor Traffic Manager is 300 seconden.

Verkeer wordt niet uitgebreid via Traffic Manager. Zodra de DNS-zoekopdracht is voltooid, heeft de client een IP-adres voor een exemplaar van uw website. De client rechtstreeks verbinding maakt met dit adres en geeft niet via het Traffic Manager. Het Traffic Manager-beleid dat u kiest heeft geen invloed op de DNS-prestaties. Echter, een prestaties routering-methode kunt negatieve invloed hebben op de toepassingservaring. Bijvoorbeeld, als uw beleid verkeer van Noord-Amerika naar een exemplaar dat wordt gehost in Azië omleidingen, mogelijk de netwerklatentie voor deze sessies een prestatieprobleem.

## <a name="measuring-traffic-manager-performance"></a>Meten van de prestaties van Traffic Manager

Er zijn meerdere websites die kunt u de prestaties en het gedrag van een Traffic Manager-profiel te begrijpen. Veel van deze sites zijn gratis maar mogelijk beperkingen. Sommige sites bieden uitgebreide bewaking en rapportage voor een vast bedrag.

De hulpprogramma's op deze sites meting DNS latenties en weergave van de opgelost IP-adressen voor clientlocaties over de hele wereld. De DNS-resultaten niet in cache opslaan van de meeste van deze hulpprogramma's. Daarom tonen de hulpprogramma's voor de volledige DNS-zoekopdracht telkens wanneer die een test wordt uitgevoerd. Wanneer u vanaf uw eigen client test, ervaren u alleen de volledige DNS-lookup prestaties eenmaal gedurende de duur van de TTL.

## <a name="sample-tools-to-measure-dns-performance"></a>Voorbeeld-hulpprogramma's voor het meten van DNS-prestaties

* [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS biedt veel prestatiehulpprogramma's. Het hulpprogramma voor DNS-vergelijking wordt weergegeven hoe lang het duurt om uw DNS-naam te herleiden en hoe die zich verhoudt tot andere DNS-service-providers.

* [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    Een van de meest eenvoudige hulpprogramma's voor is WebSitePulse. Voer de URL om te zien van DNS-omzettingstijd, eerste Byte, laatste Byte en andere prestatiestatistieken weer. U kunt kiezen uit drie verschillende testlocaties. In dit voorbeeld ziet u dat de eerste uitvoering blijkt dat DNS-lookup 0.204 per seconde heeft.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Omdat de resultaten in cache zijn opgeslagen, de tweede test voor hetzelfde Traffic Manager-eindpunt de DNS-zoekopdracht neemt 0.002 per seconde.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

* [CA-App synthetische Monitor](https://asm.ca.com/en/checkit.php)

    Voorheen bekend als het hulpprogramma Watchmouse selectievakje Website, deze site ziet u de DNS-omzettingstijd uit meerdere geografische regio's tegelijk. Voer de URL om te zien van DNS-omzettingstijd, verbindingstijd en snelheid van verschillende geografische locaties. Deze test gebruiken om te zien welke gehoste service is geretourneerd voor verschillende locaties over de hele wereld.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

* [Pingdom](http://tools.pingdom.com/)

    Dit hulpprogramma biedt prestatiestatistieken weer voor elk element van een webpagina. Het tabblad pagina analyse toont het percentage tijd besteed aan de DNS-zoekactie.

* [Wat is mijn DNS?](http://www.whatsmydns.net/)

    Deze site heeft een DNS-zoekopdracht vanaf verschillende locaties 20 en de resultaten worden weergegeven op een kaart.

* [Kennis van Web-Interface](http://www.digwebinterface.com)

    Deze site geeft dat meer gedetailleerde DNS-informatie, met inbegrip van CNAMEs en A-records. Zorg ervoor dat u de 'Vullen met kleur uitvoer' en 'Statistieken' onder opties controleren en selecteert 'All' onder Nameservers.

## <a name="next-steps"></a>Volgende stappen

[Over verkeersrouteringsmethoden voor Traffic Manager](traffic-manager-routing-methods.md)

[Testen van uw Traffic Manager-instellingen](traffic-manager-testing-settings.md)

[Bewerkingen op Traffic Manager (REST API-referentiemateriaal)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager-Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=400769)

