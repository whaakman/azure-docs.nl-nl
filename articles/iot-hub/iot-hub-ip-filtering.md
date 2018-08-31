---
title: Azure IoT Hub-IP-verbindingsfilters | Microsoft Docs
description: Het gebruik van IP-filtering om verbindingen te blokkeren van bepaalde IP-adressen voor uw Azure-IoT-hub. U kunt verbindingen van afzonderlijke of bereiken van IP-adressen blokkeren.
author: rezasherafat
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: rezas
ms.openlocfilehash: 864af9cae35912d95f2c0bf0b574a5ca2404a608
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190638"
---
# <a name="use-ip-filters"></a>IP-filters gebruiken

Beveiliging is een belangrijk aspect van een IoT-oplossing op basis van Azure IoT Hub. Soms moet u expliciet opgeven van de IP-adressen van waaruit de apparaten kunnen verbinding maken als onderdeel van uw beveiligingsconfiguratie. De _IP-filter_ functie kunt u regels configureren voor het weigeren van of verkeer van specifieke IPv4-adressen accepteert.

## <a name="when-to-use"></a>Wanneer gebruikt u dit?

Er zijn twee specifieke use cases als het is handig om te blokkeren van de IoT Hub-eindpunten voor bepaalde IP-adressen:

- Uw IoT-hub moet verkeer ontvangen van een opgegeven bereik van IP-adressen alleen en alle andere afwijzen. Bijvoorbeeld, gebruikt u uw IoT-hub met [Azure Express Route] particuliere verbindingen maken tussen een IoT-hub en uw on-premises infrastructuur.
- U moet verkeer van IP-adressen die zijn geïdentificeerd als verdacht door de beheerder van de IoT-hub afwijzen.

## <a name="how-filter-rules-are-applied"></a>Hoe regels worden toegepast

De IP-filterregels worden toegepast op het niveau van de IoT Hub-service. Daarom de IP-filterregels van toepassing op alle verbindingen vanaf apparaten en back-end-apps met behulp van een ondersteund protocol.

Elke verbindingspoging vanaf een IP-adres dat overeenkomt met een rejecting IP-regel in uw IoT-hub ontvangt een niet-geautoriseerde 401-statuscode en de beschrijving. Het antwoordbericht wordt niet vermeld voor de IP-regel.

## <a name="default-setting"></a>Standaardinstelling

Standaard de **IP-Filter** raster in de portal voor een IoT-hub is leeg. Deze instelling betekent dat uw hub verbindingen elk IP-adres aanvaardt. Deze instelling is gelijk aan een regel waarmee het 0.0.0.0/0 IP-adresbereik accepteert.

![IoT Hub-standaard IP-filter-instellingen][img-ip-filter-default]

## <a name="add-or-edit-an-ip-filter-rule"></a>Een IP-filter-regel toevoegen of bewerken

Wanneer u een IP-filter rule toevoegt, wordt u gevraagd de volgende waarden op te geven:

- Een **IP-filter regelnaam** die moet een unieke, niet-hoofdlettergevoelig, alfanumerieke tekenreeks maximaal 128 tekens lang zijn. Alleen de ASCII-7-bits alfanumerieke tekens plus `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` worden geaccepteerd.
- Selecteer een **afwijzen** of **accepteren** als de **actie** voor de IP-filter rule.
- Geef één IPv4-adres of een blok IP-adressen in CIDR-notatie. Bijvoorbeeld, in CIDR vertegenwoordigt notatie 192.168.100.0/22 de 1024 IPv4-adressen van 192.168.100.0 tot 192.168.103.255 aangeeft.

![Een regel voor IP-filter toevoegen aan een IoT-hub][img-ip-filter-add-rule]

Nadat u de regel niet opslaan, ziet u een waarschuwing dat de update uitgevoerd wordt.

![Melding over het opslaan van een IP-filter rule][img-ip-filter-save-new-rule]

De **toevoegen** optie wordt uitgeschakeld wanneer u het maximum van 10 filterregels voor de IP-bereiken.

U kunt een bestaande regel bewerken door te dubbelklikken op de rij die de regel bevat.

> [!NOTE]
> Weigeren IP adressen kunnen voorkomen dat andere Azure-Services (zoals Azure Stream Analytics, Azure Virtual Machines of het Device Explorer in de portal) interactie met de IoT-hub.

> [!WARNING]
> Als u Azure Stream Analytics (ASA) gebruikt om berichten te lezen van een IoT-hub met IP-filtering is ingeschakeld, gebruikt u de Event Hub-compatibele naam en het eindpunt van uw IoT-Hub in de ASA-verbindingsreeks.

## <a name="delete-an-ip-filter-rule"></a>Een regel voor IP-filter verwijderen

Een IP-filter als regel wilt verwijderen, selecteert u een of meer regels in het raster en klikt u op **verwijderen**.

![Een regel voor IoT Hub-IP-filter verwijderen][img-ip-filter-delete-rule]

## <a name="ip-filter-rule-evaluation"></a>Evaluatie van IP-filter

IP-filterregels worden toegepast in volgorde en de eerste regel die overeenkomt met het IP-adres bepaalt de actie accepteren of weigeren.

Als u adressen in het bereik 192.168.100.0/22 accepteren en weigeren alle andere wilt, moet de eerste regel in het raster bijvoorbeeld, het adresbereik 192.168.100.0/22 accepteren. De volgende regel moet alle adressen weigeren met behulp van het adresbereik 0.0.0.0/0.

U kunt de volgorde van de regels van uw IP-filter in het raster wijzigen door de drie verticale puntjes aan het begin van een rij te klikken en slepen en neerzetten.

Om uw nieuwe regelvolgorde van de IP-filter hebt opgeslagen, klikt u op **opslaan**.

![De volgorde van uw IoT Hub-IP-filterregels wijzigen][img-ip-filter-rule-order]

## <a name="next-steps"></a>Volgende stappen

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Bewerkingen controleren][lnk-monitor]
- [Metrische gegevens van IoT Hub][lnk-metrics]

<!-- Images -->
[img-ip-filter-default]: ./media/iot-hub-ip-filtering/ip-filter-default.png
[img-ip-filter-add-rule]: ./media/iot-hub-ip-filtering/ip-filter-add-rule.png
[img-ip-filter-save-new-rule]: ./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png
[img-ip-filter-delete-rule]: ./media/iot-hub-ip-filtering/ip-filter-delete-rule.png
[img-ip-filter-rule-order]: ./media/iot-hub-ip-filtering/ip-filter-rule-order.png


<!-- Links -->

[IoT Hub developer guide]: iot-hub-devguide.md
[Azure Express Route]:  https://azure.microsoft.com/documentation/articles/expressroute-faqs/#supported-services

[lnk-monitor]: iot-hub-operations-monitoring.md
[lnk-metrics]: iot-hub-metrics.md
