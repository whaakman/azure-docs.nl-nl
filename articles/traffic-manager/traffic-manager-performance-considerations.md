---
title: Prestatie-overwegingen voor Azure Traffic Manager | Microsoft Docs
description: Inzicht in prestaties van Traffic Manager en het testen van prestaties van uw website bij het gebruik van Traffic Manager
services: traffic-manager
documentationcenter: ''
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: 454d06912c34a07b0b2617936769888e92adaba1
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57453657"
---
# <a name="performance-considerations-for-traffic-manager"></a>Prestatieoverwegingen voor Traffic Manager

Deze pagina leest u Prestatieoverwegingen met Traffic Manager. Houd rekening met het volgende scenario:

Er exemplaren van uw website in de regio's VS West en Oost-Aziatische. Een van de exemplaren is de statuscontrole voor de traffic manager-test mislukt. Toepassingsverkeer wordt omgeleid naar de regio in orde. Deze failover wordt verwacht maar prestaties kan een probleem opgetreden bij het op basis van de latentie van het verkeer nu onderweg naar een verafgelegen regio.

## <a name="performance-considerations-for-traffic-manager"></a>Prestatieoverwegingen voor Traffic Manager

De enige invloed op de prestaties die Traffic Manager op uw website hebben kan is de eerste DNS-zoekactie. Een DNS-aanvraag voor de naam van uw Traffic Manager-profiel wordt verwerkt door de Microsoft DNS-hoofdserver die als host fungeert voor de zone trafficmanager.net. Traffic Manager wordt gevuld en regelmatig bijgewerkt, van het Microsoft root DNS-servers op basis van het Traffic Manager-beleid en de resultaten van de test. Dus zelfs tijdens de eerste DNS-zoekactie, worden er geen DNS-query's op Traffic Manager verzonden.

Traffic Manager bestaat uit verschillende onderdelen: DNS-naam-servers, een API-service, de storage-laag en een eindpunt monitoring-service. Als een onderdeel van de service Traffic Manager is mislukt, heeft dit geen effect op de DNS-naam die is gekoppeld aan uw Traffic Manager-profiel. De records in de Microsoft DNS-servers blijven ongewijzigd. Eindpuntbewaking en DNS-updates echter niet gebeuren. Traffic Manager is daarom niet kunnen werkt DNS-server om te verwijzen naar uw failoversite als uw primaire locatie uitvalt.

DNS-naamomzetting is snel en resultaten in de cache zijn opgeslagen. De snelheid van de eerste DNS-zoekactie is afhankelijk van de DNS-servers die de client wordt gebruikt voor naamomzetting. Een client kan normaal gesproken een DNS-zoekopdracht binnen ~ 50 ms te voltooien. De resultaten van de zoekopdracht in de cache opgeslagen voor de duur van de DNS-Time-to-live (TTL). De standaard-TTL voor Traffic Manager is 300 seconden.

Verkeer wordt via Traffic Manager niet uitgebreid. Nadat de DNS-zoekactie is voltooid, is de client een IP-adres voor een exemplaar van uw website. De client maakt rechtstreeks verbinding met dit adres en geeft niet via Traffic Manager. Het Traffic Manager-beleid dat u kiest heeft geen invloed op de DNS-prestaties. Echter, kan een prestatie-routeringsmethode negatieve invloed hebben op de toepassingservaring. Bijvoorbeeld, als uw beleid verkeer van Noord-Amerika naar een exemplaar die wordt gehost in Azië omleidingen, mogelijk de netwerklatentie voor deze sessies een prestatieprobleem.

## <a name="measuring-traffic-manager-performance"></a>Meten van prestaties van Traffic Manager

Er zijn diverse websites die u gebruiken kunt om te begrijpen van de prestaties en het gedrag van een Traffic Manager-profiel. Veel van deze sites zijn gratis, maar mogelijk beperkingen. Sommige sites bieden uitgebreide bewaking en rapportage voor een betaling te doen.

De hulpprogramma's voor deze sites meting DNS latenties en weergave van de opgelost IP-adressen voor clientlocaties over de hele wereld. De meeste van deze hulpprogramma's de DNS-resultaten niet opslaan in cache. Daarom geven de hulpprogramma's voor de volledige DNS-zoekactie telkens wanneer die een test wordt uitgevoerd. Wanneer u vanaf uw eigen client test, ondervindt u alleen de volledige DNS-lookup prestaties eenmaal opent gedurende de duur van de TTL-waarde.

## <a name="sample-tools-to-measure-dns-performance"></a>Voorbeeld van hulpprogramma's voor het meten van DNS-prestaties

* [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS biedt veel hulpprogramma's voor prestaties. De vergelijking van de DNS-hulpprogramma kunt u weergeven, hoe lang het duurt om uw DNS-naam te herleiden en hoe die zich verhoudt tot andere DNS-serviceproviders.

* [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    Een van de meest eenvoudige hulpprogramma's is WebSitePulse. Voer de URL om te zien van de DNS-omzettingstijd, de eerste Byte, laatste Byte en andere statistieken over de prestaties. U kunt kiezen uit drie verschillende locaties. In dit voorbeeld ziet u dat de eerste uitvoering ziet u dat de DNS-zoekactie 0.204 sec duurt.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Omdat de resultaten worden opgeslagen in de cache, de tweede test voor hetzelfde Traffic Manager-eindpunt de DNS-zoekactie neemt 0.002 sec.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

* [CA-App synthetische Monitor](https://asm.ca.com/en/checkit.php)

    Voorheen bekend als het hulpprogramma voor controle van de muis selectievakje Website, deze site ziet u de DNS-omzettingstijd van meerdere geografische regio's tegelijkertijd. Voer de URL om te zien van de DNS-omzettingstijd, verbindingstijd en snelheid van verschillende geografische locaties. Gebruik deze test om te zien welke gehoste service wordt geretourneerd voor verschillende locaties over de hele wereld.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

* [Pingdom](http://tools.pingdom.com/)

    Dit hulpprogramma biedt statistieken over de prestaties voor elk element van een webpagina. Het tabblad analyse van de pagina bevat het percentage tijd besteed aan de DNS-zoekactie.

* [Wat is mijn DNS?](http://www.whatsmydns.net/)

    Deze site heeft een DNS-zoekopdracht vanuit 20 verschillende locaties en de resultaten worden weergegeven op een kaart.

* [Dig Web Interface](http://www.digwebinterface.com)

    Deze site bevat dat meer gedetailleerde DNS-informatie, met inbegrip van CNAME-records en A-records. Zorg ervoor dat u de 'Vullen met kleur uitvoer' en 'Statistieken' onder opties controleren, en selecteer 'All' onder naamservers.

## <a name="next-steps"></a>Volgende stappen

[Over verkeersrouteringsmethoden voor Traffic Manager](traffic-manager-routing-methods.md)

[Uw instellingen voor Traffic Manager testen](traffic-manager-testing-settings.md)

[Bewerkingen op Traffic Manager (REST API-referentiemateriaal)](https://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager-Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager)

