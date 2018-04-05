---
title: Prijzen voor Azure IoT Hub begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - informatie over hoe softwarelicentiecontrole en prijzen van werkt met IoT Hub, inclusief voorbeelden gegaan.
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 1ac90923-1edf-4134-bbd4-77fee9b68d24
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 390e917990586642e55913a69873b1707e371de3
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="azure-iot-hub-pricing-information"></a>Azure IoT Hub prijsgegevens

[Azure IoT Hub prijzen] [ lnk-pricing] biedt de algemene informatie over de verschillende SKU's en prijzen voor IoT Hub. Dit artikel bevat aanvullende informatie over hoe de verschillende functies van de IoT Hub worden gemeten als berichten door de IoT-Hub.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="charges-per-operation"></a>Kosten per bewerking

| Bewerking | Factureringsgegevens | 
| --------- | ------------------- |
| Registerbewerkingen voor identiteit <br/> (maken, ophalen, weergeven, bijwerken en verwijderen) | Niet in rekening gebracht. |
| Apparaat-naar-cloud-berichten | Verzonden berichten in rekening worden gebracht in segmenten van 4 KB op inkomend in IoT Hub. Een bericht 6 KB is bijvoorbeeld in rekening gebracht 2 berichten. |
| Cloud-naar-apparaat-berichten | Verzonden berichten in segmenten van 4 KB in rekening worden gebracht, bijvoorbeeld een 6-KB-bericht 2 berichten in rekening wordt gebracht. |
| Uploaden van bestanden | Bestandsoverdracht naar Azure Storage wordt niet door de IoT Hub datalimiet. Bestand overdracht inleiding en het voltooien van berichten worden in rekening gebracht als mailberichten gemeten in intervallen van 4 KB. Bijvoorbeeld, het overbrengen van een bestand van 10 MB is in rekening gebracht twee berichten naast de kosten van Azure Storage. |
| Directe methoden | Geslaagde methodeaanvragen in segmenten van 4 KB in rekening worden gebracht, antwoorden met niet-lege instanties in rekening worden gebracht in de 4 KB-segmenten als aanvullende berichten. Aanvragen voor niet-verbonden apparaten worden in rekening gebracht als berichten in segmenten van 4 KB. Bijvoorbeeld, een methode met een 6-KB-instantie die in een reactie bij geen instantie van het apparaat resulteert, wordt in rekening gebracht als twee berichten. Een methode met een 6-KB-instantie die in een antwoord 1 KB van het apparaat resulteert wordt in rekening gebracht als twee berichten voor de aanvraag plus een ander bericht voor het antwoord. |
| Apparaatdubbel leest | Apparaat twin leest uit het apparaat en de oplossing terug end worden in rekening gebracht als berichten in een 512-byte-segmenten. Bijvoorbeeld, een twin 6 KB apparaat lezen wordt in rekening gebracht als 12 berichten. |
| Apparaat twin updates (tags en eigenschappen) | Apparaat twin updates van het apparaat en van de back-end van de oplossing in rekening worden gebracht als berichten in een 512-byte-segmenten. Bijvoorbeeld, een twin 6 KB apparaat lezen wordt in rekening gebracht als 12 berichten. |
| Apparaat twin query 's | Query's worden in rekening gebracht als berichten, afhankelijk van de grootte van het resultaat in 512-byte-segmenten. |
| Taakbewerkingen <br/> (maken, bijwerken, weergeven, verwijderen) | Niet in rekening gebracht. |
| Taken per apparaat bewerkingen | Bewerkingen (zoals twin apparaatupdates en methoden) taken in rekening worden gebracht die normaal werken. Een taak waardoor 1000 methodeaanroepen met 1 KB aanvragen en antwoorden lege hoofdcode is bijvoorbeeld in rekening gebracht 1000 berichten. |

> [!NOTE]
> Alle grootte wordt berekend met inbegrip van de nettolading-grootte in bytes (protocol framing genegeerd). Voor berichten die eigenschappen en hoofdtekst hebt, wordt de grootte op een manier protocol networkdirect berekend. Zie voor meer informatie [messaging Ontwikkelaarshandleiding voor IoT-Hub][lnk-message-size].

## <a name="example-1"></a># 1

Een apparaat verzendt één 1 KB apparaat-naar-cloud bericht per minuut naar IoT Hub, die vervolgens door Azure Stream Analytics wordt gelezen. De back-end oplossing roept een methode (met 512-byte-nettolading) op het apparaat om de 10 minuten voor het activeren van een specifieke actie. Het apparaat reageert op de methode met een resultaat van 200 bytes.

Het apparaat verbruikt:

* Een bericht * 60 minuten * 24 uur = 1440 berichten per dag voor de apparaat-naar-cloud-berichten.
* Twee aanvragen plus antwoord * 6 keer per uur * 24 uur = 288 berichten voor de methoden.

Deze berekening biedt een totaal van 1728 berichten per dag.

## <a name="example-2"></a># 2

Een apparaat verzendt één 100 KB apparaat-naar-cloud bericht elk uur. Werkt het ook de apparaat-twin met 1 KB nettoladingen elke vier uur. De oplossing terug beëindigen, één keer per dag, leest de twin 14 KB apparaat en wordt deze bijgewerkt met 512-byte-nettoladingen configuraties wijzigen.

Het apparaat verbruikt:

* 25 (100 KB/4 KB) berichten * 24 uur voor apparaat-naar-cloud-berichten.
* Twee berichten (1 KB/0,5 KB) * zes keer per dag voor apparaatupdates twin.

Deze berekening biedt een totaal van 612 berichten per dag.

De back-end oplossing verbruikt 28 berichten (14 KB/0,5 KB) lezen van het apparaat twin, plus een bericht bij te werken, voor een totaal van 29 berichten.

Verbruiken 641 berichten per dag in totaal wordt het apparaat en de back-end oplossing.


[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-message-size]: iot-hub-devguide-messages-construct.md
