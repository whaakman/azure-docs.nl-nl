---
title: Controleer de instellingen voor Azure Traffic Manager | Microsoft Docs
description: Dit artikel helpt u de instellingen van uw Traffic Manager controleren
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 2180b640-596e-4fb2-be59-23a38d606d12
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: aadff1806a7cb22347283143563467366e857569
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="verify-traffic-manager-settings"></a>Controleer de instellingen voor het Traffic Manager

Als u wilt testen van uw Traffic Manager-instellingen, moet u meerdere clients hebt, op verschillende locaties, van waaruit u kunt uw tests uitvoeren. Breng vervolgens de eindpunten in uw Traffic Manager-profiel beneden één tegelijk.

* De DNS-TTL-waarde laag ingesteld zodat wijzigingen die zijn doorgegeven snel (bijvoorbeeld 30 seconden).
* De IP-adressen van uw Azure-cloudservices en websites in het profiel dat u wilt testen weten.
* Gebruik hulpprogramma's waarmee u een DNS-naam omzetten in een IP-adres en weer te geven dat adres.

Om te zien dat de DNS-namen naar IP-adressen van de eindpunten in uw profiel omzetten dat u controleert. De namen moeten worden omgezet in samenhang met de verkeersrouteringsmethode gedefinieerd in de Traffic Manager-profiel. U kunt de hulpprogramma's zoals **nslookup** of **verdiepen** DNS-namen omzetten.

De volgende voorbeelden kunnen u testen van uw Traffic Manager-profiel.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Traffic Manager-profiel met nslookup en ipconfig in Windows controleren

1. Open een opdracht of een Windows PowerShell-prompt als beheerder.
2. Type `ipconfig /flushdns` leegmaken van de cache van de DNS-resolver.
3. Typ `nslookup <your Traffic Manager domain name>`. De volgende opdracht controleert bijvoorbeeld de naam van het domein met het voorvoegsel *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Een typische resultaat ziet u de volgende informatie:

    + De DNS-naam en IP-adres van de DNS-server om op te lossen dit Traffic Manager-domeinnaam wordt geopend.
    + De naam van het Traffic Manager-domein u hebt getypt op de opdrachtregel na 'nslookup' en het IP-adres waarnaar het Traffic Manager-domein wordt omgezet. Het tweede IP-adres is het belangrijk om te controleren. Deze moet overeenkomen met een openbare virtuele IP-adres (VIP)-adres voor een van de cloud-services of websites in het Traffic Manager-profiel dat u wilt testen.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>De verkeersrouteringsmethode failover testen

1. Laat alle eindpunten.
2. Met behulp van één client, DNS-omzetting voor de domeinnaam van uw bedrijf met nslookup of een vergelijkbaar hulpprogramma aanvragen.
3. Zorg ervoor dat het omgezette IP-adres overeenkomt met het primaire eindpunt.
4. Buiten uw primaire eindpunt of verwijderen van het controle-bestand, zodat die Traffic Manager denkt dat de toepassing niet actief is.
5. Wacht tot de DNS-Time-to-Live (TTL) van Traffic Manager-profiel plus nog twee minuten zijn verstreken. Bijvoorbeeld, als uw DNS-TTL is 300 seconden (5 minuten), moet u wachten zeven minuten.
6. Uw DNS-client cache en aanvraag DNS-omzetting met nslookup leegmaken. In Windows kunt u uw DNS-cache met de opdracht ipconfig/flushdns leegmaken.
7. Zorg ervoor dat het omgezette IP-adres overeenkomt met uw secundaire eindpunt.
8. Herhaal dit proces, op zijn beurt omlaag elk eindpunt te brengen. Controleer of dat de DNS-server het IP-adres van het volgende eindpunt in de lijst retourneert. Wanneer alle eindpunten niet beschikbaar zijn, moet u het IP-adres van het primaire eindpunt opnieuw verkrijgen.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>De gewogen verkeersrouteringsmethode testen

1. Laat alle eindpunten.
2. Met behulp van één client, DNS-omzetting voor de domeinnaam van uw bedrijf met nslookup of een vergelijkbaar hulpprogramma aanvragen.
3. Zorg ervoor dat het omgezette IP-adres overeenkomt met een van de eindpunten.
4. Uw DNS-client-cache leegmaken en Herhaal stappen 2 en 3 voor elk eindpunt. Hier ziet u verschillende IP-adressen voor elk van uw eindpunten worden geretourneerd.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>Het testen van de verkeersrouteringsmethode voor prestaties

Als u wilt testen effectief prestaties verkeersrouteringsmethode, moet u de clients zich in verschillende onderdelen van de hele wereld hebben. U kunt clients in verschillende Azure-regio's die kunnen worden gebruikt voor het testen van uw services maken. Als u een wereldwijd netwerk hebt, kunt u op afstand op clients in andere onderdelen van de hele wereld aanmelden en uw tests van daaruit uitvoeren.

U kunt ook er zijn gratis, webgebaseerde DNS-zoekopdracht en kennis van de services die beschikbaar zijn. Sommige van deze hulpprogramma's bieden u de mogelijkheid om te controleren van de DNS-naamomzetting vanaf verschillende locaties over de hele wereld. Voer een zoekopdracht op 'DNS-zoekopdracht' voor voorbeelden. De services van derden zoals Gomez of Keynote kunnen worden gebruikt om te bevestigen dat de profielen van uw verkeer distribueert zoals verwacht.

## <a name="next-steps"></a>Volgende stappen

* [Over verkeersrouteringsmethoden voor Traffic Manager](traffic-manager-routing-methods.md)
* [Prestatieoverwegingen voor Traffic Manager](traffic-manager-performance-considerations.md)
* [Problemen met Traffic Manager in gedegradeerde status oplossen](traffic-manager-troubleshooting-degraded.md)
