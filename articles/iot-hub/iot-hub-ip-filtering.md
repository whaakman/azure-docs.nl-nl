---
title: Azure IoT Hub IP-verbindingsfilters | Microsoft Docs
description: Het gebruik van IP-filtering om verbindingen te blokkeren van specifieke IP-adressen voor uw Azure-IoT-hub. U kunt de verbindingen van afzonderlijke of bereiken van IP-adressen blokkeren.
services: iot-hub
documentationcenter: 
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: f833eac3-5b5f-46a7-a47b-f4f6fc927f3f
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: boltean
ms.openlocfilehash: 85f5f044faddd5180f0c19d3f2c235b20f6373d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="use-ip-filters"></a>IP-filters gebruiken

Beveiliging is een belangrijk aspect van een IoT-oplossing op basis van Azure IoT Hub. Soms moet u expliciet opgeven van de IP-adressen waaruit apparaten verbinding maken als onderdeel van uw beveiligingsconfiguratie. De _IP-filter_ functie kunt u regels voor weigeren of accepteren van verkeer van specifieke IPv4-adressen te configureren.

## <a name="when-to-use"></a>Wanneer gebruikt u dit?

Er zijn twee specifieke gebruiksvoorbeelden wanneer dit is handig om te blokkeren van de eindpunten van IoT Hub voor bepaalde IP-adressen:

- Uw IoT-hub moet ontvangen verkeer alleen via een opgegeven IP-adressen en alle andere afwijzen. Bijvoorbeeld, het gebruik van uw IoT-hub met [Azure Express Route] naar particuliere verbindingen maken tussen een IoT-hub en uw on-premises infrastructuur.
- U moet verkeer van IP-adressen die als verdacht zijn geïdentificeerd door de beheerder van de IoT-hub afwijzen.

## <a name="how-filter-rules-are-applied"></a>Hoe filterregels worden toegepast

De IP-filter-regels worden toegepast op het niveau van de service IoT Hub. Daarom de IP-filterregels van toepassing op alle verbindingen vanaf apparaten en back-end-apps met behulp van een ondersteund protocol.

Elke verbindingspoging van een IP-adres dat overeenkomt met een rejecting IP-regel in uw IoT-hub ontvangt een niet-geautoriseerde 401-statuscode en de beschrijving. De IP-regel niet wordt vermeld in het antwoordbericht.

## <a name="default-setting"></a>Standaardinstelling

Standaard de **IP-Filter** raster in de portal voor een IoT-hub is leeg. Deze instelling betekent dat uw hub verbindingen elk IP-adres aanvaardt. Deze instelling komt overeen met een regel die de 0.0.0.0/0 IP-adresbereik accepteert.

![IoT Hub standaard IP-filter-instellingen][img-ip-filter-default]

## <a name="add-or-edit-an-ip-filter-rule"></a>De regel van een IP-filter toevoegen of bewerken

Wanneer u een IP-filterregel toevoegt, wordt u gevraagd de volgende waarden op te geven:

- Een **IP-filter regelnaam** die moet een unieke, niet-hoofdlettergevoelige alfanumerieke tekenreeks van maximaal 128 tekens lang zijn. Alleen de ASCII-7-bits alfanumerieke tekens plus `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` worden geaccepteerd.
- Selecteer een **afwijzen** of **accepteren** als de **actie** voor de IP-filter-regel.
- Geef één IPv4-adres of een blok IP-adressen in CIDR-notatie. Bijvoorbeeld, in CIDR vertegenwoordigt notatie 192.168.100.0/22 de 1024 IPv4-adressen van 192.168.100.0 tot 192.168.103.255.

![Een IP-filter-regel toevoegen aan een IoT-hub][img-ip-filter-add-rule]

Nadat u de regel opslaat, ziet u een waarschuwing dat de update uitgevoerd wordt.

![Meldingen over het opslaan van een regel voor IP-filter][img-ip-filter-save-new-rule]

De **toevoegen** optie wordt uitgeschakeld wanneer u het maximum van 10 filterregels voor de IP-bereiken.

U kunt een bestaande regel bewerken door te dubbelklikken op de rij waarin de regel.

> [!NOTE]
> Weigeren IP adressen kunnen voorkomen dat andere Azure-Services (zoals Azure Stream Analytics, Azure Virtual Machines of de Explorer-apparaat in de portal) interactie met de IoT-hub.

> [!WARNING]
> Als u Azure Stream Analytics (ASA) gebruikt om berichten te lezen van een iothub met IP-filtering is ingeschakeld, gebruikt u de Event Hub-compatibele naam en het eindpunt van uw IoT-Hub in de ASA-verbindingsreeks.

## <a name="delete-an-ip-filter-rule"></a>Een IP-filter-regel verwijderen

Een IP-filter als regel wilt verwijderen, selecteert u een of meer regels in het raster en klik op **verwijderen**.

![Een IoT Hub IP-filter-regel verwijderen][img-ip-filter-delete-rule]

## <a name="ip-filter-rule-evaluation"></a>IP-filter Regeltoepassing

IP-filterregels in volgorde worden toegepast en de eerste regel die overeenkomt met het IP-adres bepaalt de actie accepteren of weigeren.

Als u wilt adressen in het bereik 192.168.100.0/22 accepteren en weigeren alle andere, moet de eerste regel in het raster het adresbereik 192.168.100.0/22 accepteren. De volgende regel moet alle adressen weigeren met behulp van de 0.0.0.0/0 bereik.

U kunt de volgorde van de regels van uw IP-filter in het raster wijzigen door de drie verticale punten aan het begin van een rij te klikken en slepen en neerzetten.

Klik voor het opslaan van uw nieuwe IP-filter regel order **opslaan**.

![De volgorde van uw IoT Hub IP-filterregels wijzigen][img-ip-filter-rule-order]

## <a name="next-steps"></a>Volgende stappen

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Bewerkingen controleren][lnk-monitor]
- [IoT Hub metrische gegevens][lnk-metrics]

<!-- Images -->
[img-ip-filter-default]: ./media/iot-hub-ip-filtering/ip-filter-default.png
[img-ip-filter-add-rule]: ./media/iot-hub-ip-filtering/ip-filter-add-rule.png
[img-ip-filter-save-new-rule]: ./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png
[img-ip-filter-delete-rule]: ./media/iot-hub-ip-filtering/ip-filter-delete-rule.png
[img-ip-filter-rule-order]: ./media/iot-hub-ip-filtering/ip-filter-rule-order.png


<!-- Links -->

[IoT Hub developer guide]: iot-hub-devguide.md
[Azure Express Route]:  https://azure.microsoft.com/en-us/documentation/articles/expressroute-faqs/#supported-services

[lnk-monitor]: iot-hub-operations-monitoring.md
[lnk-metrics]: iot-hub-metrics.md