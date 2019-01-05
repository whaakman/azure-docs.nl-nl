---
title: Azure Traffic Manager-instellingen controleren
description: In dit artikel helpt u uw Traffic Manager-instellingen verifiëren.
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
ms.openlocfilehash: b37022f79feafc2110366446752675e7c42188f3
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54054021"
---
# <a name="verify-traffic-manager-settings"></a>Traffic Manager-instellingen controleren

Als u wilt testen van uw Traffic Manager-instellingen, moet u meerdere clients hebben op verschillende locaties, van waaruit u uw tests kunt uitvoeren. Breng vervolgens de eindpunten in uw Traffic Manager-profiel een op een tijdstip.

* De DNS TTL-waarde laag ingesteld zodat de wijzigingen doorgeven snel (bijvoorbeeld 30 seconden).
* IP-adressen van uw Azure-cloudservices en websites in het profiel dat u wilt testen op de hoogte.
* Gebruik hulpprogramma's waarmee u een DNS-naam worden omgezet in een IP-adres en weer te geven dat adres.

Als u wilt zien dat de DNS-namen naar IP-adressen van de eindpunten in uw profiel omzetten dat u controleert. De naam moeten worden omgezet in een manier die overeenstemt met de verkeersrouteringsmethode gedefinieerd in de Traffic Manager-profiel. U kunt de hulpprogramma's zoals **nslookup** of **graven** DNS-namen worden omgezet.

De volgende voorbeelden kunnen u testen van uw Traffic Manager-profiel.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Controleren van Traffic Manager-profiel met behulp van nslookup en IP-configuratie in Windows

1. Open een opdracht of een Windows PowerShell-prompt als beheerder.
2. Type `ipconfig /flushdns` leegmaken van de cache van de DNS-resolver.
3. Typ `nslookup <your Traffic Manager domain name>`. Bijvoorbeeld de volgende opdracht wordt gecontroleerd met de naam van het domein met het voorvoegsel *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Een typische resultaat bevat de volgende informatie:

    + De DNS-naam en IP-adres van de DNS-server om op te lossen dit Traffic Manager-domeinnaam die wordt benaderd.
    + De naam van het Traffic Manager-domein u hebt getypt op de opdrachtregel na 'nslookup' en het IP-adres waarop de Traffic Manager-domein wordt omgezet. Het tweede IP-adres is het belangrijk om te controleren. Dit moet overeenkomen met een openbare virtuele IP-adres (VIP) voor een van de cloudservices of websites in het Traffic Manager-profiel dat u wilt testen.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>De verkeersrouteringsmethode failover testen

1. Laat alle eindpunten van.
2. DNS-omzetting voor de domeinnaam van uw bedrijf met behulp van nslookup of een vergelijkbaar hulpprogramma met behulp van één client, worden aanvragen.
3. Zorg ervoor dat het opgelost IP-adres overeenkomt met het primaire eindpunt.
4. Uw primaire eindpunt uitvallen of verwijder het controle-bestand zodat Traffic Manager denkt dat de toepassing is niet actief is.
5. Wacht tot de DNS-Time-to-Live (TTL) van Traffic Manager-profiel, plus een nog twee minuten. Bijvoorbeeld, als uw DNS TTL is 300 seconden (5 minuten), moet u wachten zeven minuten.
6. De DNS-client cache en aanvraag DNS-omzetting met behulp van nslookup leegmaken. In Windows, kunt u uw DNS-cache met de opdracht ipconfig/flushdns leegmaken.
7. Zorg ervoor dat het opgelost IP-adres overeenkomt met het secundaire eindpunt.
8. Herhaal dit proces, elk eindpunt op zijn beurt wordt gedeactiveerd. Controleer of dat de DNS-server het IP-adres van het eindpunt van de volgende in de lijst retourneert. Wanneer alle eindpunten niet beschikbaar zijn, moet u het IP-adres van het primaire eindpunt opnieuw te verkrijgen.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>De gewogen verkeersrouteringsmethode testen

1. Laat alle eindpunten van.
2. DNS-omzetting voor de domeinnaam van uw bedrijf met behulp van nslookup of een vergelijkbaar hulpprogramma met behulp van één client, worden aanvragen.
3. Zorg ervoor dat het opgelost IP-adres overeenkomt met een van de eindpunten.
4. De DNS client-cache leegmaken en Herhaal stappen 2 en 3 voor elk eindpunt. Hier ziet u verschillende IP-adressen die worden geretourneerd voor elk van de eindpunten.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>De methode voor verkeersroutering voor prestaties testen

Als u wilt effectief testen een methode voor verkeersroutering voor prestaties, moet u clients zich in verschillende onderdelen van de hele wereld. U kunt clients in verschillende Azure-regio's die kunnen worden gebruikt voor het testen van uw services maken. Als u een wereldwijd netwerk hebt, kunt u extern aanmelden bij clients in andere onderdelen van de hele wereld en uw tests van daaruit uitvoeren.

U kunt ook er zijn gratis, webgebaseerde DNS-zoekactie en graven services die beschikbaar zijn. Sommige van deze hulpprogramma's bieden u de mogelijkheid om te controleren of de DNS-naamomzetting vanaf verschillende locaties over de hele wereld. Voer een zoekopdracht op 'DNS-zoekactie' voor meer voorbeelden. De services van derden, zoals Gomez of Keynote kunnen worden gebruikt om te bevestigen dat de profielen van uw verkeer distribueert naar verwachting.

## <a name="next-steps"></a>Volgende stappen

* [Over verkeersrouteringsmethoden voor Traffic Manager](traffic-manager-routing-methods.md)
* [Prestatieoverwegingen voor Traffic Manager](traffic-manager-performance-considerations.md)
* [Problemen met Traffic Manager in gedegradeerde status oplossen](traffic-manager-troubleshooting-degraded.md)
